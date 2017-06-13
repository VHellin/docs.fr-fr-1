---
title: "Cr&#233;ation de r&#244;les d&#39;application dans SQL Server | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-ado"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 27442435-dfb2-4062-8c59-e2960833a638
caps.latest.revision: 9
author: "JennieHubbard"
ms.author: "jhubbard"
manager: "jhubbard"
caps.handback.revision: 9
---
# Cr&#233;ation de r&#244;les d&#39;application dans SQL Server
Les rôles d'application constituent un moyen d'affecter des autorisations à une application plutôt qu'à un rôle ou à un utilisateur de base de données.  Les utilisateurs peuvent se connecter à la base de données, activer le rôle d'application et assumer les autorisations accordées à l'application.  Les autorisations accordées au rôle d'application sont en vigueur pour la durée de la connexion.  
  
> [!IMPORTANT]
>  Les rôles d'application sont activés lorsqu'une application cliente fournit un nom de rôle d'application et un mot de passe dans la chaîne de connexion.  Ils présentent une vulnérabilité concernant la sécurité dans une application à deux niveaux, car le mot de passe doit être stocké sur l'ordinateur client.  Dans une application à trois niveaux, vous pouvez stocker le mot de passe afin qu'il soit inaccessible aux utilisateurs de l'application.  
  
## Fonctionnalités de rôle d'application  
 Les rôles d'application possèdent les fonctionnalités suivantes :  
  
-   Contrairement aux rôles de base de données, les rôles d'application ne contiennent aucun membre.  
  
-   Les rôles d'application sont activés lorsqu'une application fournit le nom de rôle d'application et un mot de passe à la procédure stockée système `sp_setapprole`.  
  
-   Le mot de passe doit être stocké sur l'ordinateur client et fourni au moment de l'exécution ; un rôle d'application ne peut pas être activé depuis SQL Server.  
  
-   Le mot de passe n'est pas chiffré.  Le mot de passe de paramètre est stocké comme un hachage unidirectionnel.  
  
-   Une fois activées, les autorisations acquises via le rôle d'application restent en vigueur tout au long de la durée de la connexion.  
  
-   Le rôle d'application hérite des autorisations accordées au rôle `public`.  
  
-   Si un membre du rôle de serveur fixe `sysadmin` active un rôle d'application, le contexte de sécurité bascule vers celui du rôle d'application pour la durée de la connexion.  
  
-   Si vous créez un compte `guest` dans une base de données qui possède un rôle d'application, vous n'avez pas besoin de créer un compte d'utilisateur de base de données pour le rôle d'application ni pour la connexion qui l'invoque, quelle qu'elle soit.  Les rôles d'application peuvent accéder directement à une autre base de données uniquement s'il existe un compte `guest` dans la deuxième base de données  
  
-   Des fonctions intégrées qui renvoient des noms de connexion, comme SYSTEM\_USER, renvoie le nom de la connexion qui a invoqué le rôle d'application.  Des fonctions intégrées qui renvoient des noms d'utilisateur de base de données renvoient le nom du rôle d'application.  
  
### Principe des privilèges minimum  
 Les rôles d'application doivent recevoir uniquement les autorisations requises au cas où le mot de passe serait compromis.  Des autorisations pour le rôle `public` doivent être révoquées dans n'importe quelle base de données utilisant un rôle d'application.  Désactivez le compte `guest` dans n'importe quelle base de données à laquelle vous ne voulez pas que des appelants du rôle d'application puissent accéder.  
  
### Améliorations du rôle d'application  
 Le contexte d'exécution peut dorénavant revenir à l'appelant d'origine après l'activation d'un rôle d'application, ce qui évite d'avoir à désactiver le regroupement de connexions.  La procédure `sp_setapprole` dispose d'une nouvelle option qui permet de créer un cookie contenant des informations de contexte relatives à l'appelant.  Vous pouvez restaurer la session en appelant la procédure `sp_unsetapprole`, en lui passant le cookie.  
  
## Alternatives aux rôles d'application  
 Les rôles d'application dépendent de la sécurité d'un mot de passe, qui présente une vulnérabilité de sécurité potentielle.  Des mots de passe peuvent être exposés lorsqu'ils sont intégrés dans du code d'application ou enregistrés sur un disque.  
  
 Vous pouvez prendre en compte les alternatives suivantes.  
  
-   Utilisez le changement de contexte avec l'instruction AS EXECUTE et ses clauses NO REVERT et WITH COOKIE.  Vous pouvez créer un compte d'utilisateur dans une base de données qui n'est pas mappée à une connexion.  Vous pouvez ensuite affecter des autorisations à ce compte.  L'utilisation d'EXECUTE AS avec un utilisateur sans connexion est plus sûre, car elle est basée sur les autorisations, pas sur le mot de passe.  Pour plus d'informations, consultez [Personnalisation des autorisations avec emprunt d'identité dans SQL Server](../../../../../docs/framework/data/adonet/sql/customizing-permissions-with-impersonation-in-sql-server.md).  
  
-   Signez des procédures stockées à l'aide de certificats, en accordant uniquement l'autorisation d'exécuter les procédures.  Pour plus d'informations, consultez [Signature de procédures stockées dans SQL Server](../../../../../docs/framework/data/adonet/sql/signing-stored-procedures-in-sql-server.md).  
  
## Ressources externes  
 Pour plus d'informations, voir les ressources ci\-dessous.  
  
|Ressource|Description|  
|---------------|-----------------|  
|[Rôles d'application](http://msdn.microsoft.com/library/ms190998.aspx) dans la documentation en ligne de SQL Server|Décrit comment créer et utiliser des rôles d'application dans SQL Server 2008.|  
  
## Voir aussi  
 [Sécurisation des applications ADO.NET](../../../../../docs/framework/data/adonet/securing-ado-net-applications.md)   
 [Vue d'ensemble de la sécurité SQL Server](../../../../../docs/framework/data/adonet/sql/overview-of-sql-server-security.md)   
 [Scénarios de sécurité des applications dans SQL Server](../../../../../docs/framework/data/adonet/sql/application-security-scenarios-in-sql-server.md)   
 [Fournisseurs managés ADO.NET et Centre de développement de DataSet](http://go.microsoft.com/fwlink/?LinkId=217917)