---
title: "Comment&#160;: h&#233;berger un service WCF dans WAS | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 9e3e213e-2dce-4f98-81a3-f62f44caeb54
caps.latest.revision: 25
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 25
---
# Comment&#160;: h&#233;berger un service WCF dans WAS
Cette rubrique décrit les étapes de base requises pour créer un service [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] hébergé dans le service d'activation des processus de Windows (également appelé WAS). WAS est le nouveau service d'activation de processus généralisant les fonctionnalités des services IIS (Internet Information Services) qui fonctionnent avec des protocoles de transport non-HTTP. [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] utilise l'interface d'adaptateur d'écouteur pour communiquer les demandes d'activation qui sont reçues sur les protocoles non HTTP pris en charge par [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)], tels que TCP, les canaux nommés et Message Queuing.  
  
 Cette option d'hébergement requiert que les composants d'activation WAS soient installés et configurés correctement, mais ne requiert pas l'écriture du code d'hébergement dans le cadre de l'application. [!INCLUDE[crabout](../../../../includes/crabout-md.md)]installation et configuration du service WAS, consultez [Comment : installer et configurer les composants d’Activation WCF](../../../../docs/framework/wcf/feature-details/how-to-install-and-configure-wcf-activation-components.md).  
  
> [!WARNING]
>  L’activation de WAS n’est pas prise en charge si le pipeline de traitement de demande du serveur web est en mode classique. Le pipeline de traitement de demande du serveur Web doit être en mode intégré si l'activation de WAS doit être utilisée.  
  
 Lorsqu'un service [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] est hébergé dans WAS, les liaisons standard sont utilisées de la façon habituelle. Toutefois, lorsque vous utilisez la <xref:System.ServiceModel.NetTcpBinding> et <xref:System.ServiceModel.NetNamedPipeBinding> pour configurer un service hébergé par WAS, une contrainte doit être satisfaite. Lorsque des points de terminaison différents utilisent le même transport, les paramètres de liaison doivent correspondre dans les sept propriétés suivantes :  
  
-   ConnectionBufferSize  
  
-   ChannelInitializationTimeout  
  
-   MaxPendingConnections  
  
-   MaxOutputDelay  
  
-   MaxPendingAccepts  
  
-   ConnectionPoolSettings.IdleTimeout  
  
-   ConnectionPoolSettings.MaxOutboundConnectionsPerEndpoint  
  
 Dans le cas contraire, le point de terminaison initialisé premier détermine toujours les valeurs de ces propriétés, et les points de terminaison ajoutés ultérieurement lèvent un <xref:System.ServiceModel.ServiceActivationException> si elles ne correspondent pas ces paramètres.  
  
 Pour la copie de la source de cet exemple, consultez la page [l’Activation TCP](../../../../docs/framework/wcf/samples/tcp-activation.md).  
  
### <a name="to-create-a-basic-service-hosted-by-was"></a>Pour créer un service de base hébergé par WAS  
  
1.  Définissez un contrat de service pour le type de service.  
  
     [!code-csharp[C_HowTo_HostInWAS#1121](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_hostinwas/cs/service.cs#1121)]  
  
2.  Implémentez le contrat de service dans une classe de service. Notez que l'adresse ou les informations de liaison ne sont pas spécifiées dans l'implémentation du service. Par ailleurs, il n'est pas nécessaire d'écrire du code pour récupérer ces informations à partir du fichier de configuration.  
  
     [!code-csharp[C_HowTo_HostInWAS#1122](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_hostinwas/cs/service.cs#1122)]  
  
3.  Créez un fichier Web.config pour définir le <xref:System.ServiceModel.NetTcpBinding> liaison qui sera utilisé par le `CalculatorService` points de terminaison.  
  
    ```xml  
    <?xml version="1.0" encoding="utf-8" ?>  
    <configuration>  
      <system.serviceModel>  
        <bindings>  
          <netTcpBinding>  
            <binding portSharingEnabled="true">  
              <security mode="None" />  
            </binding>  
          </netTcpBinding>  
        </bindings>  
      </system.serviceModel>  
    </configuration>  
    ```  
  
4.  Créez un fichier Service.svc contenant le code suivant.  
  
    ```  
    <%@ServiceHost language=c# Service="CalculatorService" %>   
    ```  
  
5.  Placez le fichier Service.svc dans votre rɰertoire virtuel IIS.  
  
### <a name="to-create-a-client-to-use-the-service"></a>Pour créer un client qui utilise le service  
  
1.  Utilisez [Service Model Metadata Utility Tool (Svcutil.exe)](../../../../docs/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe.md) à partir de la ligne de commande pour générer le code à partir des métadonnées de service.  
  
    ```  
    Svcutil.exe <service's Metadata Exchange (MEX) address or HTTP GET address>   
    ```  
  
2.  Le client généré contient l'interface `ICalculator` qui définit le contrat de service auquel l'implémentation du client doit satisfaire.  
  
     [!code-csharp[C_HowTo_HostInWAS#1221](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_hostinwas/cs/client.cs#1221)]  
  
3.  L'application cliente générée contient également l'implémentation de `ClientCalculator`. Notez que les informations d'adresse et de liaison ne sont pas spécifiées n'importe où dans l'implémentation du service. Par ailleurs, il n'est pas nécessaire d'écrire du code pour récupérer ces informations à partir du fichier de configuration.  
  
     [!code-csharp[C_HowTo_HostInWAS#1222](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_hostinwas/cs/client.cs#1222)]  
  
4.  La configuration du client qui utilise le <xref:System.ServiceModel.NetTcpBinding> est également générée par Svcutil.exe. Ce fichier doit être nommé dans le fichier App.config lors de l'utilisation de Visual Studio.  
  
     <!-- TODO: review snippet reference [!code[C_HowTo_HostInWAS#2211](../../../../samples/snippets/common/VS_Snippets_CFX/c_howto_hostinwas/common/app.config#2211)]  -->  
  
5.  Créez une instance `ClientCalculator` dans une application, puis appelez les opérations de service.  
  
     [!code-csharp[C_HowTo_HostInWAS#1223](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_hostinwas/cs/client.cs#1223)]  
  
6.  Compilez, puis exécutez le client.  
  
## <a name="see-also"></a>Voir aussi  
 [Activation TCP](../../../../docs/framework/wcf/samples/tcp-activation.md)   
 [Fonctionnalités d’hébergement de Windows Server App Fabric](http://go.microsoft.com/fwlink/?LinkId=201276)