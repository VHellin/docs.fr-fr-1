---
title: "&lt;remarks&gt; (Guide de programmation&#160;C#) | Microsoft Docs"
ms.date: "2015-07-20"
ms.prod: ".net"
ms.technology: 
  - "devlang-csharp"
ms.topic: "article"
f1_keywords: 
  - "remarks"
  - "<remarks>"
dev_langs: 
  - "CSharp"
helpviewer_keywords: 
  - "<remarks> (balise XML C#)"
  - "remarks (balise XML C#)"
ms.assetid: f8641391-31f3-4735-af7a-c502a5b6a251
caps.latest.revision: 15
author: "BillWagner"
ms.author: "wiwagn"
caps.handback.revision: 13
---
# &lt;remarks&gt; (Guide de programmation&#160;C#)
## Syntaxe  
  
```  
<remarks>description</remarks>  
```  
  
#### Paramètres  
 `Description`  
 Description du membre.  
  
## Notes  
 La balise \<remarks\> permet d'ajouter des informations sur un type et de compléter les informations spécifiées par [\<summary\>](../../../csharp/programming-guide/xmldoc/summary.md).  Ces informations sont affichées dans [Object Browser Window](http://msdn.microsoft.com/fr-fr/3c7f1673-1f0d-41b1-94ca-a3dcfcb82cda).  
  
 Compilez avec [\/doc](../../../csharp/language-reference/compiler-options/doc-compiler-option.md) pour traiter les commentaires de documentation et les placer dans un fichier.  
  
## Exemple  
 [!code-cs[csProgGuideDocComments#9](../../../csharp/programming-guide/xmldoc/codesnippet/CSharp/remarks_1.cs)]  
  
## Voir aussi  
 [Guide de programmation C\#](../../../csharp/programming-guide/index.md)   
 [Balises recommandées pour les commentaires de documentation](../../../csharp/programming-guide/xmldoc/recommended-tags-for-documentation-comments.md)