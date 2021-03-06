---
title: "Guide pratique pour rechercher des phrases qui contiennent un groupe de mots spécifié (LINQ) (C#)"
ms.custom: 
ms.date: 07/20/2015
ms.prod: .net
ms.reviewer: 
ms.suite: 
ms.technology: devlang-csharp
ms.topic: article
ms.assetid: 0724b429-4b87-4d26-a7b1-409358f3fc20
caps.latest.revision: "3"
author: BillWagner
ms.author: wiwagn
ms.openlocfilehash: befc93a21388a87fdfd2416349b1310e82378f18
ms.sourcegitcommit: bd1ef61f4bb794b25383d3d72e71041a5ced172e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/18/2017
---
# <a name="how-to-query-for-sentences-that-contain-a-specified-set-of-words-linq-c"></a>Guide pratique pour rechercher des phrases qui contiennent un groupe de mots spécifié (LINQ) (C#)
Cet exemple montre comment rechercher dans un fichier texte les phrases qui contiennent des correspondances pour chaque ensemble de mots spécifié. Même si le tableau des termes de recherche est codé en dur dans cet exemple, il pourrait aussi être rempli dynamiquement lors de l’exécution. Dans cet exemple, la requête retourne les phrases qui contiennent les mots « Historically », « data » et « integrated ».  
  
## <a name="example"></a>Exemple  
  
```csharp  
class FindSentences  
{  
    static void Main()  
    {  
        string text = @"Historically, the world of data and the world of objects " +  
        @"have not been well integrated. Programmers work in C# or Visual Basic " +  
        @"and also in SQL or XQuery. On the one side are concepts such as classes, " +  
        @"objects, fields, inheritance, and .NET Framework APIs. On the other side " +  
        @"are tables, columns, rows, nodes, and separate languages for dealing with " +  
        @"them. Data types often require translation between the two worlds; there are " +  
        @"different standard functions. Because the object world has no notion of query, a " +  
        @"query can only be represented as a string without compile-time type checking or " +  
        @"IntelliSense support in the IDE. Transferring data from SQL tables or XML trees to " +  
        @"objects in memory is often tedious and error-prone.";  
  
        // Split the text block into an array of sentences.  
        string[] sentences = text.Split(new char[] { '.', '?', '!' });  
  
        // Define the search terms. This list could also be dynamically populated at runtime.  
        string[] wordsToMatch = { "Historically", "data", "integrated" };  
  
        // Find sentences that contain all the terms in the wordsToMatch array.  
        // Note that the number of terms to match is not specified at compile time.  
        var sentenceQuery = from sentence in sentences  
                            let w = sentence.Split(new char[] { '.', '?', '!', ' ', ';', ':', ',' },  
                                                    StringSplitOptions.RemoveEmptyEntries)  
                            where w.Distinct().Intersect(wordsToMatch).Count() == wordsToMatch.Count()  
                            select sentence;  
  
        // Execute the query. Note that you can explicitly type  
        // the iteration variable here even though sentenceQuery  
        // was implicitly typed.   
        foreach (string str in sentenceQuery)  
        {  
            Console.WriteLine(str);  
        }  
  
        // Keep the console window open in debug mode.  
        Console.WriteLine("Press any key to exit");  
        Console.ReadKey();  
    }  
}  
/* Output:  
Historically, the world of data and the world of objects have not been well integrated  
*/  
```  
  
 La requête fractionne d’abord le texte en phrases, puis fractionne les phrases en tableaux de chaînes contenant chacun des mots. Pour chacun de ces tableaux, la méthode <xref:System.Linq.Enumerable.Distinct%2A> supprime tous les mots en double, puis la requête effectue une opération <xref:System.Linq.Enumerable.Intersect%2A> sur le tableau de mots et le tableau `wordsToMatch`. Si le nombre de l’intersection est identique à celui du tableau `wordsToMatch`, cela signifie que tous les mots ont été trouvés et la phrase d’origine est donc retournée.  
  
 Dans l’appel à <xref:System.String.Split%2A>, les signes de ponctuation sont utilisés comme séparateurs pour être supprimés de la chaîne. Si vous ne l’avez pas fait, vous pourriez avoir, par exemple, la chaîne « Historically, », qui ne correspondrait pas au mot « Historically » du tableau `wordsToMatch`. Vous devrez peut-être utiliser des séparateurs supplémentaires, selon le type des signes de ponctuation qui se trouvent dans le texte source.  
  
## <a name="compiling-the-code"></a>Compilation du code  
 Créez un projet qui cible le .NET Framework version 3.5 ou version ultérieure, avec une référence à System.Core.dll et des directives `using` pour les espaces de noms System.Linq et System.IO.  
  
## <a name="see-also"></a>Voir aussi  
 [LINQ et chaînes (C#)](../../../../csharp/programming-guide/concepts/linq/linq-and-strings.md)
