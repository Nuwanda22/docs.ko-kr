---
title: "Cleaning Up Unmanaged Resources | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-standard"
ms.tgt_pltfrm: ""
ms.topic: "article"
helpviewer_keywords: 
  - "Close method"
  - "Dispose method"
  - "garbage collector"
  - "garbage collection, unmanaged resources"
  - "cleanup operations"
  - "explicit cleanups"
  - "unmanaged resource cleanup"
  - "Finalize method"
ms.assetid: a17b0066-71c2-4ba4-9822-8e19332fc213
caps.latest.revision: 19
author: "rpetrusha"
ms.author: "ronpet"
manager: "wpickett"
caps.handback.revision: 19
---
# Cleaning Up Unmanaged Resources
사용자 응용 프로그램에서 만들어지는 대부분의 개체에 대해 .NET Framework의 가비지 수집기를 사용하여 메모리 관리를 처리할 수 있습니다.  그러나 관리되지 않는 리소스를 포함하는 개체를 만든 경우에는 응용 프로그램에서 해당 개체의 사용을 마치면 이러한 리소스를 명시적으로 해제해야 합니다.  가장 일반적인 형태의 관리되지 않는 리소스로는 파일, 창, 네트워크 연결 또는 데이터베이스 연결 등의 운영 체제 리소스를 래핑하는 개체를 들 수 있습니다.  가비지 수집기에서는 관리되지 않는 리소스를 캡슐화하는 개체의 수명을 추적할 수 있지만, 관리되지 않는 리소스를 해제하고 정리하는 방법에 대해서는 알 수 없습니다.  
  
 형식이 관리되지 않는 리소스를 사용하는 경우 다음을 수행해야 합니다.  
  
-   [삭제 패턴](../../../docs/standard/design-guidelines/dispose-pattern.md)을 구현합니다.  이를 수행하려면 관리되지 않는 리소스의 명확한 해제를 활성화하기 위해 <xref:System.IDisposable.Dispose%2A?displayProperty=fullName> 구현을 제공해야 합니다.  개체 및 해당 개체에서 사용하는 리소스가 더 이상 필요하지 않은 경우 형식의 소비자가 <xref:System.IDisposable.Dispose%2A>를 호출합니다.  <xref:System.IDisposable.Dispose%2A> 메서드가 관리되지 않는 리소스를 즉시 해제합니다.  
  
-   형식의 소비자가 실수로 <xref:System.IDisposable.Dispose%2A>를 호출하지 않은 경우 해제되는 관리되지 않는 리소스를 제공합니다.  여기에는 두 가지 방법이 있습니다.  
  
    -   SafeHandle을 사용하여 관리되지 않는 리소스를 래핑합니다.  이것이 권장되는 방법입니다.  SafeHandle은 <xref:System.Runtime.InteropServices.SafeHandle?displayProperty=fullName> 클래스에서 파생되며, 강력한 <xref:System.Object.Finalize%2A> 메서드를 포함합니다.  SafeHandle을 사용하면 <xref:System.IDisposable> 인터페이스를 간단하게 구현하고 <xref:System.IDisposable.Dispose%2A?displayProperty=fullName> 구현에서 SafeHandle의 <xref:System.Runtime.InteropServices.SafeHandle.Dispose%2A> 메서드를 호출할 수 있습니다.  <xref:System.IDisposable.Dispose%2A> 메서드가 호출되지 않는 경우 가비지 수집기에 의해 SafeHandle의 종료자가 자동으로 호출됩니다.  
  
         —또는—  
  
    -   <xref:System.Object.Finalize%2A?displayProperty=fullName> 메서드를 재정의합니다.  종료를 사용하면 형식의 소비자가 명확하게 삭제할 수 있도록 <xref:System.IDisposable.Dispose%2A?displayProperty=fullName>를 호출하지 못한 경우 관리되지 않는 리소스가 명확하지 않게 해제됩니다.  그러나 개체 종료가 복잡하고 오류가 발생하기 쉬운 작업일 수 있기 때문에 고유한 종료자를 제공하는 대신 SafeHandle을 사용하는 것이 좋습니다.  
  
 그러면 형식의 소비자가 <xref:System.IDisposable.Dispose%2A?displayProperty=fullName> 구현을 직접 호출하여 관리되지 않는 리소스가 사용하는 메모리를 확보할 수 있습니다.  <xref:System.IDisposable.Dispose%2A> 메서드를 제대로 구현하면 SafeHandle의 <xref:System.Object.Finalize%2A> 메서드 또는 <xref:System.Object.Finalize%2A?displayProperty=fullName> 메서드의 고유한 재정의가 <xref:System.IDisposable.Dispose%2A> 메서드가 호출되지 않는 경우 리소스를 정리하는 보호 기능이 됩니다.  
  
## 단원 내용  
 [Implementing a Dispose Method](../../../docs/standard/garbage-collection/implementing-dispose.md)  
 관리되지 않는 리소스를 해제하기 위해 [삭제 패턴](../../../docs/standard/design-guidelines/dispose-pattern.md)을 구현하는 방법을 설명합니다.  
  
 [Using Objects That Implement IDisposable](../../../docs/standard/garbage-collection/using-objects.md)  
 형식의 소비자가 <xref:System.IDisposable.Dispose%2A> 구현이 호출되었는지 확인하는 방법을 설명합니다.  이를 수행하는 경우 C\# `using` 문 또는 Visual Basic `Using` 문을 사용하는 것이 좋습니다.  
  
## 참조  
 <xref:System.IDisposable?displayProperty=fullName>  
 관리되지 않은 리소스 해제를 위한 <xref:System.IDisposable.Dispose%2A> 메서드를 정의합니다.  
  
 <xref:System.Object.Finalize%2A?displayProperty=fullName>  
 관리되지 않는 리소스가 <xref:System.IDisposable.Dispose%2A> 메서드에 의해 해제되지 않은 경우 개체 종료 기능을 제공합니다.  
  
 <xref:System.GC.SuppressFinalize%2A?displayProperty=fullName>  
 종료를 표시하지 않습니다.  이 메서드는 종료자가 실행되지 않도록 방지하기 위해 통상 `Dispose` 메서드에서 호출됩니다.