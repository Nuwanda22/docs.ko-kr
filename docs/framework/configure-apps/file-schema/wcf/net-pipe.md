---
title: "&lt;net.pipe&gt; | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 6a0f0318-f8f6-466c-9fae-199d7274a82e
caps.latest.revision: 11
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 11
---
# &lt;net.pipe&gt;
명명된 파이프 연결의 수명을 관리하고 명명된 파이프을 통해 수신되는 활성화 요청을 처리하는 Named Pipe Activation Service의 구성 설정을 지정합니다.  
  
## 구문  
  
```  
  
<configuration>  
   <system.serviceModel.activation>  
       <net.pipe maxPendingAccepts="Integer"  
                    maxPendingConnections="Integer"  
          receiveTimeout="TimeSpan">  
          <allowAccounts>  
             // LocalSystem account  
             <add securityIdentifier="S-1-5-18"/>  
             // LocalService account  
             <add securityIdentifier="S-1-5-19"/>  
             // Administrators account  
             <add securityIdentifier="S-1-5-20"/>  
             // Network Service account  
             <add securityIdentifier="S-1-5-32-544" />  
             // IIS_IUSRS account (Vista only)  
             <add securityIdentifier="S-1-5-32-568"/>  
           </allowAccounts>  
       </net.pipe>  
   </system.serviceModel.activation>  
</configuration>  
```  
  
## 형식  
 `Type`  
  
## 특성 및 요소  
 다음 단원에서는 특성, 자식 요소 및 부모 요소에 대해 설명합니다.  
  
### 특성  
  
|특성|설명|  
|--------|--------|  
|`maxPendingAccepts`|공유 서비스에 대한 수신 끝점에서 동시에 수용할 수 있는 활성 스레드의 최대 수를 지정하는 정수입니다.  기본값은 2입니다.|  
|`maxPendingConnections`|디스패치를 대기할 수 있는 최대 연결 수를 지정하는 정수입니다.  기본값은 100입니다.|  
|`receiveTimeout`|프레이밍 데이터를 읽고 내부 연결에서 연결 디스패치를 수행하는 데 대한 제한 시간을 지정하는 <xref:System.Timespan>입니다.  기본값은 "00:00:10"입니다.|  
  
### 자식 요소  
  
|요소|설명|  
|--------|--------|  
|[\<allowAccounts\>](../../../../../docs/framework/configure-apps/file-schema/wcf/allowaccounts.md)|[!INCLUDE[indigo2](../../../../../includes/indigo2-md.md)] 서비스를 호스트하고 공유 서비스에 대한 연결 액세스가 부여된 프로세스의 사용자 계정을 지정하기 위한 `securityIdentifier` 특성이 포함된 구성 요소 컬렉션입니다.|  
  
### 부모 요소  
  
|요소|설명|  
|--------|--------|  
|[\<system.serviceModel.activation\>](../../../../../docs/framework/configure-apps/file-schema/wcf/system-servicemodel-activation.md)|수신기 프로세스 SMSvcHost.exe에 대한 구성 설정을 포함합니다.|  
  
## 참고 항목  
 <xref:System.ServiceModel.Activation.Configuration.NetPipeSection>