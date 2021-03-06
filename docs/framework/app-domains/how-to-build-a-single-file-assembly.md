---
title: Guide pratique pour générer un assembly à fichier unique
ms.custom: ''
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: ''
ms.suite: ''
ms.technology:
- dotnet-bcl
ms.tgt_pltfrm: ''
ms.topic: article
dev_langs:
- csharp
- vb
helpviewer_keywords:
- assembly manifest, single-file assemblies
- library assemblies
- command-line compilers
- assemblies [.NET Framework], single-file
- output file name for assemblies
- code modules
- single-file assemblies
ms.assetid: a6063221-43a5-4d3e-814c-288a4ec69aec
caps.latest.revision: ''
author: rpetrusha
ms.author: ronpet
manager: wpickett
ms.workload:
- dotnet
ms.openlocfilehash: 80fa584a21a3bdfb9392021959d777139daafd04
ms.sourcegitcommit: 498799639937c89de777361aab74261efe7b79ea
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/22/2018
---
# <a name="how-to-build-a-single-file-assembly"></a>Guide pratique pour générer un assembly à fichier unique
Un assembly à fichier unique, qui est le type d’assembly le plus simple, contient l’implémentation et les informations de type, ainsi que le [manifeste d’assembly](../../../docs/framework/app-domains/assembly-manifest.md). Vous pouvez utiliser des compilateurs de ligne de commande ou [!INCLUDE[vsprvslong](../../../includes/vsprvslong-md.md)] pour créer un assembly à fichier unique. Par défaut, le compilateur crée un fichier d’assembly avec une extension .exe.  
  
> [!NOTE]
>  [!INCLUDE[vsprvslong](../../../includes/vsprvslong-md.md)] pour C# et Visual Basic peut être utilisé uniquement pour créer des assemblys à fichier unique. Si vous souhaitez créer des assemblys multifichiers, vous devez utiliser les compilateurs de ligne de commande ou [!INCLUDE[vsprvslong](../../../includes/vsprvslong-md.md)] pour Visual C++.  
  
 Les procédures suivantes montrent comment créer des assemblys à fichier unique à l’aide des compilateurs de ligne de commande.  
  
### <a name="to-create-an-assembly-with-an-exe-extension"></a>Pour créer un assembly avec une extension .exe  
  
1.  À l'invite de commandes, tapez la commande suivante :  
  
     \<*commande_du_compilateur*> \<*nom_du_module*>  
  
     Dans cette commande, *commande_du_compilateur* est la commande du compilateur pour le langage utilisé dans votre module de code, et *nom_du_module* est le nom du module de code à compiler dans l’assembly.  
  
 L’exemple suivant crée un assembly nommé `myCode.exe` à partir d’un module de code nommé `myCode`.  
  
```console
csc myCode.cs  
```  

```console
vbc myCode.vb  
```  
  
#### <a name="to-create-an-assembly-with-an-exe-extension-and-specify-the-output-file-name"></a>Pour créer un assembly avec une extension .exe et spécifier le nom du fichier de sortie  
  
1.  À l'invite de commandes, tapez la commande suivante :  
  
     \<*commande_du_compilateur*> **/out:**\<*nom_du_fichier*> \<*nom_du_module*>  
  
     Dans cette commande, *commande_du_compilateur* est la commande du compilateur pour le langage utilisé dans votre module de code, *nom_du_fichier* est le nom du fichier de sortie, et *nom_du_module* est le nom du module de code à compiler dans l’assembly.  
  
 L’exemple suivant crée un assembly nommé `myAssembly.exe` à partir d’un module de code nommé `myCode`.  
  
```console  
csc -out:myAssembly.exe myCode.cs  
```  
  
```console
vbc -out:myAssembly.exe myCode.vb  
```  
  
## <a name="creating-library-assemblies"></a>Création d’assemblys de bibliothèque  
 Un assembly de bibliothèque est similaire à une bibliothèque de classes. Il contient des types qui seront référencés par d’autres assemblys, mais n’a aucun point d’entrée pour commencer l’exécution.  
  
#### <a name="to-create-a-library-assembly"></a>Pour créer un assembly de bibliothèque  
  
1.  À l'invite de commandes, tapez la commande suivante :  
  
     \<*commande_du_compilateur*> **-t:library** \<*nom_du_module*>  
  
     Dans cette commande, *commande_du_compilateur* est la commande du compilateur pour le langage utilisé dans votre module de code, et *nom_du_module* est le nom du module de code à compiler dans l’assembly. Vous pouvez également utiliser d’autres options du compilateur, telles que l’option **-out:**.  
  
 L’exemple suivant crée un assembly de bibliothèque nommé `myCodeAssembly.dll` à partir d’un module de code nommé `myCode`.  
  
```console  
csc -out:myCodeLibrary.dll -t:library myCode.cs  
```  
  
```console
vbc -out:myCodeLibrary.dll -t:library myCode.vb  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Création d’assemblys](../../../docs/framework/app-domains/create-assemblies.md)  
 [Assemblys multifichiers](../../../docs/framework/app-domains/multifile-assemblies.md)  
 [Guide pratique pour générer un assembly multifichier](../../../docs/framework/app-domains/how-to-build-a-multifile-assembly.md)  
 [Programmation à l’aide d’assemblys](../../../docs/framework/app-domains/programming-with-assemblies.md)
