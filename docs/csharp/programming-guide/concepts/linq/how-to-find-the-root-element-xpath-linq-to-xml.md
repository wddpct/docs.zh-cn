---
title: "如何：查找根元素 (XPath-LINQ to XML) (C#) | Microsoft Docs"
ms.custom: 
ms.date: 2015-07-20
ms.prod: .net
ms.reviewer: 
ms.suite: 
ms.technology:
- devlang-csharp
ms.topic: article
dev_langs:
- CSharp
ms.assetid: 4fd824e0-4d39-429b-b092-f6a5c046ee6c
caps.latest.revision: 3
author: BillWagner
ms.author: wiwagn
translationtype: Human Translation
ms.sourcegitcommit: a06bd2a17f1d6c7308fa6337c866c1ca2e7281c0
ms.openlocfilehash: 1f8fa5eaa88565d5505ef9310cf4fe40ecab44b2
ms.lasthandoff: 03/13/2017


---
# <a name="how-to-find-the-root-element-xpath-linq-to-xml-c"></a>如何：查找根元素 (XPath-LINQ to XML) (C#)
本主题演示如何使用 XPath 和 [!INCLUDE[sqltecxlinq](../../../../csharp/programming-guide/concepts/linq/includes/sqltecxlinq_md.md)] 获取根元素。  
  
 XPath 表达式为：  
  
 `/PurchaseOrders`  
  
## <a name="example"></a>示例  
 此示例查找根元素。  
  
 本示例使用以下 XML 文档：[示例 XML 文件：多个采购订单 (LINQ to XML)](../../../../csharp/programming-guide/concepts/linq/sample-xml-file-multiple-purchase-orders-linq-to-xml.md)。  
  
```csharp  
XDocument po = XDocument.Load("PurchaseOrders.xml");  
  
// LINQ to XML query  
XElement el1 = po.Root;  
  
// XPath expression  
XElement el2 = po.XPathSelectElement("/PurchaseOrders");  
  
if (el1 == el2)  
    Console.WriteLine("Results are identical");  
else  
    Console.WriteLine("Results differ");  
Console.WriteLine(el1.Name);  
```  
  
 该示例产生下面的输出：  
  
```  
Results are identical  
PurchaseOrders  
```  
  
## <a name="see-also"></a>请参阅  
 [针对 XPath 用户的 LINQ to XML (C#)](../../../../csharp/programming-guide/concepts/linq/linq-to-xml-for-xpath-users.md)
