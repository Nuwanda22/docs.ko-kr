---
title: "Windows Communication Foundation로 메시지 큐 | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 6d718eb0-9f61-4653-8a75-d2dac8fb3520
caps.latest.revision: 34
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 34
---
# Windows Communication Foundation로 메시지 큐
이 샘플에서는 MSMQ\(메시지 큐\) 응용 프로그램이 [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] 서비스로 MSMQ 메시지를 보내는 방법을 보여 줍니다.이 서비스는 자체적으로 호스팅되는 콘솔 응용 프로그램으로서 이를 사용하여 서비스에서 대기된 메시지를 받는 것을 볼 수 있습니다.  
  
 서비스 계약은 `IOrderProcessor`이고, 이는 큐에 사용하기에 적합한 단방향 서비스를 정의합니다.MSMQ 메시지에는 동작 헤더가 없기 때문에 여러 MSMQ 메시지를 작업 계약에 자동으로 매핑할 수 없습니다.따라서 작업 계약은 하나만 있을 수 있습니다.서비스에 대해 둘 이상의 작업 계약을 정의하려는 경우 응용 프로그램은 디스패치할 작업 계약을 결정하는 데 사용할 수 있는 MSMQ 메시지의 헤더와 관련된 정보\(예: 레이블 또는 correlationID\)를 제공해야 합니다.이에 대해서는 [사용자 지정 Demux](../../../../docs/framework/wcf/samples/custom-demux.md)에서 보여 줍니다.  
  
 MSMQ 메시지에는 작업 계약의 여러 매개 변수에 매핑되는 헤더에 대한 정보는 들어 있지 않습니다.매개 변수의 형식은 <xref:System.ServiceModel.MsmqIntegration.MsmqMessage%601>\(`MsmqMessage<T>`\)이고, 여기에는 기본 MSMQ 메시지가 포함됩니다.<xref:System.ServiceModel.MsmqIntegration.MsmqMessage%601>\(`MsmqMessage<T>`\) 클래스의 "T" 형식은 MSMQ 메시지 본문으로 serialize되는 데이터를 나타냅니다.이 샘플에서는 `PurchaseOrder` 형식이 MSMQ 메시지 본문으로 serialize됩니다.  
  
 다음 샘플 코드에서는 주문 처리 서비스의 서비스 계약을 보여 줍니다.  
  
```  
// Define a service contract.  
[ServiceContract(Namespace = "http://Microsoft.ServiceModel.Samples")]  
[ServiceKnownType(typeof(PurchaseOrder))]  
public interface IOrderProcessor  
{  
    [OperationContract(IsOneWay = true, Action = "*")]  
    void SubmitPurchaseOrder(MsmqMessage<PurchaseOrder> msg);  
}  
  
```  
  
 서비스는 자체 호스팅됩니다.MSMQ를 사용하는 경우에는 사용되는 큐를 미리 만들어야 합니다.수동으로 또는 코드를 통해 이 작업을 수행할 수 있습니다.이 샘플에서 서비스는 큐가 있는지 확인하고 필요한 경우 큐를 만듭니다.큐 이름은 구성 파일에서 읽습니다.  
  
```  
public static void Main()  
{  
    // Get the MSMQ queue name from the application settings in   
    // configuration.  
    string queueName = ConfigurationManager.AppSettings["queueName"];  
    // Create the MSMQ queue if necessary.  
    if (!MessageQueue.Exists(queueName))  
        MessageQueue.Create(queueName, true);  
    …  
}  
  
```  
  
 다음 샘플 코드에서와 같이 서비스는 `OrderProcessorService`를 위한 <xref:System.ServiceModel.ServiceHost>를 만들고 엽니다.  
  
```  
using (ServiceHost serviceHost = new ServiceHost(typeof(OrderProcessorService)))  
{  
    serviceHost.Open();  
    Console.WriteLine("The service is ready.");  
    Console.WriteLine("Press <ENTER> to terminate service.");  
    Console.ReadLine();  
    serviceHost.Close();  
}  
  
```  
  
 다음 샘플 구성에서 보여 주는 것처럼 MSMQ 큐 이름은 구성 파일의 appSettings 섹션에 지정됩니다.  
  
> [!NOTE]
>  큐 이름은 로컬 컴퓨터에 점\(.\)을, 그 경로에는 백슬래시 구분 기호를 사용합니다.[!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] 끝점 주소는 msmq.formatname 체계를 지정하며 로컬 컴퓨터로 localhost를 사용합니다.각 MSMQ 형식 이름 주소 지정 지침의 큐 주소는 msmq.formatname 체계를 따릅니다.  
  
```  
<appSettings>  
    <add key="orderQueueName" value=".\private$\Orders" />  
</appSettings>  
  
```  
  
 다음 샘플 코드에서와 같이 클라이언트 응용 프로그램은 <xref:System.Messaging.MessageQueue.Send%2A> 메서드를 사용하여 지속적인 트랜잭션 메시지를 큐로 보내는 MSMQ 응용 프로그램입니다.  
  
```  
//Connect to the queue.  
MessageQueue orderQueue = new MessageQueue(ConfigurationManager.AppSettings["orderQueueName"]);  
  
// Create the purchase order.  
PurchaseOrder po = new PurchaseOrder();  
po.CustomerId = "somecustomer.com";  
po.PONumber = Guid.NewGuid().ToString();  
  
PurchaseOrderLineItem lineItem1 = new PurchaseOrderLineItem();  
lineItem1.ProductId = "Blue Widget";  
lineItem1.Quantity = 54;  
lineItem1.UnitCost = 29.99F;  
  
PurchaseOrderLineItem lineItem2 = new PurchaseOrderLineItem();  
lineItem2.ProductId = "Red Widget";  
lineItem2.Quantity = 890;  
lineItem2.UnitCost = 45.89F;  
  
po.orderLineItems = new PurchaseOrderLineItem[2];  
po.orderLineItems[0] = lineItem1;  
po.orderLineItems[1] = lineItem2;  
  
// Submit the purchase order.  
Message msg = new Message();  
msg.Body = po;  
//Create a transaction scope.  
using (TransactionScope scope = new TransactionScope(TransactionScopeOption.Required))  
{  
  
    orderQueue.Send(msg, MessageQueueTransactionType.Automatic);  
    // Complete the transaction.  
    scope.Complete();  
  
}  
Console.WriteLine("Placed the order:{0}", po);  
Console.WriteLine("Press <ENTER> to terminate client.");  
Console.ReadLine();  
  
```  
  
 샘플을 실행하면 클라이언트 및 서비스 동작이 서비스 콘솔 창과 클라이언트 콘솔 창에 모두 표시됩니다.서비스에서 클라이언트가 보내는 메시지를 받는 것을 볼 수 있습니다.서비스와 클라이언트를 종료하려면 각 콘솔 창에서 Enter 키를 누릅니다.큐를 사용하므로 클라이언트와 서비스가 동시에 실행 중일 필요는 없습니다.예를 들어, 클라이언트를 실행하고 종료한 다음 서비스를 다시 시작해도 메시지를 수신할 수 있습니다.  
  
### 샘플을 설치, 빌드 및 실행하려면  
  
1.  [Windows Communication Foundation 샘플의 일회 설치 절차](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md)를 수행했는지 확인합니다.  
  
2.  서비스가 처음 실행되는 경우 서비스에서는 큐가 있는지 확인하고큐가 없으면 큐를 만듭니다.서비스를 처음 실행하여 큐를 만들거나 MSMQ 큐 관리자를 통해 큐를 만들 수 있습니다.Windows 2008에서 큐를 만들려면 다음 단계를 수행하십시오.  
  
    1.  [!INCLUDE[vs_current_long](../../../../includes/vs-current-long-md.md)]에서 서버 관리자를 엽니다.  
  
    2.  **기능** 탭을 확장합니다.  
  
    3.  **개인 메시지 큐**를 마우스 오른쪽 단추로 클릭하고 **새로 만들기**, **개인 큐**를 차례로 선택합니다.  
  
    4.  **트랜잭션** 상자를 선택합니다.  
  
    5.  `ServiceModelSamplesTransacted`를 새 큐의 이름으로 입력합니다.  
  
3.  C\# 또는 Visual Basic .NET 버전의 솔루션을 빌드하려면 [Windows Communication Foundation 샘플 빌드](../../../../docs/framework/wcf/samples/building-the-samples.md)의 지침을 따릅니다.  
  
4.  단일 컴퓨터 구성에서 샘플을 실행하려면 [Windows Communication Foundation 샘플 실행](../../../../docs/framework/wcf/samples/running-the-samples.md)의 지침을 따릅니다.  
  
### 다중 컴퓨터 구성에서 샘플을 실행하려면  
  
1.  언어별 폴더의 \\service\\bin\\ 폴더에서 서비스 컴퓨터로 서비스 프로그램 파일을 복사합니다.  
  
2.  언어별 폴더의 \\client\\bin\\ 폴더에서 클라이언트 프로그램 파일을 클라이언트 컴퓨터로 복사합니다.  
  
3.  Client.exe.config 파일에서 orderQueueName을 변경하여 "." 대신 서비스 컴퓨터 이름을 지정합니다.  
  
4.  서비스 컴퓨터의 명령 프롬프트에서 Service.exe를 실행합니다.  
  
5.  클라이언트 컴퓨터의 명령 프롬프트에서 Client.exe를 실행합니다.  
  
> [!IMPORTANT]
>  컴퓨터에 이 샘플이 이미 설치되어 있을 수도 있습니다.계속하기 전에 다음\(기본\) 디렉터리를 확인하십시오.  
>   
>  `<InstallDrive>:\WF_WCF_Samples`  
>   
>  이 디렉터리가 없으면 [Windows Communication Foundation \(WCF\) and Windows Workflow Foundation \(WF\) Samples for .NET Framework 4](http://go.microsoft.com/fwlink/?LinkId=150780)로 이동하여 [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] 및 [!INCLUDE[wf1](../../../../includes/wf1-md.md)] 샘플을 모두 다운로드하십시오.이 샘플은 다음 디렉터리에 있습니다.  
>   
>  `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Binding\MSMQIntegration\MsmqToWcf`  
  
## 참고 항목  
 [WCF의 큐](../../../../docs/framework/wcf/feature-details/queues-in-wcf.md)   
 [방법: WCF 끝점 및 메시지 큐 응용 프로그램과 메시지 교환](../../../../docs/framework/wcf/feature-details/how-to-exchange-messages-with-wcf-endpoints-and-message-queuing-applications.md)   
 [메시지 큐\(영문 페이지일 수 있음\)](http://go.microsoft.com/fwlink/?LinkId=94968)