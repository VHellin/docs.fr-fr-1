---
title: "Utilisation du langage de définition de données"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology: dotnet-ado
ms.tgt_pltfrm: 
ms.topic: article
dev_langs:
- csharp
- vb
ms.assetid: ec50083d-44f4-4093-9b23-5eacd601f96e
caps.latest.revision: "3"
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.workload: dotnet
ms.openlocfilehash: dc2642df7cfe0f0a4b56537d0b2ebeae34304145
ms.sourcegitcommit: c0dd436f6f8f44dc80dc43b07f6841a00b74b23f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/19/2018
---
# <a name="working-with-data-definition-language"></a>Utilisation du langage de définition de données
En commençant par le [!INCLUDE[dnprdnshort](../../../../../includes/dnprdnshort-md.md)] version 4, le [!INCLUDE[adonet_ef](../../../../../includes/adonet-ef-md.md)] prend en charge le langage de définition de données (DDL). Cela vous permet de créer ou de supprimer une instance de base de données selon la chaîne de connexion et les métadonnées du modèle de stockage (SSDL).  
  
 Les méthodes suivantes sur l'objet <xref:System.Data.Objects.ObjectContext> utilisent la chaîne de connexion et le contenu SSDL pour effectuer les opérations suivantes : créer ou supprimer la base de données, vérifier si la base de données existe et consulter le script DDL généré :  
  
-   <xref:System.Data.Objects.ObjectContext.CreateDatabase%2A>  
  
-   <xref:System.Data.Objects.ObjectContext.DeleteDatabase%2A>  
  
-   <xref:System.Data.Objects.ObjectContext.DatabaseExists%2A>  
  
-   <xref:System.Data.Objects.ObjectContext.CreateDatabaseScript%2A>  
  
> [!NOTE]
>  L'exécution des commandes DDL exige de disposer d'autorisations suffisantes.  
  
 Les méthodes précédemment répertoriées délèguent la plupart du travail au fournisseur de données ADO.NET sous-jacent. Il incombe au fournisseur de vérifier que la convention d'affectation des noms utilisée pour générer les objets de base de données correspond aux conventions utilisées pour l'interrogation et les mises à jour.  
  
 L'exemple suivant vous indique comment générer la base de données selon le modèle existant. Il ajoute également un nouvel objet entité au contexte de l'objet puis l'enregistre dans la base de données.  
  
## <a name="procedures"></a>Procédures  
  
#### <a name="to-define-a-database-based-on-the-existing-model"></a>Pour définir une base de données selon le modèle existant  
  
1.  Créez une application console.  
  
2.  Ajoutez un modèle existant à votre application.  
  
    1.  Ajouter un modèle vide nommé `SchoolModel`. Pour créer un modèle vide, consultez la [Comment : créer un nouveau .edmx fichier](http://msdn.microsoft.com/library/beb8189e-e51c-4051-839c-9902c224abf2) rubrique.  
  
     Le fichier SchoolModel.edmx est ajouté à votre projet.  
  
    1.  Copier le stockage conceptuel et de mappage du contenu pour le modèle School à partir de la [modèle School](http://msdn.microsoft.com/library/859a9587-81ea-4a45-9bc0-f8d330e1adac) rubrique.  
  
    2.  Ouvrez le fichier SchoolModel.edmx et collez le contenu dans les balises `edmx:Runtime`.  
  
3.  Ajoutez le code suivant à votre fonction principale. Le code initialise la chaîne de connexion à votre serveur de base de données, consulte le script DDL, crée la base de données, ajoute une nouvelle entité au contexte et enregistre les modifications dans la base de données.  
  
     [!code-csharp[DP ObjectServices Concepts#DDL](../../../../../samples/snippets/csharp/VS_Snippets_Data/DP ObjectServices Concepts/CS/Source.cs#ddl)]
     [!code-vb[DP ObjectServices Concepts#DDL](../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP ObjectServices Concepts/VB/Source.vb#ddl)]
