---
title: "Poison Message Handling in MSMQ 4.0 | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: ec8d59e3-9937-4391-bb8c-fdaaf2cbb73e
caps.latest.revision: 18
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 18
---
# Poison Message Handling in MSMQ 4.0
이 샘플에서는 서비스에서 포이즌 메시지 처리를 수행하는 방법을 보여 줍니다.  이 샘플은 [트랜잭션된 MSMQ 바인딩](../../../../docs/framework/wcf/samples/transacted-msmq-binding.md) 샘플을 기반으로 합니다.  이 샘플에서는 `netMsmqBinding`을 사용합니다.  이 서비스는 자체적으로 호스트되는 콘솔 응용 프로그램으로서 이를 사용하여 서비스에서 대기된 메시지를 받는 것을 볼 수 있습니다.  
  
 대기 중인 통신에서 클라이언트는 큐를 사용하여 서비스와 통신합니다.  좀더 정확하게 말하면 클라이언트는 큐에 메시지를 보내고,  서비스는 큐에서 보낸 메시지를 받습니다.  따라서 서비스와 클라이언트가 동시에 실행되고 있지 않더라도 큐를 사용하여 통신할 수 있습니다.  
  
 포이즌 메시지는 메시지를 읽는 서비스가 메시지를 처리할 수 없어 메시지를 읽는 트랜잭션을 종료할 때 큐에서 반복적으로 읽는 메시지입니다.  이런 경우 메시지가 다시 시도됩니다.  이론적으로는, 메시지에 문제가 있는 경우 이런 현상이 계속해서 발생할 수 있지만  실제로는 트랜잭션을 사용하여 큐에서 읽고 서비스 작업을 호출하는 경우에만 발생할 수 있습니다.  
  
 MSMQ의 버전에 따라 NetMsmqBinding에서는 포이즌 메시지의 제한적 검색부터 완전한 검색까지 지원합니다.  포이즌 메시지로 검색된 메시지는 몇 가지 방법으로 처리할 수 있습니다.  또한 MSMQ의 버전에 따라 NetMsmqBinding에서는 포이즌 메시지의 제한된 처리부터 전체 처리까지를 지원합니다.  
  
 이 샘플에서는 [!INCLUDE[ws2003](../../../../includes/ws2003-md.md)] 및 [!INCLUDE[wxp](../../../../includes/wxp-md.md)] 플랫폼에서 제공되는 제한된 포이즌 기능 및 [!INCLUDE[wv](../../../../includes/wv-md.md)]에서 제공되는 전체 포이즌 기능을 보여 줍니다.  두 샘플의 목표는 큐에서 포이즌 메시지를 다른 큐로 이동하여 포이즌 메시지 서비스에 의해 처리되도록 하는 것입니다.  
  
## MSMQ v4.0 Poison Handling 샘플  
 [!INCLUDE[wv](../../../../includes/wv-md.md)]의 MSMQ에서는 포이즌 메시지를 저장하는 데 사용할 수 있는 포이즌 하위 큐 기능을 제공합니다.  이 샘플에서는 [!INCLUDE[wv](../../../../includes/wv-md.md)]를 사용하여 포이즌 메시지를 처리하는 가장 좋은 방법을 보여 줍니다.  
  
 [!INCLUDE[wv](../../../../includes/wv-md.md)]의 포이즌 메시지 검색은 매우 정교합니다.  검색에 도움이 되는 속성은 세 가지입니다.  <xref:System.ServiceModel.MsmqBindingBase.ReceiveRetryCount%2A>는 큐에서 지정된 메시지를 다시 읽고 응용 프로그램으로 디스패치하여 처리할 수 있는 횟수입니다.  메시지를 응용 프로그램으로 디스패치할 수 없어 큐에 다시 배치하거나 응용 프로그램이 서비스 작업에서 트랜잭션을 롤백하는 경우에 큐의 메시지를 다시 읽을 수 있습니다.  <xref:System.ServiceModel.MsmqBindingBase.MaxRetryCycles%2A>는 메시지를 재시도 큐로 이동하는 횟수입니다.  <xref:System.ServiceModel.MsmqBindingBase.ReceiveRetryCount%2A>에 도달하면 메시지가 재시도 큐로 이동합니다.  <xref:System.ServiceModel.MsmqBindingBase.RetryCycleDelay%2A> 속성은 메시지가 재시도 큐에서 다시 기본 큐로 이동한 후의 시간 지연입니다.  <xref:System.ServiceModel.MsmqBindingBase.ReceiveRetryCount%2A>는 0으로 다시 설정되고,  메시지가 다시 시도됩니다.  메시지를 읽으려는 시도가 모두 실패하면 메시지가 포이즌으로 표시됩니다.  
  
 메시지가 포이즌으로 표시되면 메시지는 <xref:System.ServiceModel.MsmqBindingBase.ReceiveErrorHandling%2A> 열거의 설정에 따라 처리됩니다.  반복하려는 경우 가능한 값은 다음과 같습니다.  
  
-   Fault\(기본값\): 수신기와 서비스 호스트에 오류를 발생시킵니다.  
  
-   Drop: 메시지를 제거합니다.  
  
-   Move: 메시지를 포이즌 메시지 하위 큐로 이동합니다.  이 값은 [!INCLUDE[wv](../../../../includes/wv-md.md)]에서만 사용할 수 있습니다.  
  
-   Reject: 메시지를 보낸 사람의 배달 못 한 편지 큐로 돌려보내는 방법으로 메시지를 거부합니다.  이 값은 [!INCLUDE[wv](../../../../includes/wv-md.md)]에서만 사용할 수 있습니다.  
  
 이 샘플에서는 포이즌 메시지에 `Move` 처리를 사용하는 방법을 보여 줍니다.  `Move`를 사용하면 메시지가 포이즌 하위 큐로 이동합니다.  
  
 서비스 계약은 `IOrderProcessor`이며, 이는 큐에 사용하기에 적합한 단방향 서비스를 정의합니다.  
  
```  
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples")]  
public interface IOrderProcessor  
{  
    [OperationContract(IsOneWay = true)]  
    void SubmitPurchaseOrder(PurchaseOrder po);  
}  
  
```  
  
 서비스 작업에는 주문을 처리하고 있다는 메시지가 표시됩니다.  포이즌 메시지 기능을 보여 주기 위해 `SubmitPurchaseOrder` 서비스 작업에서는 서비스를 임의로 호출하여 트랜잭션을 롤백하는 예외를 throw합니다.  그러면 메시지는 큐로 돌아가서,  포이즌 메시지로 표시됩니다.  구성은 포이즌 메시지를 포이즌 하위 큐로 이동하도록 설정됩니다.  
  
```  
  
// Service class that implements the service contract.  
// Added code to write output to the console window.  
public class OrderProcessorService : IOrderProcessor  
{  
    static Random r = new Random(137);  
  
    [OperationBehavior(TransactionScopeRequired = true, TransactionAutoComplete = true)]  
    public void SubmitPurchaseOrder(PurchaseOrder po)  
    {  
  
        int randomNumber = r.Next(10);  
  
        if (randomNumber % 2 == 0)  
        {  
            Orders.Add(po);  
            Console.WriteLine("Processing {0} ", po);  
        }  
        else  
        {  
            Console.WriteLine("Aborting transaction, cannot process purchase order: " + po.PONumber);  
            Console.WriteLine();  
            throw new Exception("Cannot process purchase order: " + po.PONumber);  
        }  
    }  
  
    public static void OnServiceFaulted(object sender, EventArgs e)   
    {  
        Console.WriteLine("Service Faulted");  
    }  
  
    // Host the service within this EXE console application.  
    public static void Main()  
    {  
        // Get MSMQ queue name from app settings in configuration.  
        string queueName = ConfigurationManager.AppSettings["queueName"];  
  
        // Create the transacted MSMQ queue if necessary.  
        if (!System.Messaging.MessageQueue.Exists(queueName))  
            System.Messaging.MessageQueue.Create(queueName, true);  
  
        // Get the base address that is used to listen for WS-MetaDataExchange requests.  
        // This is useful to generate a proxy for the client.  
        string baseAddress = ConfigurationManager.AppSettings["baseAddress"];  
  
        // Create a ServiceHost for the OrderProcessorService type.  
        ServiceHost serviceHost = new ServiceHost(typeof(OrderProcessorService), new Uri(baseAddress));  
  
        // Hook on to the service host faulted events.  
        serviceHost.Faulted += new EventHandler(OnServiceFaulted);  
  
        // Open the ServiceHostBase to create listeners and start listening for messages.  
        serviceHost.Open();  
  
        // The service can now be accessed.  
        Console.WriteLine("The service is ready.");  
        Console.WriteLine("Press <ENTER> to terminate service.");  
        Console.WriteLine();  
        Console.ReadLine();  
  
        if(serviceHost.State != CommunicationState.Faulted) {  
            serviceHost.Close();  
        }  
  
    }  
}  
```  
  
 서비스 구성에는 다음 구성 파일에 표시된 `receiveRetryCount`, `maxRetryCycles`, `retryCycleDelay` 및 `receiveErrorHandling`와 같은 포이즌 메시지 속성이 포함됩니다.  
  
```  
<?xml version="1.0" encoding="utf-8" ?>  
<configuration>  
  <appSettings>  
    <!-- Use appSetting to configure MSMQ queue name. -->  
    <add key="queueName" value=".\private$\ServiceModelSamplesPoison" />  
    <add key="baseAddress" value="http://localhost:8000/orderProcessor/poisonSample"/>  
  </appSettings>  
  <system.serviceModel>  
    <services>  
      <service   
              name="Microsoft.ServiceModel.Samples.OrderProcessorService">  
        <!-- Define NetMsmqEndpoint -->  
        <endpoint address="net.msmq://localhost/private/ServiceModelSamplesPoison"  
                  binding="netMsmqBinding"  
                  bindingConfiguration="PoisonBinding"   
                  contract="Microsoft.ServiceModel.Samples.IOrderProcessor" />  
      </service>  
    </services>  
  
    <bindings>  
      <netMsmqBinding>  
        <binding name="PoisonBinding"   
                 receiveRetryCount="0"  
                 maxRetryCycles="1"  
                 retryCycleDelay="00:00:05"                        
                 receiveErrorHandling="Move">  
        </binding>  
      </netMsmqBinding>  
    </bindings>  
  </system.serviceModel>  
</configuration>  
```  
  
## 포이즌 메시지 큐의 메시지 처리  
 포이즌 메시지 서비스에서는 최종 포이즌 메시지 큐로부터 메시지를 읽어 처리합니다.  
  
 포이즌 메시지 큐의 메시지는 메시지를 처리 중인 서비스로 주소가 지정되는 메시지입니다. 이 서비스는 포이즌 메시지 서비스 끝점과 다를 수 있습니다.  따라서 포이즌 메시지 서비스가 큐에서 메시지를 읽으면 [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] 채널 계층은 끝점에서 불일치를 발견하고 메시지를 디스패치하지 않습니다.  이 경우 메시지의 주소는 주문 처리 서비스로 지정되지만 포이즌 메시지 서비스에서 메시지를 받게 됩니다.  메시지의 주소가 다른 끝점으로 지정되는 경우에도 메시지를 계속 받으려면 `ServiceBehavior`를 추가하여 메시지 주소가 지정된 서비스 끝점과 일치하는 주소를 필터링해야 합니다.  포이즌 메시지 큐에서 읽은 메시지를 성공적으로 처리하려면 이러한 구성이 필요합니다.  
  
 포이즌 메시지 서비스 구현 자체는 서비스 구현과 매우 유사합니다.  이 구현에서는 계약을 구현하고 주문을 처리합니다.  코드 예제는 다음과 같습니다.  
  
```  
// Service class that implements the service contract.  
// Added code to write output to the console window.  
[ServiceBehavior(AddressFilterMode=AddressFilterMode.Any)]  
public class OrderProcessorService : IOrderProcessor  
{  
    [OperationBehavior(TransactionScopeRequired = true, TransactionAutoComplete = true)]  
    public void SubmitPurchaseOrder(PurchaseOrder po)  
    {  
        Orders.Add(po);  
        Console.WriteLine("Processing {0} ", po);  
    }  
  
    public static void OnServiceFaulted(object sender, EventArgs e)   
    {  
        Console.WriteLine("Service Faulted...exiting app");  
        Environment.Exit(1);  
    }  
  
    // Host the service within this EXE console application.  
    public static void Main()  
    {  
  
        // Create a ServiceHost for the OrderProcessorService type.  
        ServiceHost serviceHost = new ServiceHost(typeof(OrderProcessorService));  
  
        // Hook on to the service host faulted events.  
        serviceHost.Faulted += new EventHandler(OnServiceFaulted);  
  
        serviceHost.Open();  
  
        // The service can now be accessed.  
        Console.WriteLine("The poison message service is ready.");  
        Console.WriteLine("Press <ENTER> to terminate service.");  
        Console.WriteLine();  
        Console.ReadLine();  
  
        // Close the ServiceHostBase to shutdown the service.  
        if(serviceHost.State != CommunicationState.Faulted)  
        {  
            serviceHost.Close();  
        }  
    }  
  
```  
  
 주문 큐에서 메시지를 읽는 주문 처리 서비스와 달리 포이즌 메시지 서비스는 포이즌 하위 큐에서 메시지를 읽습니다.  포이즌 큐는 기본 큐의 하위 큐로서 "poison"이라는 이름이 지정되고 MSMQ에서 자동으로 생성됩니다.  이 큐에 액세스하려면 다음 샘플 구성처럼 기본 큐 이름, ";" 및 하위 큐 이름\(여기서는 \-"poison"\)을 차례로 입력합니다.  
  
> [!NOTE]
>  MSMQ v3.0 샘플에서 포이즌 큐 이름은 하위 큐가 아닌 메시지를 이동한 큐입니다.  
  
```  
  
<?xml version="1.0" encoding="utf-8" ?>  
<configuration>  
  <system.serviceModel>  
    <services>  
      <service name="Microsoft.ServiceModel.Samples.OrderProcessorService">  
        <!-- Define NetMsmqEndpoint -->  
        <endpoint address="net.msmq://localhost/private/ServiceModelSamplesPoison;poison"  
                  binding="netMsmqBinding"  
                  contract="Microsoft.ServiceModel.Samples.IOrderProcessor" >  
        </endpoint>  
      </service>  
    </services>  
  
  </system.serviceModel>  
</configuration>  
  
```  
  
 샘플을 실행하면 클라이언트, 서비스 및 포이즌 메시지 서비스 동작이 콘솔 창에 표시됩니다.  서비스에서 클라이언트가 보내는 메시지를 받는 것을 볼 수 있습니다.  서비스를 종료하려면 각 콘솔 창에서 Enter 키를 누릅니다.  
  
 서비스가 실행되어 주문 처리를 시작하고 임의로 처리를 종료하기 시작합니다.  주문을 처리했다는 메시지가 나타나면 클라이언트를 다시 실행하여 서비스에서 실제로 메시지를 종료한 것이 확인될 때까지 다른 메시지를 보낼 수 있습니다.  구성된 포이즌 설정에 따라 최종 포이즌 큐로 이동하기 전에 메시지 처리가 한 번 시도됩니다.  
  
```  
  
The service is ready.  
Press <ENTER> to terminate service.  
  
Processing Purchase Order: 0f063b71-93e0-42a1-aa3b-bca6c7a89546  
        Customer: somecustomer.com  
        OrderDetails  
                Order LineItem: 54 of Blue Widget @unit price: $29.99  
                Order LineItem: 890 of Red Widget @unit price: $45.89  
        Total cost of this order: $42461.56  
        Order status: Pending  
  
Processing Purchase Order: 5ef9a4fa-5a30-4175-b455-2fb1396095fa  
        Customer: somecustomer.com  
        OrderDetails  
                Order LineItem: 54 of Blue Widget @unit price: $29.99  
                Order LineItem: 890 of Red Widget @unit price: $45.89  
        Total cost of this order: $42461.56  
        Order status: Pending  
  
Aborting transaction, cannot process purchase order: 23e0b991-fbf9-4438-a0e2-20adf93a4f89  
```  
  
 포이즌 메시지 서비스를 시작하여 포이즌 큐에서 포이즌 메시지를 읽습니다.  이 예제에서는 포이즌 메시지 서비스가 메시지를 읽고 처리합니다.  종료되어 포이즌 메시지가 된 구매 주문을 포이즌 메시지 서비스에서 읽는 것을 확인할 수 있습니다.  
  
```  
The service is ready.  
Press <ENTER> to terminate service.  
  
Processing Purchase Order: 23e0b991-fbf9-4438-a0e2-20adf93a4f89  
        Customer: somecustomer.com  
        OrderDetails  
                Order LineItem: 54 of Blue Widget @unit price: $29.99  
                Order LineItem: 890 of Red Widget @unit price: $45.89  
        Total cost of this order: $42461.56  
        Order status: Pending  
  
```  
  
#### 샘플을 설치, 빌드 및 실행하려면  
  
1.  [Windows Communication Foundation 샘플의 일회 설치 절차](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md)를 수행했는지 확인합니다.  
  
2.  서비스가 처음 실행되는 경우 서비스에서는 큐가 있는지 확인하고  큐가 없으면 큐를 만듭니다.  서비스를 처음 실행하여 큐를 만들거나 MSMQ 큐 관리자를 통해 큐를 만들 수 있습니다.  Windows 2008에서 큐를 만들려면 다음 단계를 수행하세요.  
  
    1.  [!INCLUDE[vs_current_long](../../../../includes/vs-current-long-md.md)]에서 서버 관리자를 엽니다.  
  
    2.  **기능** 탭을 확장합니다.  
  
    3.  **개인 메시지 큐**를 마우스 오른쪽 단추로 클릭하고 **새로 만들기**, **개인 큐**를 차례로 선택합니다.  
  
    4.  **트랜잭션** 상자를 선택합니다.  
  
    5.  `ServiceModelSamplesTransacted`를 새 큐의 이름으로 입력합니다.  
  
3.  C\# 또는 Visual Basic .NET 버전의 솔루션을 빌드하려면 [Windows Communication Foundation 샘플 빌드](../../../../docs/framework/wcf/samples/building-the-samples.md)의 지침을 따릅니다.  
  
4.  단일 컴퓨터 또는 다중 컴퓨터 구성에서 샘플을 실행하려면 localhost 대신 실제 호스트 이름을 반영하도록 큐 이름을 변경하고 [Windows Communication Foundation 샘플 실행](../../../../docs/framework/wcf/samples/running-the-samples.md)의 지침을 따릅니다.  
  
 기본적으로 `netMsmqBinding` 바인딩 전송을 사용하여 보안이 설정됩니다.  `MsmqAuthenticationMode` 및 `MsmqProtectionLevel` 속성은 모두 전송 보안의 형식을 결정합니다.  기본적으로 인증 모드는 `Windows`로 설정되고 보호 수준은 `Sign`으로 설정됩니다.  MSMQ에서 인증 및 서명 기능을 제공하려면 도메인에 속해 있어야 합니다.  도메인에 속하지 않은 컴퓨터에서 이 샘플을 실행할 경우 "사용자의 내부 메시지 큐 인증서가 없습니다."라는 오류 메시지가 표시됩니다.  
  
#### 작업 그룹에 가입된 컴퓨터에서 샘플을 실행하려면  
  
1.  컴퓨터가 도메인에 속하지 않는 경우 다음 샘플 구성과 같이 인증 모드와 보호 수준을 `None`으로 설정하여 전송 보안을 해제합니다.  
  
    ```  
    <bindings>  
        <netMsmqBinding>  
            <binding name="TransactedBinding">  
                <security mode="None"/>  
            </binding>  
        </netMsmqBinding>  
    </bindings>  
    ```  
  
     끝점의 bindingConfiguration 특성을 설정하여 끝점이 바인딩에 연결되어 있는지 확인합니다.  
  
2.  샘플을 실행하기 전에 PoisonMessageServer, 서버 및 클라이언트에서 구성을 변경해야 합니다.  
  
    > [!NOTE]
    >  `security mode`를 `None`으로 설정하는 것은 `MsmqAuthenticationMode`, `MsmqProtectionLevel` 및 `Message` 보안을 `None`으로 설정하는 것과 같습니다.  
  
3.  메타데이터 교환을 작동하기 위해 http 바인딩을 사용하여 URL을 등록합니다.  이렇게 하려면 권한이 높은 명령 창에서 서비스를 실행해야 합니다.  그렇지 않으면 "처리되지 않은 예외: System.ServiceModel.AddressAccessDeniedException: HTTP가 URL http:\/\/\+:8000\/ServiceModelSamples\/service\/을\(를\) 등록할 수 없습니다.  사용자의 프로세스에 이 네임스페이스에 액세스할 수 있는 권한이 없습니다\(자세한 내용은 http:\/\/go.microsoft.com\/fwlink\/?LinkId\=70353 참조\).  \-\-\-\> System.Net.HttpListenerException: 액세스가 거부되었습니다."와 같은 예외가 발생합니다.  
  
> [!IMPORTANT]
>  컴퓨터에 이 샘플이 이미 설치되어 있을 수도 있습니다.  계속하기 전에 다음\(기본\) 디렉터리를 확인하세요.  
>   
>  `<InstallDrive>:\WF_WCF_Samples`  
>   
>  이 디렉터리가 없으면 [.NET Framework 4용 WCF\(Windows Communication Foundation\) 및 Windows WF\(Workflow Foundation\) 샘플](http://go.microsoft.com/fwlink/?LinkId=150780)\(영문\)로 이동하여 [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] 및 [!INCLUDE[wf1](../../../../includes/wf1-md.md)] 샘플을 모두 다운로드하세요.  이 샘플은 다음 디렉터리에 있습니다.  
>   
>  `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Binding\Net\MSMQ\Poison\MSMQ4`  
  
## 참고 항목