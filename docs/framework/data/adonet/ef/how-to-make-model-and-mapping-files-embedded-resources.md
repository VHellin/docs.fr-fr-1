---
title: "Comment : transformer les fichiers modèle et les fichiers de mappage en ressources incorporées"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology: dotnet-ado
ms.tgt_pltfrm: 
ms.topic: article
ms.assetid: 20dfae4d-e95a-4264-9540-f5ad23b462d3
caps.latest.revision: "3"
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.workload: dotnet
ms.openlocfilehash: 60c17ebbdd4b7e7f460855d4888a922da9c953d2
ms.sourcegitcommit: c0dd436f6f8f44dc80dc43b07f6841a00b74b23f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/19/2018
---
# <a name="how-to-make-model-and-mapping-files-embedded-resources"></a>Comment : transformer les fichiers modèle et les fichiers de mappage en ressources incorporées
Le [!INCLUDE[adonet_ef](../../../../../includes/adonet-ef-md.md)] vous permet de déployer des fichiers de mappage et de modèle en tant que ressources incorporées d’une application. L'assembly comprenant les fichiers de mappage et de modèle incorporés doit être chargé dans le même domaine d'application que la connexion d'entité. Pour plus d’informations, consultez [Chaînes de connexion](../../../../../docs/framework/data/adonet/ef/connection-strings.md). Par défaut, les outils [!INCLUDE[adonet_edm](../../../../../includes/adonet-edm-md.md)] incorporent les fichiers de mappage et de modèle. Lorsque vous définissez manuellement les fichiers de mappage et de modèle, utilisez cette procédure pour garantir que les fichiers sont déployés en tant que ressources incorporées avec une application [!INCLUDE[adonet_ef](../../../../../includes/adonet-ef-md.md)].  
  
> [!NOTE]
>  Pour conserver des ressources incorporées, vous devez répéter cette procédure chaque fois que les fichiers de mappage et de modèle sont modifiés.  
  
### <a name="to-embed-model-and-mapping-files"></a>Pour incorporer les fichiers de mappage et de modèle  
  
1.  Dans **l’Explorateur de solutions**, sélectionnez le fichier conceptuel (.csdl).  
  
2.  Dans le **propriétés** volet, définissez **Action de génération** à **ressource incorporée**.  
  
3.  Répétez les étapes 1 et 2 pour le fichier de stockage (.ssdl) et le fichier de mappage (.msl).  
  
4.  Dans **l’Explorateur de solutions**, double-cliquez sur le fichier App.config et modifiez le `Metadata` paramètre dans le `connectionString` attribut basé sur l’un des formats suivants :  
  
    -   `Metadata=` `res://<assemblyFullName>/<resourceName>;`  
  
    -   `Metadata=` `res://*/<resourceName>;`  
  
    -   `Metadata=res://*;`  
  
     Pour plus d’informations, consultez [Chaînes de connexion](../../../../../docs/framework/data/adonet/ef/connection-strings.md).  
  
## <a name="example"></a>Exemple  
 Fait référence à la chaîne de connexion pour les fichiers de mappage et de modèle incorporés le [AdventureWorks Sales Model](http://msdn.microsoft.com/library/f16cd988-673f-4376-b034-129ca93c7832). Cette chaîne de connexion est stockée dans le fichier App.config du projet.  
  
  
  
## <a name="see-also"></a>Voir aussi  
 [Modélisation et mappage](../../../../../docs/framework/data/adonet/ef/modeling-and-mapping.md)  
 [Guide pratique pour définir la chaîne de connexion](../../../../../docs/framework/data/adonet/ef/how-to-define-the-connection-string.md)  
 [Guide pratique pour créer une chaîne de connexion EntityConnection](../../../../../docs/framework/data/adonet/ef/how-to-build-an-entityconnection-connection-string.md)  
 [Outils ADO.NET Entity Data Model](http://msdn.microsoft.com/library/91076853-0881-421b-837a-f582f36be527)
