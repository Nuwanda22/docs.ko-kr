---
title: "SQL Server 프로그래밍 및 호스트 보호 특성"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology:
- dotnet-clr
ms.tgt_pltfrm: 
ms.topic: article
helpviewer_keywords:
- SQL Server [.NET Framework]
- permission sets, SQL Server
- SQL Server Programming and Host Protection Attributes
- managed code, SQL Server
- reliability [.NET Framework]
- writing reliable code
- hosts, reliability
- host protection attributes
- HostProtectionAttribute class, reliability
ms.assetid: 7dfa36b4-e773-4c75-a3ff-ff1af3ce4c4f
caps.latest.revision: 13
author: mairaw
ms.author: mairaw
manager: wpickett
ms.translationtype: HT
ms.sourcegitcommit: 306c608dc7f97594ef6f72ae0f5aaba596c936e1
ms.openlocfilehash: 3181237540bd7379fd25c8b58c0f2c6f188148ea
ms.contentlocale: ko-kr
ms.lasthandoff: 08/21/2017

---
# <a name="sql-server-programming-and-host-protection-attributes"></a>SQL Server 프로그래밍 및 호스트 보호 특성
SQL Server 호스트에서 관리 코드를 로드 및 실행하려면 코드 액세스 보안과 호스트 리소스 보호 둘 다를 위해 호스트의 요구 사항을 충족해야 합니다.  코드 액세스 보안 요구 사항은 세 가지 SQL Server 권한 집합인 SAFE, EXTERNAL-ACCESS, UNSAFE 중 하나로 지정됩니다. SAFE 또는 EXTERNAL-ACCESS 권한 집합 내에서 코드를 실행하는 경우 <xref:System.Security.Permissions.HostProtectionAttribute> 특성이 적용된 특정 형식이나 멤버를 사용하면 안 됩니다. <xref:System.Security.Permissions.HostProtectionAttribute>는 호스트에서 허용되지 않을 수 있는 형식이나 메서드인 특정 코드 구문을 식별한다는 점에서 보안 권한이 아니라 안정성 보장입니다.  <xref:System.Security.Permissions.HostProtectionAttribute>를 사용하면 호스트의 안정성을 보호하는 데 도움이 되는 프로그래밍 모델이 적용됩니다.  
  
## <a name="host-protection-attributes"></a>호스트 보호 특성  
 호스트 보호 특성은 호스트 프로그래밍 모델에 맞지 않는 형식이나 멤버를 식별하고 다음과 같은 증가하는 안정성 위협 수준을 나타냅니다.  
  
-   달리 유해하지 않습니다.  
  
-   서버에서 관리하는 사용자 코드가 불안정해질 수 있습니다.  
  
-   서버 프로세스 자체가 불안정해질 수 있습니다.  
  
 SQL Server에서는 <xref:System.Security.Permissions.HostProtectionResource> 값 <xref:System.Security.Permissions.HostProtectionResource.SharedState>, <xref:System.Security.Permissions.HostProtectionResource.Synchronization>, <xref:System.Security.Permissions.HostProtectionResource.MayLeakOnAbort> 또는 <xref:System.Security.Permissions.HostProtectionResource.ExternalProcessMgmt>를 지정하는 <xref:System.Security.Permissions.HostProtectionAttribute>가 있는 형식 또는 멤버의 사용을 허용하지 않습니다. 이 때문에 어셈블리에서 공유 상태를 사용하도록 설정하거나, 동기화를 수행하거나, 종료 시 리소스 누수가 발생할 수 있거나, SQL Server 프로세스의 무결성에 영향을 주는 멤버를 호출할 수 없습니다.  
  
### <a name="disallowed-types-and-members"></a>허용되지 않는 형식 및 멤버  
 다음 표에서는 <xref:System.Security.Permissions.HostProtectionResource> 값이 SQL Server에서 허용되지 않는 형식 및 멤버를 식별합니다.  
  
|네임스페이스|형식 또는 멤버|  
|---------------|--------------------|  
|`Microsoft.Win32`|<xref:Microsoft.Win32.PowerModeChangedEventArgs> 클래스<br /><br /> <xref:Microsoft.Win32.PowerModeChangedEventHandler> 대리자<br /><br /> <xref:Microsoft.Win32.SessionEndedEventArgs> 클래스<br /><br /> <xref:Microsoft.Win32.SessionEndedEventHandler> 대리자<br /><br /> <xref:Microsoft.Win32.SessionEndingEventArgs> 클래스<br /><br /> <xref:Microsoft.Win32.SessionEndingEventHandler> 대리자<br /><br /> <xref:Microsoft.Win32.SessionSwitchEventArgs> 클래스<br /><br /> <xref:Microsoft.Win32.SessionSwitchEventHandler> 대리자<br /><br /> <xref:Microsoft.Win32.SystemEvents> 클래스<br /><br /> <xref:Microsoft.Win32.TimerElapsedEventArgs> 클래스<br /><br /> <xref:Microsoft.Win32.TimerElapsedEventHandler> 대리자<br /><br /> <xref:Microsoft.Win32.UserPreferenceChangedEventArgs> 클래스<br /><br /> <xref:Microsoft.Win32.UserPreferenceChangingEventArgs> 클래스|  
|`System.Collections`|<xref:System.Collections.ArrayList.Synchronized%2A?displayProperty=fullName> 메서드<br /><br /> <xref:System.Collections.Hashtable.Synchronized%2A?displayProperty=fullName> 메서드<br /><br /> <xref:System.Collections.Queue.Synchronized%2A?displayProperty=fullName> 메서드<br /><br /> <xref:System.Collections.SortedList.Synchronized%2A?displayProperty=fullName> 메서드<br /><br /> <xref:System.Collections.Stack.Synchronized%2A?displayProperty=fullName> 메서드|  
|`System.ComponentModel`|<xref:System.ComponentModel.AddingNewEventArgs> 클래스<br /><br /> <xref:System.ComponentModel.AddingNewEventHandler> 대리자<br /><br /> <xref:System.ComponentModel.ArrayConverter> 클래스<br /><br /> <xref:System.ComponentModel.AsyncCompletedEventArgs> 클래스<br /><br /> <xref:System.ComponentModel.AsyncCompletedEventHandler> 대리자<br /><br /> <xref:System.ComponentModel.AsyncOperation> 클래스<br /><br /> <xref:System.ComponentModel.AsyncOperationManager> 클래스<br /><br /> <xref:System.ComponentModel.AttributeCollection> 클래스<br /><br /> <xref:System.ComponentModel.BackgroundWorker> 클래스<br /><br /> <xref:System.ComponentModel.BaseNumberConverter> 클래스<br /><br /> <xref:System.ComponentModel.BindingList%601> 클래스<br /><br /> <xref:System.ComponentModel.BooleanConverter> 클래스<br /><br /> <xref:System.ComponentModel.ByteConverter> 클래스<br /><br /> <xref:System.ComponentModel.CancelEventArgs> 클래스<br /><br /> <xref:System.ComponentModel.CancelEventHandler> 대리자<br /><br /> <xref:System.ComponentModel.CharConverter> 클래스<br /><br /> <xref:System.ComponentModel.CollectionChangeEventArgs> 클래스<br /><br /> <xref:System.ComponentModel.CollectionChangeEventHandler> 대리자<br /><br /> <xref:System.ComponentModel.CollectionConverter> 클래스<br /><br /> <xref:System.ComponentModel.ComponentCollection> 클래스<br /><br /> <xref:System.ComponentModel.ComponentConverter> 클래스<br /><br /> <xref:System.ComponentModel.ComponentEditor> 클래스<br /><br /> <xref:System.ComponentModel.ComponentResourceManager> 클래스<br /><br /> <xref:System.ComponentModel.Container> 클래스<br /><br /> <xref:System.ComponentModel.ContainerFilterService> 클래스<br /><br /> <xref:System.ComponentModel.CultureInfoConverter> 클래스<br /><br /> <xref:System.ComponentModel.CustomTypeDescriptor> 클래스<br /><br /> <xref:System.ComponentModel.DateTimeConverter> 클래스<br /><br /> <xref:System.ComponentModel.DecimalConverter> 클래스<br /><br /> <xref:System.ComponentModel.Design.ActiveDesignerEventArgs> 클래스<br /><br /> <xref:System.ComponentModel.Design.ActiveDesignerEventHandler> 대리자<br /><br /> <xref:System.ComponentModel.Design.CheckoutException> 클래스<br /><br /> <xref:System.ComponentModel.Design.CommandID> 클래스<br /><br /> <xref:System.ComponentModel.Design.ComponentChangedEventArgs> 클래스<br /><br /> <xref:System.ComponentModel.Design.ComponentChangedEventHandler> 대리자<br /><br /> <xref:System.ComponentModel.Design.ComponentChangingEventArgs> 클래스<br /><br /> <xref:System.ComponentModel.Design.ComponentChangingEventHandler> 대리자<br /><br /> <xref:System.ComponentModel.Design.ComponentEventArgs> 클래스<br /><br /> <xref:System.ComponentModel.Design.ComponentEventHandler> 대리자<br /><br /> <xref:System.ComponentModel.Design.ComponentRenameEventArgs> 클래스<br /><br /> <xref:System.ComponentModel.Design.ComponentRenameEventHandler> 대리자<br /><br /> <xref:System.ComponentModel.Design.DesignerCollection> 클래스<br /><br /> <xref:System.ComponentModel.Design.DesignerEventArgs> 클래스<br /><br /> <xref:System.ComponentModel.Design.DesignerEventHandler> 대리자<br /><br /> <xref:System.ComponentModel.Design.DesignerOptionService> 클래스<br /><br /> <xref:System.ComponentModel.Design.DesignerTransaction> 클래스<br /><br /> <xref:System.ComponentModel.Design.DesignerTransactionCloseEventArgs> 클래스<br /><br /> <xref:System.ComponentModel.Design.DesignerTransactionCloseEventHandler> 대리자<br /><br /> <xref:System.ComponentModel.Design.DesignerVerb> 클래스<br /><br /> <xref:System.ComponentModel.Design.DesignerVerbCollection> 클래스<br /><br /> <xref:System.ComponentModel.Design.DesigntimeLicenseContext> 클래스<br /><br /> <xref:System.ComponentModel.Design.DesigntimeLicenseContextSerializer> 클래스<br /><br /> <xref:System.ComponentModel.Design.MenuCommand> 클래스<br /><br /> <xref:System.ComponentModel.Design.Serialization.ComponentSerializationService> 클래스<br /><br /> <xref:System.ComponentModel.Design.Serialization.ContextStack> 클래스<br /><br /> <xref:System.ComponentModel.Design.Serialization.DesignerLoader> 클래스<br /><br /> <xref:System.ComponentModel.Design.Serialization.InstanceDescriptor> 클래스<br /><br /> <xref:System.ComponentModel.Design.Serialization.MemberRelationshipService> 클래스<br /><br /> <xref:System.ComponentModel.Design.Serialization.ResolveNameEventArgs> 클래스<br /><br /> <xref:System.ComponentModel.Design.Serialization.ResolveNameEventHandler> 대리자<br /><br /> <xref:System.ComponentModel.Design.Serialization.SerializationStore> 클래스<br /><br /> <xref:System.ComponentModel.Design.ServiceContainer> 클래스<br /><br /> <xref:System.ComponentModel.Design.ServiceCreatorCallback> 대리자<br /><br /> <xref:System.ComponentModel.Design.StandardCommands> 클래스<br /><br /> <xref:System.ComponentModel.Design.StandardToolWindows> 클래스<br /><br /> <xref:System.ComponentModel.DoubleConverter> 클래스<br /><br /> <xref:System.ComponentModel.DoWorkEventArgs> 클래스<br /><br /> <xref:System.ComponentModel.DoWorkEventHandler> 대리자<br /><br /> <xref:System.ComponentModel.EnumConverter> 클래스<br /><br /> <xref:System.ComponentModel.EventDescriptor> 클래스<br /><br /> <xref:System.ComponentModel.EventDescriptorCollection> 클래스<br /><br /> <xref:System.ComponentModel.EventHandlerList> 클래스<br /><br /> <xref:System.ComponentModel.ExpandableObjectConverter> 클래스<br /><br /> <xref:System.ComponentModel.HandledEventArgs> 클래스<br /><br /> <xref:System.ComponentModel.HandledEventHandler> 대리자<br /><br /> <xref:System.ComponentModel.InstanceCreationEditor> 클래스<br /><br /> <xref:System.ComponentModel.Int16Converter> 클래스<br /><br /> <xref:System.ComponentModel.Int32Converter> 클래스<br /><br /> <xref:System.ComponentModel.Int64Converter> 클래스<br /><br /> <xref:System.ComponentModel.InvalidAsynchronousStateException> 클래스<br /><br /> <xref:System.ComponentModel.InvalidEnumArgumentException> 클래스<br /><br /> <xref:System.ComponentModel.ISynchronizeInvoke.BeginInvoke%2A> 메서드<br /><br /> <xref:System.ComponentModel.License> 클래스<br /><br /> <xref:System.ComponentModel.LicenseContext> 클래스<br /><br /> <xref:System.ComponentModel.LicenseException> 클래스<br /><br /> <xref:System.ComponentModel.LicenseManager> 클래스<br /><br /> <xref:System.ComponentModel.LicenseProvider> 클래스<br /><br /> <xref:System.ComponentModel.LicFileLicenseProvider> 클래스<br /><br /> <xref:System.ComponentModel.ListChangedEventArgs> 클래스<br /><br /> <xref:System.ComponentModel.ListChangedEventHandler> 대리자<br /><br /> <xref:System.ComponentModel.ListSortDescription> 클래스<br /><br /> <xref:System.ComponentModel.ListSortDescriptionCollection> 클래스<br /><br /> <xref:System.ComponentModel.MaskedTextProvider> 클래스<br /><br /> <xref:System.ComponentModel.MemberDescriptor> 클래스<br /><br /> <xref:System.ComponentModel.MultilineStringConverter> 클래스<br /><br /> <xref:System.ComponentModel.NestedContainer> 클래스<br /><br /> <xref:System.ComponentModel.NullableConverter> 클래스<br /><br /> <xref:System.ComponentModel.ProgressChangedEventArgs> 클래스<br /><br /> <xref:System.ComponentModel.ProgressChangedEventHandler> 대리자<br /><br /> <xref:System.ComponentModel.PropertyChangedEventArgs> 클래스<br /><br /> <xref:System.ComponentModel.PropertyChangedEventHandler> 대리자<br /><br /> <xref:System.ComponentModel.PropertyDescriptor> 클래스<br /><br /> <xref:System.ComponentModel.PropertyDescriptorCollection> 클래스<br /><br /> <xref:System.ComponentModel.ReferenceConverter> 클래스<br /><br /> <xref:System.ComponentModel.RefreshEventArgs> 클래스<br /><br /> <xref:System.ComponentModel.RefreshEventHandler> 대리자<br /><br /> <xref:System.ComponentModel.RunWorkerCompletedEventArgs> 클래스<br /><br /> <xref:System.ComponentModel.RunWorkerCompletedEventHandler> 대리자<br /><br /> <xref:System.ComponentModel.SByteConverter> 클래스<br /><br /> <xref:System.ComponentModel.SingleConverter> 클래스<br /><br /> <xref:System.ComponentModel.StringConverter> 클래스<br /><br /> <xref:System.ComponentModel.SyntaxCheck> 클래스<br /><br /> <xref:System.ComponentModel.TimeSpanConverter> 클래스<br /><br /> <xref:System.ComponentModel.TypeConverter> 클래스<br /><br /> <xref:System.ComponentModel.TypeDescriptionProvider> 클래스<br /><br /> <xref:System.ComponentModel.TypeDescriptor> 클래스<br /><br /> <xref:System.ComponentModel.TypeListConverter> 클래스<br /><br /> <xref:System.ComponentModel.UInt16Converter> 클래스<br /><br /> <xref:System.ComponentModel.UInt32Converter> 클래스<br /><br /> <xref:System.ComponentModel.UInt64Converter> 클래스<br /><br /> <xref:System.ComponentModel.WarningException> 클래스<br /><br /> <xref:System.ComponentModel.Win32Exception> 클래스|  
|`System.Diagnostics`|<xref:System.Diagnostics.Debug.Listeners%2A?displayProperty=fullName> 속성<br /><br /> <xref:System.Diagnostics.Trace.Listeners%2A?displayProperty=fullName> 속성<br /><br /> <xref:System.Diagnostics.EventLog.SynchronizingObject%2A?displayProperty=fullName> 속성<br /><br /> <xref:System.Diagnostics.ConsoleTraceListener> 클래스<br /><br /> <xref:System.Diagnostics.DefaultTraceListener> 클래스<br /><br /> <xref:System.Diagnostics.DelimitedListTraceListener> 클래스<br /><br /> <xref:System.Diagnostics.EventLogTraceListener> 클래스<br /><br /> <xref:System.Diagnostics.PerformanceCounter> 클래스<br /><br /> <xref:System.Diagnostics.PerformanceCounterCategory> 클래스<br /><br /> <xref:System.Diagnostics.Process> 클래스<br /><br /> <xref:System.Diagnostics.ProcessStartInfo> 클래스<br /><br /> <xref:System.Diagnostics.TextWriterTraceListener> 클래스<br /><br /> <xref:System.Diagnostics.TraceListener> 클래스<br /><br /> <xref:System.Diagnostics.XmlWriterTraceListener> 클래스<br /><br /> <xref:System.Diagnostics.TraceSource.Listeners%2A?displayProperty=fullName> 속성|  
|`System.IO`|<xref:System.IO.Stream.Synchronized%2A?displayProperty=fullName> 메서드<br /><br /> <xref:System.IO.TextReader.Synchronized%2A?displayProperty=fullName> 메서드<br /><br /> <xref:System.IO.TextWriter.Synchronized%2A?displayProperty=fullName> 메서드|  
|`System.Reflection.Emit`|<xref:System.Reflection.Emit.ConstructorBuilder> 클래스<br /><br /> <xref:System.Reflection.Emit.EventBuilder> 클래스<br /><br /> <xref:System.Reflection.Emit.FieldBuilder> 클래스<br /><br /> <xref:System.Reflection.Emit.MethodBuilder> 클래스<br /><br /> <xref:System.Reflection.Emit.CustomAttributeBuilder> 클래스<br /><br /> <xref:System.Reflection.Emit.MethodRental> 클래스<br /><br /> <xref:System.Reflection.Emit.ModuleBuilder> 클래스<br /><br /> <xref:System.Reflection.Emit.PropertyBuilder> 클래스<br /><br /> <xref:System.Reflection.Emit.TypeBuilder> 클래스<br /><br /> <xref:System.Reflection.Emit.UnmanagedMarshal> 클래스|  
|`System.Text`|<xref:System.Text.RegularExpressions.Group.Synchronized%2A?displayProperty=fullName> 메서드<br /><br /> <xref:System.Text.RegularExpressions.Match.Synchronized%2A?displayProperty=fullName> 메서드|  
|`System.Threading`|<xref:System.Threading.AutoResetEvent> 클래스<br /><br /> <xref:System.Threading.EventWaitHandle> 클래스<br /><br /> <xref:System.Threading.ManualResetEvent> 클래스<br /><br /> <xref:System.Threading.Monitor> 클래스<br /><br /> <xref:System.Threading.Mutex> 클래스<br /><br /> <xref:System.Threading.ReaderWriterLock> 클래스<br /><br /> <xref:System.Threading.Semaphore> 클래스<br /><br /> <xref:System.Threading.Thread.AllocateNamedDataSlot%2A?displayProperty=fullName> 메서드<br /><br /> <xref:System.Threading.Thread.BeginCriticalRegion%2A?displayProperty=fullName> 메서드<br /><br /> <xref:System.Threading.Thread.EndCriticalRegion%2A?displayProperty=fullName> 메서드<br /><br /> <xref:System.Threading.Thread.FreeNamedDataSlot%2A?displayProperty=fullName> 메서드<br /><br /> <xref:System.Threading.Thread.GetData%2A?displayProperty=fullName> 메서드<br /><br /> <xref:System.Threading.Thread.Join%2A?displayProperty=fullName> 메서드<br /><br /> <xref:System.Threading.Thread.SetApartmentState%2A?displayProperty=fullName> 메서드<br /><br /> <xref:System.Threading.Thread.SetData%2A?displayProperty=fullName> 메서드<br /><br /> <xref:System.Threading.Thread.SpinWait%2A?displayProperty=fullName> 메서드<br /><br /> <xref:System.Threading.Thread.Start%2A?displayProperty=fullName> 메서드<br /><br /> <xref:System.Threading.Thread.TrySetApartmentState%2A?displayProperty=fullName> 메서드<br /><br /> <xref:System.Threading.ThreadPool> 클래스<br /><br /> <xref:System.Threading.Timer> 클래스|  
|`System.Timers`|<xref:System.Timers.Timer> 클래스|  
|`System.Web.Configuration`|<xref:System.Web.Configuration.MachineKeyValidationConverter> 클래스|  
|`System.Windows.Forms`|<xref:System.Windows.Forms.AutoCompleteStringCollection.SyncRoot%2A?displayProperty=fullName> 속성|  
  
## <a name="sql-server-permission-sets"></a>SQL Server 권한 집합  
 SQL Server에서는 사용자가 데이터베이스에 배포되는 코드에 대한 안정성 요구 사항을 지정할 수 있습니다. 어셈블리를 데이터베이스에 업로드할 때 어셈블리 작성자가 해당 어셈블리에 대해 세 가지 권한 집합(SAFE, EXTERNAL-ACCESS 또는 UNSAFE) 중 하나를 지정할 수 있습니다.  
  
|권한 집합|SAFE|EXTERNAL-ACCESS|UNSAFE|  
|--------------------|----------|----------------------|------------|  
|코드 액세스 보안|실행만|실행 및 외부 리소스 액세스|제한 없음|  
|프로그래밍 모델 제한|예|예|제한 없음|  
|안정성 요구 사항|예|예|아니요|  
|네이티브 코드 호출 기능|아니요|아니요|예|  
  
 SAFE는 허용되는 프로그래밍 모델 측면에서 연결된 제한 사항이 있는 가장 안정적인 보안 모드입니다. SAFE 코드에는 높은 안정성 및 보안 기능이 있습니다. SAFE 어셈블리에는 충분한 실행 권한이 제공되며, 계산을 수행하고, 로컬 데이터베이스에 액세스할 수 있습니다. SAFE 어셈블리는 형식이 안전해야 하며 비관리 코드를 호출할 수 없습니다.  
  
 EXTERNAL-ACCESS는 중급 보안 옵션을 제공하며, 코드에서 데이터베이스 외부의 리소스에 액세스할 수 있도록 허용하지만 여전히 SAFE의 안정성과 보안을 유지합니다.  
  
 UNSAFE는 데이터베이스 관리자만 만들 수 있는 고도로 신뢰할 수 있는 코드에 사용됩니다. 이 신뢰할 수 있는 코드에는 코드 액세스 제한이 없으며 비관리(네이티브) 코드를 호출할 수 있습니다.  
  
 SQL Server는 호스트 수준의 코드 액세스 보안 정책 계층을 사용하여 SQL Server 카탈로그에 저장된 권한 집합에 따라 세 가지 권한 집합 중 하나를 부여하는 호스트 정책을 설정합니다. 데이터베이스 내에서 실행되는 관리 코드는 항상 이러한 코드 액세스 권한 집합 중 하나를 가져옵니다.  
  
## <a name="programming-model-restrictions"></a>프로그래밍 모델 제한  
 SQL Server의 관리 코드에 대한 프로그래밍 모델에는 여러 호출 간에 유지되는 상태를 사용하거나 여러 사용자 세션 간에 상태를 공유할 필요가 없는 함수, 프로시저 및 형식이 필요합니다. 또한 앞에서 설명한 것처럼 공유 상태가 있을 경우 응용 프로그램의 안정성 및 확장성에 영향을 주는 중대한 예외가 발생할 수 있습니다.  
  
 이러한 점을 고려하여 SQL Server에서는 정적 변수와 정적 데이터 멤버의 사용을 허용하지 않습니다. SAFE 및 EXTERNAL-ACCESS 어셈블리의 경우 SQL Server에서 CREATE ASSEMBLY 시간에 어셈블리의 메타데이터를 검사하고, 정적 데이터 멤버와 변수 사용을 발견할 경우 해당 어셈블리를 만들지 못합니다.  
  
## <a name="see-also"></a>참고 항목  
 <xref:System.Security.Permissions.HostProtectionAttribute>   
 <xref:System.Security.Permissions.HostProtectionResource>

