---
title: "如何：在 Visual Basic 中查找具有特定模式的子目录 | Microsoft Docs"
ms.custom: 
ms.date: 2015-07-20
ms.prod: .net
ms.reviewer: 
ms.suite: 
ms.technology:
- devlang-visual-basic
ms.topic: article
dev_langs:
- VB
helpviewer_keywords:
- pattern matching
- folders, finding
ms.assetid: c9265fd1-7483-4150-8b7f-ff642caa939d
caps.latest.revision: 16
author: dotnet-bot
ms.author: dotnetcontent
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
ms.translationtype: Human Translation
ms.sourcegitcommit: 9f5b8ebb69c9206ff90b05e748c64d29d82f7a16
ms.openlocfilehash: 3fc7683c6629e2d94f3acb670810c6632efec094
ms.contentlocale: zh-cn
ms.lasthandoff: 05/22/2017

---
# <a name="how-to-find-subdirectories-with-a-specific-pattern-in-visual-basic"></a>如何：在 Visual Basic 中查找具有特定模式的子目录
<xref:Microsoft.VisualBasic.FileIO.FileSystem.GetDirectories%2A> 方法返回一个只读字符串集合，这些字符串表示目录中子目录的路径名称。 可以使用 `wildCards` 参数来指定特定模式。 如果要在搜索中包含子目录的内容，请将 `searchType` 参数设置为 `SearchOption.SearchAllSubDirectories`。  
  
 如果没有找到与指定模式匹配的目录，则返回一个空集合。  
  
### <a name="to-find-subdirectories-with-a-specific-pattern"></a>查找具有特定模式的子目录  
  
-   使用 `GetDirectories` 方法，并提供要搜索的目录的名称和路径。 以下示例返回名称中包含单词“Logs”的目录结构中的所有目录，并将它们添加到 `ListBox1`。  
  
     [!code-vb[VbVbcnFileAccess#1](../../../../visual-basic/developing-apps/programming/drives-directories-files/codesnippet/VisualBasic/how-to-find-subdirectories-with-a-specific-pattern_1.vb)]  
  
## <a name="robust-programming"></a>可靠编程  
 以下情况可能会导致异常：  
  
-   路径由于以下原因之一而无效：是零长度字符串；仅为空白；包含无效字符；是一个设备路径（开头字符为 \\\\.\\）(<xref:System.ArgumentException>)。  
  
-   路径无效，因为它是 `Nothing` (<xref:System.ArgumentNullException>)。  
  
-   有一个或多个指定通配符是 `Nothing`、空字符串，或仅为空格 (<xref:System.ArgumentNullException>)。  
  
-   `directory` 不存在 (<xref:System.IO.DirectoryNotFoundException>)。  
  
-   `directory` 指向某个现有文件 (<xref:System.IO.IOException>)。  
  
-   路径超过了系统定义的最大长度 (<xref:System.IO.PathTooLongException>)。  
  
-   路径中的文件名或文件夹名包含冒号 (:)，或其格式无效 (<xref:System.NotSupportedException>)。  
  
-   该用户缺少查看该路径所必需的权限 (<xref:System.Security.SecurityException>)。  
  
-   该用户缺少必要的权限 (<xref:System.UnauthorizedAccessException>)。  
  
## <a name="see-also"></a>另请参阅  
 <xref:Microsoft.VisualBasic.FileIO.FileSystem.GetDirectories%2A>   
 [如何：查找具有特定模式的文件](../../../../visual-basic/developing-apps/programming/drives-directories-files/how-to-find-files-with-a-specific-pattern.md)
