---
title: Guide pratique pour interroger le contenu de fichiers dans un dossier (LINQ) (C#) | Microsoft Docs
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
ms.assetid: f5b4dce7-1a34-4eb4-9bf1-60d5bdda264c
caps.latest.revision: 4
author: BillWagner
ms.author: wiwagn
translation.priority.mt:
- cs-cz
- pl-pl
- pt-br
- tr-tr
translationtype: Human Translation
ms.sourcegitcommit: a06bd2a17f1d6c7308fa6337c866c1ca2e7281c0
ms.openlocfilehash: 9e5d5af4e8f0d9fb19e854c62799e67aedc6ff0e
ms.lasthandoff: 03/13/2017

---
# <a name="how-to-query-the-contents-of-files-in-a-folder-linq-c"></a>Guide pratique pour interroger le contenu de fichiers dans un dossier (LINQ) (C#)
Cet exemple montre comment interroger tous les fichiers d’une arborescence de répertoires spécifiée, comment ouvrir chaque fichier et comment inspecter son contenu. Ce type de technique peut être utilisé pour créer des index ou des index inversés à partir du contenu d’une arborescence de répertoires. Dans cet exemple, une recherche de chaîne simple est effectuée. Toutefois, il est possible d’effectuer une recherche avec des critères spéciaux plus complexes à l’aide d’une expression régulière. Pour plus d’informations, consultez [Guide pratique pour combiner des requêtes LINQ avec des expressions régulières (C#)](../../../../csharp/programming-guide/concepts/linq/how-to-combine-linq-queries-with-regular-expressions.md).  
  
## <a name="example"></a>Exemple  
  
```csharp  
class QueryContents  
{  
    public static void Main()  
    {  
        // Modify this path as necessary.  
        string startFolder = @"c:\program files\Microsoft Visual Studio 9.0\";  
  
        // Take a snapshot of the file system.  
        System.IO.DirectoryInfo dir = new System.IO.DirectoryInfo(startFolder);  
  
        // This method assumes that the application has discovery permissions  
        // for all folders under the specified path.  
        IEnumerable<System.IO.FileInfo> fileList = dir.GetFiles("*.*", System.IO.SearchOption.AllDirectories);  
  
        string searchTerm = @"Visual Studio";  
  
        // Search the contents of each file.  
        // A regular expression created with the RegEx class  
        // could be used instead of the Contains method.  
        // queryMatchingFiles is an IEnumerable<string>.  
        var queryMatchingFiles =  
            from file in fileList  
            where file.Extension == ".htm"  
            let fileText = GetFileText(file.FullName)  
            where fileText.Contains(searchTerm)  
            select file.FullName;  
  
        // Execute the query.  
        Console.WriteLine("The term \"{0}\" was found in:", searchTerm);  
        foreach (string filename in queryMatchingFiles)  
        {  
            Console.WriteLine(filename);  
        }  
  
        // Keep the console window open in debug mode.  
        Console.WriteLine("Press any key to exit");  
        Console.ReadKey();  
    }  
  
    // Read the contents of the file.  
    static string GetFileText(string name)  
    {  
        string fileContents = String.Empty;  
  
        // If the file has been deleted since we took   
        // the snapshot, ignore it and return the empty string.  
        if (System.IO.File.Exists(name))  
        {  
            fileContents = System.IO.File.ReadAllText(name);  
        }  
        return fileContents;  
    }  
}  
```  
  
## <a name="compiling-the-code"></a>Compilation du code  
 Créez un projet qui cible le .NET Framework version 3.5 ou version ultérieure, avec une référence à System.Core.dll et des directives `using` pour les espaces de noms System.Linq et System.IO.  
  
## <a name="see-also"></a>Voir aussi  
 [LINQ et répertoires de fichiers (C#)](../../../../csharp/programming-guide/concepts/linq/linq-and-file-directories.md)   
 [LINQ to Objects (C#)](../../../../csharp/programming-guide/concepts/linq/linq-to-objects.md)