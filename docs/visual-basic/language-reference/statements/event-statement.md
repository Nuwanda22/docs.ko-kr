---
title: "Event 문 | Microsoft 문서"
ms.date: 2015-07-20
ms.prod: .net
ms.reviewer: 
ms.suite: 
ms.technology:
- devlang-visual-basic
ms.topic: article
f1_keywords:
- vb.Event
- vb.Custom
dev_langs:
- VB
helpviewer_keywords:
- Event statement
- declaring events, syntax
- Public keyword, Event statements
- Custom keyword
- declarations, events
- event keyword [Visual Basic]
- WithEvents keyword, Event statements
- events [Visual Basic], declaring
- ByVal keyword, Event statements
- events [Visual Basic], custom
- ByRef keyword, Event statements
- declaring user-defined events
ms.assetid: 306ff8ed-74dd-4b6a-bd2f-e91b17474042
caps.latest.revision: 33
author: stevehoag
ms.author: shoag
translation.priority.ht:
- cs-cz
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pl-pl
- pt-br
- ru-ru
- tr-tr
- zh-cn
- zh-tw
translationtype: Machine Translation
ms.sourcegitcommit: a06bd2a17f1d6c7308fa6337c866c1ca2e7281c0
ms.openlocfilehash: b0ea8074f996622df3cd88a7e87fc1156b63dcaf
ms.lasthandoff: 03/13/2017

---
# <a name="event-statement"></a>Event 문
사용자 정의된 이벤트를 선언합니다.  
  
## <a name="syntax"></a>구문  
  
```  
[ <attrlist> ] [ accessmodifier ] _  
[ Shared ] [ Shadows ] Event eventname[(parameterlist)] _  
[ Implements implementslist ]  
' -or-  
[ <attrlist> ] [ accessmodifier ] _  
[ Shared ] [ Shadows ] Event eventname As delegatename _  
[ Implements implementslist ]  
' -or-  
 [ <attrlist> ] [ accessmodifier ] _  
[ Shared ] [ Shadows ] Custom Event eventname As delegatename _  
[ Implements implementslist ]  
   [ <attrlist> ] AddHandler(ByVal value As delegatename)  
      [ statements ]  
   End AddHandler  
   [ <attrlist> ] RemoveHandler(ByVal value As delegatename)  
      [ statements ]  
   End RemoveHandler  
   [ <attrlist> ] RaiseEvent(delegatesignature)  
      [ statements ]  
   End RaiseEvent  
End Event  
```  
  
## <a name="parts"></a>요소  
  
|파트|설명|  
|---|---|  
|`attrlist`|선택적 요소. 이 이벤트에 적용되는 특성 목록입니다. 여러 특성은 쉼표로 구분합니다. 묶어야는 [특성 목록](../../../visual-basic/language-reference/statements/attribute-list.md) 꺾쇠 괄호에서 ("`<`"및"`>`").|  
|`accessmodifier`|선택적 요소. 이벤트에 액세스할 수 있는 코드를 지정합니다. 다음 중 하나일 수 있습니다.<br /><br /> -   [공용](../../../visual-basic/language-reference/modifiers/public.md)-변수를 선언 하는 요소에 액세스할 수 있는 모든 코드에서 액세스할 수 있습니다.<br />-   [보호 된](../../../visual-basic/language-reference/modifiers/protected.md)-만 코드의 클래스 및 파생된 클래스에서 액세스할 수 있습니다.<br />-   [Friend](../../../visual-basic/language-reference/modifiers/friend.md)-만 같은 어셈블리의 코드에서 액세스할 수 있습니다.<br />-   [개인](../../../visual-basic/language-reference/modifiers/private.md)-변수를 선언 하는 요소에만 코드에 액세스할 수 있습니다.<br /><br /> `Protected Friend`를 지정하면 이벤트의 클래스, 파생 클래스 또는 동일한 어셈블리의 코드에서 액세스하도록 할 수 있습니다.|  
|`Shared`|선택적 요소. 이 이벤트가 클래스 또는 구조체의 특정 인스턴스와 연결되지 않도록 지정합니다.|  
|`Shadows`|선택적 요소. 이 이벤트가 기본 클래스의 이름이 같은 프로그래밍 요소 또는 오버로드된 요소 집합을 다시 선언하고 숨김을 나타냅니다. 모든 종류의 선언된 요소를 다른 종류로 섀도잉할 수 있습니다.<br /><br /> 섀도잉된 요소는 섀도잉 요소에 액세스할 수 없는 위치를 제외하고 해당 요소를 섀도잉하는 파생 클래스 내에서 사용할 수 없습니다. 예를 들어 `Private` 요소가 기본 클래스 요소를 섀도잉하는 경우 `Private` 요소에 액세스할 수 있는 권한이 없는 코드는 기본 클래스 요소에 대신 액세스합니다.|  
|`eventname`|필수 요소. 이벤트의 이름입니다. 표준 변수 명명 규칙을 따릅니다.|  
|`parameterlist`|선택적 요소. 이 이벤트의 매개 변수를 나타내는 지역 변수 목록입니다. 묶어야는 [매개 변수 목록](../../../visual-basic/language-reference/statements/parameter-list.md) 괄호 안에 있습니다.|  
|`Implements`|선택적 요소. 이 이벤트가 인터페이스의 이벤트를 구현함을 나타냅니다.|  
|`implementslist`|`Implements`가 제공된 경우 필수입니다. 구현할 `Sub` 프로시저 목록입니다. 여러 프로시저는 다음과 같이 쉼표로 구분합니다.<br /><br /> *implementedprocedure* [, *implementedprocedure* ...]<br /><br /> 각 `implementedprocedure`에는 다음과 같은 구문과 요소가 있습니다.<br /><br /> `interface`.`definedname`<br /><br /> -   `interface`-필요합니다. 이 프로시저의 포함하는 클래스 또는 구조체에서 구현하는 인터페이스의 이름입니다.<br />-   `Definedname`-필요합니다. 프로시저가 `interface`에 정의되는 이름입니다. 이 이름은 이 프로시저가 정의된 프로시저를 구현하는 데 사용하는 이름인 `name`과 동일하지 않아도 됩니다.|  
|`Custom`|필수 요소. `Custom`으로 선언된 이벤트는 사용자 지정 `AddHandler`, `RemoveHandler` 및 `RaiseEvent` 접근자를 정의해야 합니다.|  
|`delegatename`|선택적 요소. 이벤트 처리기 서명을 지정하는 대리자의 이름입니다.|  
|`AddHandler`|필수 요소. `AddHandler` 접근자를 선언합니다. 이 접근자는 명시적으로 `AddHandler` 문을 사용하거나 암시적으로 `Handles` 절을 사용하여 이벤트 처리기가 추가될 때 실행할 문을 지정합니다.|  
|`End AddHandler`|필수 요소. `AddHandler` 블록을 종료합니다.|  
|`value`|필수 요소. 매개 변수 이름입니다.|  
|`RemoveHandler`|필수 요소. `RemoveHandler` 접근자를 선언합니다. 이 접근자는 `RemoveHandler` 문을 사용하여 이벤트 처리기가 제거될 때 실행할 문을 지정합니다.|  
|`End RemoveHandler`|필수 요소. `RemoveHandler` 블록을 종료합니다.|  
|`RaiseEvent`|필수 요소. `RaiseEvent` 접근자를 선언합니다. 이 접근자는 `RaiseEvent` 문을 사용하여 이벤트가 발생될 때 실행할 문을 지정합니다. 일반적으로 `AddHandler` 및 `RemoveHandler` 접근자에서 유지 관리하는 대리자 목록을 호출합니다.|  
|`End RaiseEvent`|필수 요소. `RaiseEvent` 블록을 종료합니다.|  
|`delegatesignature`|필수 요소. `delegatename` 대리자에 필요한 매개 변수와 일치하는 매개 변수 목록입니다. 묶어야는 [매개 변수 목록](../../../visual-basic/language-reference/statements/parameter-list.md) 괄호 안에 있습니다.|  
|`statements`|선택적 요소. `AddHandler`, `RemoveHandler`, 및 `RaiseEvent` 메서드의 본문을 포함하는 문입니다.|  
|`End Event`|필수 요소. `Event` 블록을 종료합니다.|  
  
## <a name="remarks"></a>설명  
 이벤트가 선언된 다음 `RaiseEvent` 문을 사용하여 이벤트를 발생시킵니다. 일반적인 이벤트는 다음 코드 조각에 표시된 대로 선언하고 발생시킬 수 있습니다.  
  
 [!code-vb[VbVbalrEvents #&13;](../../../visual-basic/language-reference/statements/codesnippet/VisualBasic/event-statement_1.vb)]  
  
> [!NOTE]
>  프로시저의 인수를 선언할 때처럼 이벤트 인수를 선언할 수 있습니다. 단, 이벤트에 명령된 인수, `ParamArray` 인수 또는 `Optional` 인수가 없는 경우는 예외입니다. 이벤트에는 반환 값이 없습니다.  
  
 이벤트를 처리하려면 `Handles` 또는 `AddHandler` 문을 사용하여 이벤트 처리기 서브루틴에 연결해야 합니다. 서브루틴 및 이벤트의 서명이 일치해야 합니다. 공유 이벤트를 처리하려면 `AddHandler` 문을 사용해야 합니다.  
  
 `Event`는 모듈 수준에서만 사용할 수 있습니다. 이 의미는 *선언 컨텍스트* 이벤트 클래스, 구조체, 모듈 또는 인터페이스를 이어야 하며 소스 파일, 네임 스페이스, 프로시저 또는 차단 될 수 없습니다. 자세한 내용은 참조 [선언 컨텍스트 및 기본 액세스 수준](../../../visual-basic/language-reference/statements/declaration-contexts-and-default-access-levels.md)합니다.  
  
 대부분의 경우 이 항목의 구문 섹션에 있는 첫 번째 구문을 이벤트 선언에 사용할 수 있습니다. 그러나 일부 시나리오에서는 이벤트의 세부 동작을 더 많이 제어해야 합니다. 이 항목의 구문 섹션에 있는 마지막 구문에서는 `Custom` 키워드를 사용하여 사용자 지정 이벤트를 정의할 수 있도록 함으로써 해당 제어를 제공합니다. 사용자 지정 이벤트에서는 코드가 이벤트에서 이벤트 처리기를 추가하거나 제거할 때 또는 코드에서 이벤트를 발생시킬 때 수행되는 동작을 정확히 지정합니다. 예를 보려면 [하는 방법: 절약 하기 위해 메모리를 사용자 지정 이벤트 선언](../../../visual-basic/programming-guide/language-features/events/how-to-declare-custom-events-to-conserve-memory.md) 및 [하는 방법: 선언 사용자 지정 이벤트를 방지 차단](../../../visual-basic/programming-guide/language-features/events/how-to-declare-custom-events-to-avoid-blocking.md)합니다.  
  
## <a name="example"></a>예제  
 다음 예제에서는 이벤트를 사용하여 10초부터 0초까지 카운트 다운합니다. 코드는 여러 가지 이벤트 관련 메서드, 속성 및 문을 보여 줍니다. 여기에는 `RaiseEvent` 문이 포함됩니다.  
  
 이벤트를 발생시키는 클래스는 이벤트 소스이고 이벤트를 처리하는 메서드는 이벤트 처리기입니다. 이벤트 소스는 생성되는 이벤트에 대해 여러 개의 처리기를 사용할 수 있습니다. 클래스에서 이벤트를 발생시키면 해당 이벤트는 개체의 해당 인스턴스에 대해 이벤트를 처리하도록 선택한 모든 클래스에서 발생됩니다.  
  
 또한 이 예제에서는 단추(`Button1`)와 텍스트 상자(`TextBox1`)가 있는 폼(`Form1`)을 사용합니다. 단추를 클릭하면 첫 번째 텍스트 상자에 10초부터 0초까지의 카운트 다운이 표시됩니다. 전체 시간(10초)이 경과되면 첫 번째 텍스트 상자에 "Done"이 표시됩니다.  
  
 `Form1`에 대한 코드는 폼의 초기 상태 및 최종 상태를 지정합니다. 또한 이벤트가 발생될 때 실행될 코드도 포함됩니다.  
  
 이 예제를 사용하려면 새 Windows Forms 프로젝트를 엽니다. 그런 다음 `Button1`이라는 단추와 `TextBox1`이라는 텍스트 상자를 `Form1`이라는 기본 폼에 추가합니다. 그런 다음 폼을 마우스 오른쪽 단추로 클릭 하 고 클릭 **코드 보기** 코드 편집기를 엽니다.  
  
 `Form1` 클래스의 선언 섹션에 `WithEvents` 변수를 추가합니다.  
  
 [!code-vb[VbVbalrEvents #&14;](../../../visual-basic/language-reference/statements/codesnippet/VisualBasic/event-statement_2.vb)]  
  
 `Form1`에 대한 코드에 다음 코드를 추가합니다. `Form_Load` 또는 `Button_Click`과 같이 있을 수 있는 모든 중복 프로시저를 대체합니다.  
  
 [!code-vb[VbVbalrEvents #&15;](../../../visual-basic/language-reference/statements/codesnippet/VisualBasic/event-statement_3.vb)]  
  
 앞의 예제를 실행 하 고 저장 단추를 클릭 하는 F5 키를 눌러 **시작**합니다. 첫 번째 텍스트 상자에서 초를 카운트 다운하기 시작합니다. 전체 시간(10초)이 경과되면 첫 번째 텍스트 상자에 "Done"이 표시됩니다.  
  
> [!NOTE]
>  `My.Application.DoEvents` 메서드는 폼과 같은 방식으로 이벤트를 처리하지 않습니다. 폼에서 이벤트를 직접 처리하도록 하려면 다중 스레딩을 사용할 수 있습니다. 자세한 내용은 참조 [스레딩](http://msdn.microsoft.com/library/552f6c68-dbdb-4327-ae36-32cf9063d88c)합니다.  
  
## <a name="see-also"></a>참고 항목  
 [RaiseEvent 문](../../../visual-basic/language-reference/statements/raiseevent-statement.md)   
 [Implements 문](../../../visual-basic/language-reference/statements/implements-statement.md)   
 [이벤트](../../../visual-basic/programming-guide/language-features/events/index.md)   
 [AddHandler 문](../../../visual-basic/language-reference/statements/addhandler-statement.md)   
 [RemoveHandler 문](../../../visual-basic/language-reference/statements/removehandler-statement.md)   
 [핸들](../../../visual-basic/language-reference/statements/handles-clause.md)   
 [Delegate 문](../../../visual-basic/language-reference/statements/delegate-statement.md)   
 [방법: 메모리를 절약 하기 위해 사용자 지정 이벤트 선언](../../../visual-basic/programming-guide/language-features/events/how-to-declare-custom-events-to-conserve-memory.md)   
 [방법: 차단을 방지 하는 사용자 지정 이벤트 선언](../../../visual-basic/programming-guide/language-features/events/how-to-declare-custom-events-to-avoid-blocking.md)   
 [공유](../../../visual-basic/language-reference/modifiers/shared.md)   
 [Shadows](../../../visual-basic/language-reference/modifiers/shadows.md)