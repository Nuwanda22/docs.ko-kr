---
title: "레이블 | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-wpf"
ms.tgt_pltfrm: ""
ms.topic: "article"
f1_keywords: 
  - "AutoGeneratedOrientationPage"
helpviewer_keywords: 
  - "컨트롤[WPF], 레이블"
  - "Label 컨트롤[WPF]"
ms.assetid: 241c1ce2-60f8-4613-a0ec-9b9bb25fb6af
caps.latest.revision: 66
author: "dotnet-bot"
ms.author: "dotnetcontent"
manager: "wpickett"
caps.handback.revision: 66
---
# 레이블
<xref:System.Windows.Controls.Label> 컨트롤은 대개 [!INCLUDE[TLA#tla_ui](../../../../includes/tlasharptla-ui-md.md)]에서 정보를 제공하는 데 사용됩니다.  원래 <xref:System.Windows.Controls.Label>은 텍스트만 포함했지만 [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)]에서 제공되는 <xref:System.Windows.Controls.Label>이 <xref:System.Windows.Controls.ContentControl>이므로 텍스트뿐만 아니라 <xref:System.Windows.UIElement>도 포함할 수 있습니다.  
  
 <xref:System.Windows.Controls.Label>은 선택키에 대한 기능적 지원과 시각적 지원을 모두 제공합니다.  선택키는 키보드를 통해 <xref:System.Windows.Controls.TextBox>와 같은 컨트롤에 빠르게 액세스하는 데 주로 사용됩니다.  <xref:System.Windows.Controls.Control>에 <xref:System.Windows.Controls.Label>을 할당하려면 사용자가 선택키를 누를 때 포커스를 받아야 하는 컨트롤에 <xref:System.Windows.Controls.Label.Target%2A?displayProperty=fullName> 속성을 설정합니다.  
  
 다음 그림에서는 <xref:System.Windows.Controls.ComboBox>를 대상으로 하는 <xref:System.Windows.Controls.Label> "테마"를 보여 줍니다.  사용자가 선택키를 누르면 <xref:System.Windows.Controls.ComboBox>가 포커스를 받습니다.  자세한 내용은 [How to: Set the Target Property of a Label](http://msdn.microsoft.com/ko-kr/b24c6977-ebcb-4855-a9bb-3fd4435af8f8)을 참조하십시오.  
  
 ![용도별로 레이블 지정된 표시 속성](../../../../docs/framework/wpf/controls/media/labeledby.JPG "LabeledBy")  
  
## 단원 내용  
 [How to: Set the Target Property of a Label](http://msdn.microsoft.com/ko-kr/b24c6977-ebcb-4855-a9bb-3fd4435af8f8)  
  
## 참조  
 <xref:System.Windows.Controls.Label>