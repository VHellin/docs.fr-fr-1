---
title: Introduction au routage
ms.date: 03/30/2017
ms.assetid: bf6ceb38-6622-433b-9ee7-f79bc93497a1
ms.openlocfilehash: e540e084305aee51d6820cc9ae43f7791d5c07d6
ms.sourcegitcommit: 586dbdcaef9767642436b1e4efbe88fb15473d6f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/06/2018
ms.locfileid: "48842763"
---
# <a name="routing-introduction"></a>Introduction au routage
Le service de routage fournit un intermédiaire SOAP générique connectable, capable de router des messages en fonction du contenu. Le service de routage vous permet de créer une logique de routage complexe et d'implémenter des scénarios, tels que l'agrégation de service, le contrôle des versions de service, le routage par priorité et en mode multidiffusion. Le service de routage fournit également une gestion des erreurs qui vous permet de définir des listes de points de terminaison de sauvegarde auxquels sont envoyés les messages en cas d'échec de l'envoi au point de terminaison de destination primaire.  
  
 Cette rubrique est destinée aux nouveaux utilisateurs du service de routage ; elle présente la configuration de base et l'hébergement du service de routage.  
  
## <a name="configuration"></a>Configuration  
 Le service de routage est implémenté en tant que service WCF qui expose un ou plusieurs points de terminaison de service ; ceux-ci reçoivent les messages provenant d'applications clientes et les acheminent vers un ou plusieurs points de terminaison de destination. Le service fournit une classe <xref:System.ServiceModel.Routing.RoutingBehavior>, appliquée aux points de terminaison de service exposés par le service. Ce comportement permet de configurer différents aspects du fonctionnement du service. Pour faciliter la configuration lors de l’utilisation d’un fichier de configuration, les paramètres sont spécifiés dans le **RoutingBehavior**. Dans les scénarios basés sur le code, ces paramètres seraient spécifiés comme partie d’un <xref:System.ServiceModel.Routing.RoutingConfiguration> objet, qui peut ensuite être transmis à un **RoutingBehavior**.  
  
 Au moment du démarrage, ce comportement ajoute aux points de terminaison clients la classe <xref:System.ServiceModel.Routing.SoapProcessingBehavior>, utilisée pour exécuter le traitement SOAP des messages. Cela permet au Service de routage transmettre des messages aux points de terminaison qui requièrent une autre **MessageVersion** que le message a été reçu sur le point de terminaison. Le **RoutingBehavior** enregistre également une extension de service, le <xref:System.ServiceModel.Routing.RoutingExtension>, qui fournit un point d’accès pour modifier la configuration de Service de routage au moment de l’exécution.  
  
 Le **RoutingConfiguration** classe fournit une méthode cohérente de la configuration et la mise à jour la configuration du Service de routage.  Il contient des paramètres qui agissent en tant que les paramètres pour le Service de routage et sont utilisés pour configurer le **RoutingBehavior** lorsque le service démarre, ou est passé à la **RoutingExtension** pour modifier le routage configuration en cours d’exécution.  
  
 La logique de routage utilisée pour router les messages en fonction du contenu est définie en regroupant plusieurs objets <xref:System.ServiceModel.Dispatcher.MessageFilter> dans des tables de filtres (objets <xref:System.ServiceModel.Dispatcher.MessageFilterTable%601>). Les messages entrants sont évalués sur les filtres de messages contenus dans la table de filtres et pour chaque **MessageFilter** qui correspond au message, transféré vers un point de terminaison de destination. La table de filtres qui doit être utilisé pour acheminer les messages est spécifiée en utilisant soit le **RoutingBehavior** dans la configuration ou le code à l’aide de la **RoutingConfiguration** objet.  
  
### <a name="defining-endpoints"></a>Définition des points de terminaison  
 La première chose à configurer n'est pas, comme vous pourriez le penser, de définir la logique de routage que vous utiliserez, mais de déterminer la forme des points de terminaison vers lesquels les messages seront routés. Le service de routage utilise des contrats définissant la forme des canaux qui permettent de recevoir et envoyer des messages ; par conséquent, la forme du canal d'entrée doit correspondre à celle du canal de sortie.  Par exemple, si vous effectuez un routage vers des points de terminaison utilisant la forme de canal demande-réponse, vous devez utiliser un contrat compatible sur les points de terminaison entrants, comme l'interface <xref:System.ServiceModel.Routing.IRequestReplyRouter>.  
  
 Cela signifie que si vos points de terminaison de destination utilisent des contrats avec plusieurs modèles de communication (comme une combinaison d'opérations monodirectionnelles et bidirectionnelles), vous ne pouvez pas créer de point de terminaison de service unique qui puisse recevoir et router des messages vers tous les points de terminaison de destination. Vous devez déterminer les points de terminaison qui ont des formes compatibles et définir un ou plusieurs points de terminaison de service qui permettront de recevoir les messages devant être routés vers les points de terminaison de destination.  
  
> [!NOTE]
> En cas d'utilisation de contrats spécifiant plusieurs modèles de communication (comme une combinaison d'opérations monodirectionnelles et bidirectionnelles), vous pouvez utiliser un contrat duplex au service de routage tel que l'interface <xref:System.ServiceModel.Routing.IDuplexSessionRouter>. Cependant, cela suppose que la liaison soit capable d'une communication duplex, ce qui n'est pas le cas dans tous les scénarios. Dans les scénarios où cela n'est pas possible, il faudra peut-être développer la communication en plusieurs points de terminaison ou modifier l'application.  
  
 Pour plus d’informations sur les contrats de routage, consultez [contrats de routage](routing-contracts.md).  
  
 Après avoir défini le point de terminaison de service, vous pouvez utiliser la **RoutingBehavior** pour associer un spécifique **RoutingConfiguration** avec le point de terminaison. Lorsque vous configurez le Service de routage à l’aide d’un fichier de configuration, le **RoutingBehavior** est utilisé pour spécifier la table de filtres qui contient la logique de routage utilisée pour traiter les messages reçus sur ce point de terminaison. Si vous configurez le Service de routage par programme, vous pouvez spécifier la table de filtres à l’aide de la **RoutingConfiguration**.  
  
 L'exemple suivant définit les points de terminaison clients et de service utilisés par le service de routage à la fois par programme et par un fichier de configuration.  
  
```xml  
    <services>  
      <!--ROUTING SERVICE -->  
      <service behaviorConfiguration="routingData"  
               name="System.ServiceModel.Routing.RoutingService">  
        <host>  
          <baseAddresses>  
            <add baseAddress="http://localhost:8000/routingservice/router"/>  
          </baseAddresses>  
        </host>  
        <!-- Define the service endpoints that are receive messages -->  
        <endpoint address=""  
                  binding="wsHttpBinding"  
                  name="reqReplyEndpoint"  
                  contract="System.ServiceModel.Routing.IRequestReplyRouter" />
      </service>  
    </services>  
    <behaviors>  
      <serviceBehaviors>  
        <behavior name="routingData">  
          <serviceMetadata httpGetEnabled="True"/>  
          <!-- Add the RoutingBehavior and specify the Routing Table to use -->  
          <routing filterTableName="routingTable1" />  
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
    <client>  
    <!-- Define the client endpoint(s) to route messages to -->  
      <endpoint name="CalculatorService"  
                address="http://localhost:8000/servicemodelsamples/service"  
                binding="wsHttpBinding" contract="*" />  
    </client>  
```  
  
```csharp  
//set up some communication defaults  
string clientAddress = "http://localhost:8000/servicemodelsamples/service";  
string routerAddress = "http://localhost:8000/routingservice/router";  
Binding routerBinding = new WSHttpBinding();  
Binding clientBinding = new WSHttpBinding();  
//add the endpoint the router uses to receive messages  
serviceHost.AddServiceEndpoint(  
     typeof(IRequestReplyRouter),   
     routerBinding,   
     routerAddress);  
//create the client endpoint the router routes messages to  
ContractDescription contract = ContractDescription.GetContract(  
     typeof(IRequestReplyRouter));  
ServiceEndpoint client = new ServiceEndpoint(  
     contract,   
     clientBinding,   
     new EndpointAddress(clientAddress));  
//create a new routing configuration object  
RoutingConfiguration rc = new RoutingConfiguration();  
….  
rc.FilterTable.Add(new MatchAllMessageFilter(), endpointList);  
//attach the behavior to the service host  
serviceHost.Description.Behaviors.Add(  
     new RoutingBehavior(rc));  
```  
  
 Cet exemple configure le service de routage pour exposer un point de terminaison unique avec une adresse `http://localhost:8000/routingservice/router`, qui est utilisée pour recevoir des messages à router. Étant donné que les messages sont routés vers des points de terminaison demande-réponse, le point de terminaison de service utilise le contrat <xref:System.ServiceModel.Routing.IRequestReplyRouter>. Cette configuration définit également un point de terminaison client unique `http://localhost:8000/servicemodelsample/service` vers lequel les messages sont routés. La table de filtres (non illustrée) nommée « routingTable1 » contient la logique de routage utilisée pour router les messages et elle est associée au point de terminaison de service en utilisant **RoutingBehavior** (pour un fichier de configuration) ou **RoutingConfiguration** (pour une configuration par programmation).  
  
### <a name="routing-logic"></a>Logique de routage  
 Pour définir la logique de routage utilisée pour router les messages, vous devez déterminer sur quelles données contenues dans les messages entrants il est possible d'agir de manière unique. Par exemple, si tous les points de terminaison de destination de votre routage partagent la même Action SOAP, la valeur de l'Action contenue dans le message ne constitue pas un bon indicateur du point de terminaison spécifique vers lequel le message doit être routé. Si vous devez router des messages uniquement vers un point de terminaison spécifique, vous devez appliquer un filtre sur des données qui identifient de manière unique le point de terminaison de destination vers lequel le message est routé.  
  
 Le Service de routage fournit plusieurs **MessageFilter** implémentations qui inspectent des valeurs spécifiques dans le message, telles que l’adresse, action, nom de point de terminaison ou même une requête XPath. Si aucun de ces implémentations ne répond à vos besoins, vous pouvez créer un personnalisé **MessageFilter** implémentation. Pour plus d’informations sur les filtres de message et une comparaison des implémentations utilisées par le Service de routage, consultez [filtres de Message](message-filters.md) et [choix d’un filtre](choosing-a-filter.md).  
  
 Plusieurs filtres de message sont organisés en tables de filtres, qui associent chaque **MessageFilter** avec un point de terminaison de destination. La table de filtres peut également être utilisée pour spécifier une liste de points de terminaison de sauvegarde auxquels, en cas d'échec de transmission, le service de routage tente d'envoyer le message.  
  
 Par défaut, tous les filtres de message d'une table de filtres sont évalués simultanément ; toutefois, vous pouvez spécifier une propriété <xref:System.ServiceModel.Routing.Configuration.FilterTableEntryElement.Priority%2A> qui fait évaluer les filtres de messages dans un ordre spécifique. Les entrées de priorité plus élevée sont évaluées en premier et les filtres de message de priorité plus faible ne sont pas évalués si un résultat est trouvé à un niveau de priorité supérieur. Pour plus d’informations sur les tables de filtres, consultez [filtres de Message](message-filters.md).  
  
 Les exemples suivants utilisent l'objet <xref:System.ServiceModel.Dispatcher.MatchAllMessageFilter>, qui prend tous les messages qui ont la valeur `true`. Cela **MessageFilter** est ajouté à la table de filtres « routingTable1 », qui associe le **MessageFilter** avec le point de terminaison client nommé « CalculatorService ». Le **RoutingBehavior** puis spécifie que cette table doit être utilisé pour router les messages traités par le point de terminaison de service.  
  
```xml  
<behaviors>  
  <serviceBehaviors>  
    <behavior name="routingData">  
      <serviceMetadata httpGetEnabled="True"/>  
      <!-- Add the RoutingBehavior and specify the Routing Table to use -->  
      <routing filterTableName="routingTable1" />  
    </behavior>  
  </serviceBehaviors>  
</behaviors>  
<!--ROUTING SECTION -->  
<routing>  
  <filters>  
    <filter name="MatchAllFilter1" filterType="MatchAll" />  
  </filters>  
  <filterTables>  
    <table name="routingTable1">  
      <filters>  
        <add filterName="MatchAllFilter1" endpointName="CalculatorService" />  
      </filters>  
    </table>  
  </filterTables>  
</routing>  
```  
  
```csharp  
//create a new routing configuration object  
RoutingConfiguration rc = new RoutingConfiguration();  
//create the endpoint list that contains the endpoints to route to  
//in this case we have only one  
List<ServiceEndpoint> endpointList = new List<ServiceEndpoint>();  
endpointList.Add(client);  
//add a MatchAll filter to the Router's filter table  
//map it to the endpoint list defined earlier  
//when a message matches this filter, it is sent to the endpoint contained in the list  
rc.FilterTable.Add(new MatchAllMessageFilter(), endpointList);  
```  
  
> [!NOTE]
>  Par défaut, le service de routage évalue uniquement les en-têtes du message. Pour permettre aux filtres d'accéder au corps du message, vous devez attribuer à la propriété <xref:System.ServiceModel.Routing.RoutingConfiguration.RouteOnHeadersOnly%2A> la valeur `false`.  
  
 **Multidiffusion**  
  
 Alors que de nombreuses configurations de service de routage utilisent une logique de filtre exclusive qui route les messages vers un seul point de terminaison spécifique, vous aurez peut-être besoin de router un message donné vers plusieurs points de terminaison de destination. Pour envoyer un message en mode multidiffusion à des destinations multiples, les conditions suivantes doivent être remplies :  
  
-   La forme de canal ne doit pas être de type demande-réponse (bien qu'elle puisse être monodirectionnelle ou duplex), parce qu'une seule réponse peut être reçue par l'application cliente en réponse à la demande.  
  
-   Plusieurs filtres doivent retourner une valeur `true` lors de l'évaluation du message.  
  
 Si ces conditions sont remplies, le message est routé vers tous les points de terminaison de tous les filtres qui doivent avoir la valeur `true`. L’exemple suivant définit une configuration de routage qui résulte dans les messages acheminés vers les deux points de terminaison si l’adresse de point de terminaison dans le message est `http://localhost:8000/routingservice/router/rounding`.  
  
```xml  
<!--ROUTING SECTION -->  
<routing>  
  <filters>  
    <filter name="MatchAllFilter1" filterType="MatchAll" />  
    <filter name="RoundingFilter1" filterType="EndpointAddress"  
            filterData="http://localhost:8000/routingservice/router/rounding" />  
  </filters>  
  <filterTables>  
    <table name="routingTable1">  
      <filters>  
        <add filterName="MatchAllFilter1" endpointName="CalculatorService" />  
        <add filterName="RoundingFilter1" endpointName="RoundingCalcService" />  
      </filters>  
    </table>  
  </filterTables>  
</routing>  
```  
  
```csharp  
rc.FilterTable.Add(new MatchAllMessageFilter(), calculatorEndpointList);  
rc.FilterTable.Add(new EndpointAddressMessageFilter(new EndpointAddress(  
    "http://localhost:8000/routingservice/router/rounding")),  
    roundingCalcEndpointList);  
```  
  
### <a name="soap-processing"></a>Traitement SOAP  
 Pour prendre en charge le routage des messages entre des protocoles différents, le **RoutingBehavior** par défaut ajoute le <xref:System.ServiceModel.Routing.SoapProcessingBehavior> à tous les points de terminaison client messages sont routés vers. Ce comportement crée automatiquement un **MessageVersion** avant routage du message vers le point de terminaison, ainsi que la création d’un écran compatible **MessageVersion** pour tout document de réponse avant de le renvoyer à l’application cliente demandeuse.  
  
 Les étapes effectuées pour créer un nouveau **MessageVersion** pour le message sortant sont les suivantes :  
  
 **Traitement des demandes**  
  
-   Obtenir le **MessageVersion** de la liaison/canal sortant.  
  
-   Obtenez le lecteur de corps du message d'origine.  
  
-   Créer un nouveau message avec la même action, lecteur de corps et un nouveau **MessageVersion**.  
  
-   Si <xref:System.ServiceModel.Channels.MessageVersion.Addressing%2A> ! = **Addressing.None**, copiez la ligne à, de, FaultTo et les en-têtes RelatesTo dans le nouveau message.  
  
-   Copiez toutes les propriétés du message dans le nouveau message.  
  
-   Stockez le message de demande d'origine à utiliser lors du traitement de la réponse.  
  
-   Retournez le nouveau message de demande.  
  
 **Traitement de la réponse**  
  
-   Obtenir le **MessageVersion** du message de demande d’origine.  
  
-   Obtenez le lecteur de corps du message de réponse reçu.  
  
-   Créer un nouveau message de réponse avec la même action, le lecteur de corps et le **MessageVersion** du message de demande d’origine.  
  
-   Si <xref:System.ServiceModel.Channels.MessageVersion.Addressing%2A> ! = **Addressing.None**, copiez la ligne à, de, FaultTo et les en-têtes RelatesTo dans le nouveau message.  
  
-   Copiez les propriétés du message dans le nouveau message.  
  
-   Retournez le nouveau message de réponse.  
  
 Par défaut, le **SoapProcessingBehavior** est automatiquement ajouté aux points de terminaison client par le <xref:System.ServiceModel.Routing.RoutingBehavior> lorsque le service démarre ; Toutefois, vous pouvez contrôler si le traitement SOAP est ajouté à tous les points de terminaison de client à l’aide de la <xref:System.ServiceModel.Routing.RoutingConfiguration.SoapProcessingEnabled%2A> propriété. Vous pouvez également ajouter directement le comportement à un point de terminaison spécifique, au niveau duquel vous pouvez l'activer ou le désactiver, si un contrôle plus précis du traitement SOAP est nécessaire.  
  
> [!NOTE]
>  Si le traitement SOAP est désactivé pour un point de terminaison qui requiert une version MessageVersion différente de celle du message de demande d'origine, vous devez fournir un mécanisme personnalisé pour effectuer les modifications SOAP nécessaires avant d'envoyer le message au point de terminaison de destination.  
  
 Dans les exemples suivants, le **soapProcessingEnabled** propriété est utilisée pour empêcher le **SoapProcessingBehavior** d’être ajouté automatiquement à tous les points de terminaison client.  
  
```xml  
<behaviors>  
  <!--default routing service behavior definition-->  
  <serviceBehaviors>  
    <behavior name="routingConfiguration">  
      <routing filterTableName="filterTable1" soapProcessingEnabled="false"/>  
    </behavior>  
  </serviceBehaviors>  
</behaviors>  
```  
  
```csharp  
//create the default RoutingConfiguration  
RoutingConfiguration rc = new RoutingConfiguration();  
rc.SoapProcessingEnabled = false;  
```  
  
### <a name="dynamic-configuration"></a>Configuration Dynamique  
 Lorsque vous ajoutez des points de terminaison clients supplémentaires, ou que vous devez modifier les filtres utilisés pour router les messages, vous devez pouvoir mettre la configuration à jour dynamiquement au moment de l'exécution afin d'éviter une interruption de service aux points de terminaison qui reçoivent actuellement des messages via le service de routage. Modifier un fichier de configuration ou le code de l'application hôte n'est pas toujours suffisant, car l'une ou l'autre méthode requiert le recyclage de l'application, ce qui signifierait la perte potentielle de tous les messages actuellement en transit et le risque d'un temps mort en attendant le redémarrage du service.  
  
 Vous pouvez uniquement modifier le **RoutingConfiguration** par programmation. Vous pouvez initialement configurer le service à l’aide d’un fichier de configuration, vous pouvez uniquement modifier la configuration au moment de l’exécution en créant un nouveau **RoutingConfigution** et en lui passant comme paramètre à la <xref:System.ServiceModel.Routing.RoutingExtension.ApplyConfiguration%2A> (méthode) exposées par le <xref:System.ServiceModel.Routing.RoutingExtension> extension du service. Tous les messages actuellement en transit continuent d’être routés à l’aide de la configuration précédente, alors que les messages reçus après l’appel à **ApplyConfiguration** utilisent la nouvelle configuration. L'exemple suivant montre la création d'une instance du service de routage, suivie d'une modification de la configuration.  
  
```csharp  
RoutingConfiguration routingConfig = new RoutingConfiguration();  
routingConfig.RouteOnHeadersOnly = true;  
routingConfig.FilterTable.Add(new MatchAllMessageFilter(), endpointList);  
RoutingBehavior routing = new RoutingBehavior(routingConfig);  
routerHost.Description.Behaviors.Add(routing);  
routerHost.Open();  
// Construct a new RoutingConfiguration  
RoutingConfiguration rc2 = new RoutingConfiguration();  
ServiceEndpoint clientEndpoint = new ServiceEndpoint();  
ServiceEndpoint clientEndpoint2 = new ServiceEndpoint();  
// Add filters to the FilterTable in the new configuration  
rc2.FilterTable.add(new MatchAllMessageFilter(),  
       new List<ServiceEndpoint>() { clientEndpoint });  
rc2.FilterTable.add(new MatchAllMessageFilter(),  
       new List<ServiceEndpoint>() { clientEndpoint2 });  
rc2.RouteOnHeadersOnly = false;  
// Apply the new configuration to the Routing Service hosted in  
routerHost.routerHost.Extensions.Find<RoutingExtension>().ApplyConfiguration(rc2);  
```  
  
> [!NOTE]
>  Lorsque le service de routage est mis à jour de cette manière, seule la transmission d'une nouvelle configuration est possible. Il est impossible de modifier uniquement une sélection d'éléments de la configuration actuelle ni d'ajouter à celle-ci de nouvelles entrées ; vous devez créer et transmettre une nouvelle configuration qui remplace l'existante.  
  
> [!NOTE]
>  Toutes les sessions ouvertes à l'aide de la configuration précédente continuent à utiliser la configuration précédente. La nouvelle configuration est utilisée uniquement par les nouvelles sessions.  
  
## <a name="error-handling"></a>Gestion des erreurs  
 Si un objet <xref:System.ServiceModel.CommunicationException> est rencontré lors de la tentative d'envoi d'un message, une gestion des erreurs intervient. Ces exceptions indiquent en général qu'un problème s'est produit lors de la tentative de communication avec le point de terminaison client défini ; il peut s'agir d'une exception <xref:System.ServiceModel.EndpointNotFoundException>, <xref:System.ServiceModel.ServerTooBusyException> ou <xref:System.ServiceModel.CommunicationObjectFaultedException>. Le code de gestion des erreurs intercepte également et tentez de nouvelle tentative d’envoi quand un <xref:System.TimeoutException> se produit, qui est une autre exception courante non dérivée de **CommunicationException**.  
  
 Lorsque l'une des exceptions précédentes se produit, le service de routage bascule sur une liste de points de terminaison de sauvegarde. Si tous les points de terminaison de sauvegarde échouent avec un échec de communication, ou si un point de terminaison retourne une exception indiquant une défaillance dans le service de destination, le service de routage retourne une erreur à l'application cliente.  
  
> [!NOTE]
>  Les fonctionnalités de gestion des erreurs capturent et gèrent des exceptions qui se produisent lors des tentatives d'envoi d'un message et de fermeture d'un canal. Le code de gestion des erreurs n’est pas destiné à détecter ni à gérer les exceptions créées par les points de terminaison application qu'il communique avec ; un <xref:System.ServiceModel.FaultException> levée par un service apparaît au niveau du Service de routage comme un **FaultMessage** et est transmise au client.  
>   
>  Si une erreur se produit lorsque le service de routage tente de relayer un message, vous pouvez obtenir un <xref:System.ServiceModel.FaultException> côté client, au lieu du <xref:System.ServiceModel.EndpointNotFoundException> que vous obtiendriez normalement en l'absence du service de routage. Un service de routage peut par conséquent masquer les exceptions et ne pas fournir une transparence complète, sauf si vous analysez les exceptions imbriquées.  
  
### <a name="tracing-exceptions"></a>Suivi d'exceptions  
 Lorsque vous envoyez un message à un point de terminaison dans une liste échoue, le Service de routage effectue le suivi des données d’exception résultantes et joint les détails de l’exception sous la forme d’une propriété de message nommée **Exceptions**. Cela conserve les données d'exception et autorise un accès utilisateur par programme via un inspecteur de message.  Les données d'exception sont stockées par message dans un dictionnaire qui mappe le nom du point de terminaison aux détails de l'exception rencontrée lors de la tentative d'envoi d'un message à ce point.  
  
### <a name="backup-endpoints"></a>Points de terminaison de sauvegarde  
 Chaque entrée de filtre dans la table de filtres peut éventuellement spécifier une liste de points de terminaison de sauvegarde, utilisés en cas de défaillance de la transmission lors de l'envoi au point de terminaison primaire. Si une telle défaillance se produit, le service de routage essaie de transmettre le message à la première entrée de la liste des points de terminaison de sauvegarde. Si cette tentative d'envoi rencontre également un échec de transmission, il essaie avec le point de terminaison de sauvegarde suivant sur la liste. Le service de routage continue à envoyer le message à chaque point de terminaison de la liste jusqu'à ce que le message soit correctement reçu, que tous les points de terminaison aient retourné un échec de transmission, ou qu'un point de terminaison retourne un autre échec que celui de la transmission.  
  
 Les exemples suivants configurent le service de routage de manière à utiliser une liste de sauvegarde.  
  
```xml  
<routing>  
  <filters>  
    <!-- Create a MatchAll filter that catches all messages -->  
    <filter name="MatchAllFilter1" filterType="MatchAll" />  
  </filters>  
  <filterTables>  
    <!-- Set up the Routing Service's Message Filter Table -->  
    <filterTable name="filterTable1">  
        <!-- Add an entry that maps the MatchAllMessageFilter to the dead destination -->  
        <!-- If that endpoint is down, tell the Routing Service to try the endpoints -->  
        <!-- Listed in the backupEndpointList -->  
        <add filterName="MatchAllFilter1" endpointName="deadDestination" backupList="backupEndpointList"/>  
    </filterTable>  
  </filterTables>  
  <!-- Create the backup endpoint list -->  
  <backupLists>  
    <!-- Add an endpoint list that contains the backup destinations -->  
    <backupList name="backupEndpointList">  
      <add endpointName="realDestination" />  
      <add endpointName="backupDestination" />  
    </backupList>  
  </backupLists>  
</routing>  
```  
  
```csharp  
//create the endpoint list that contains the service endpoints we want to route to  
List<ServiceEndpoint> backupList = new List<ServiceEndpoint>();  
//add the endpoints in the order that the Routing Service should contact them  
//first add the endpoint that we know is down  
//clearly, normally you wouldn't know that this endpoint was down by default  
backupList.Add(fakeDestination);  
//then add the real Destination endpoint  
//the Routing Service attempts to send to this endpoint only if it   
//encounters a TimeOutException or CommunicationException when sending  
//to the previous endpoint in the list.  
backupList.Add(realDestination);  
//add the backupDestination endpoint  
//the Routing Service attempts to send to this endpoint only if it  
//encounters a TimeOutException or CommunicationsException when sending  
//to the previous endpoints in the list  
backupList.Add(backupDestination);  
//create the default RoutingConfiguration option              
RoutingConfiguration rc = new RoutingConfiguration();  
//add a MatchAll filter to the Routing Configuration's filter table  
//map it to the list of endpoints defined above  
//when a message matches this filter, it is sent to the endpoints in the list in order  
//if an endpoint is down or does not respond (which the first endpoint won't  
//since the client does not exist), the Routing Service automatically moves the message  
//to the next endpoint in the list and try again.  
rc.FilterTable.Add(new MatchAllMessageFilter(), backupList);  
```  
  
### <a name="supported-error-patterns"></a>Modèles d'erreurs pris en charge  
 Le tableau suivant décrit les modèles compatibles avec l'utilisation des listes de point de terminaison de sauvegarde, avec une description des détails de gestion des erreurs pour chaque modèle spécifique.  
  
|Modèle|Session|Transaction|Contexte de réception|Liste de sauvegarde prise en charge|Remarques|  
|-------------|-------------|-----------------|---------------------|---------------------------|-----------|  
|One-Way||||Oui|Tente de renvoyer le message sur un point de terminaison de sauvegarde. Si ce message est en mode multidiffusion, seul le message sur le canal en échec est déplacé vers sa destination de sauvegarde.|  
|One-Way||![Case à cocher](media/checkmark.gif "coche")||Non|Une exception est levée et la transaction est restaurée.|  
|One-Way|||![Case à cocher](media/checkmark.gif "coche")|Oui|Tente de renvoyer le message sur un point de terminaison de sauvegarde. Une fois le message correctement reçu, effectuez tous les contextes de réception. Si le message n'a pas été correctement reçu par un point de terminaison, n'effectuez pas le contexte de réception.<br /><br /> Lorsque ce message est envoyé en mode multidiffusion, le contexte de réception n'est effectué que si le message a été correctement reçu par au moins un point de terminaison (primaire ou de sauvegarde). Si aucun des points de terminaison situés sur les chemins d'accès de multidiffusion ne reçoit correctement le message, n'effectuez pas le contexte de réception.|  
|One-Way||![Case à cocher](media/checkmark.gif "coche")|![Case à cocher](media/checkmark.gif "coche")|Oui|Abandonnez la transaction précédente, créez une nouvelle transaction et renvoyez tous les messages. Les messages qui ont rencontré une erreur sont transmis à une destination de sauvegarde.<br /><br /> Une fois créée une transaction dont toutes les transmissions ont réussi, effectuez les contextes de réception et validez la transaction.|  
|One-Way|![Case à cocher](media/checkmark.gif "coche")|||Oui|Tente de renvoyer le message sur un point de terminaison de sauvegarde. Dans un scénario de multidiffusion, seuls les messages qui se trouvent dans une session ayant rencontré une erreur ou dont la fin de session a échoué sont renvoyés aux destinations de sauvegarde.|  
|One-Way|![Case à cocher](media/checkmark.gif "coche")|![Case à cocher](media/checkmark.gif "coche")||Non|Une exception est levée et la transaction est restaurée.|  
|One-Way|![Case à cocher](media/checkmark.gif "coche")||![Case à cocher](media/checkmark.gif "coche")|Oui|Tente de renvoyer le message sur un point de terminaison de sauvegarde. Une fois tous les messages envoyés sans erreur, la session indique qu'il n'y a plus de messages, le service de routage ferme correctement tous les canaux de session sortante, tous les contextes de réception sont effectués et le canal de session entrante est fermé.|  
|One-Way|![Case à cocher](media/checkmark.gif "coche")|![Case à cocher](media/checkmark.gif "coche")|![Case à cocher](media/checkmark.gif "coche")|Oui|Abandonnez la transaction actuelle et créez en une nouvelle. Renvoyez tous les messages précédents de la session. Une fois créée une transaction dont tous les messages ont été correctement envoyés et lorsque la session indique qu'il n'y a plus de messages, tous les canaux de session sortante sont fermés, les contextes de réception sont tous effectués avec la transaction, le canal de session entrante est fermé et la transaction est validée.<br /><br /> Lorsque les sessions sont en mode multidiffusion, les messages qui n'ont eu aucune erreur sont renvoyés à la même destination qu'avant, et les messages qui ont rencontré une erreur sont envoyés aux destinations de sauvegarde.|  
|Bidirectionnel||||Oui|Envoyez à une destination de sauvegarde.  Après qu'un canal a retourné un message de réponse, retournez la réponse au client d'origine.|  
|Bidirectionnel|![Case à cocher](media/checkmark.gif "coche")|||Oui|Envoyez tous les messages sur le canal à une destination de sauvegarde.  Après qu'un canal a retourné un message de réponse, retournez la réponse au client d'origine.|  
|Bidirectionnel||![Case à cocher](media/checkmark.gif "coche")||Non|Une exception est levée et la transaction est restaurée.|  
|Bidirectionnel|![Case à cocher](media/checkmark.gif "coche")|![Case à cocher](media/checkmark.gif "coche")||Non|Une exception est levée et la transaction est restaurée.|  
|Duplex||||Non|La communication duplex sans session n'est pas prise en charge actuellement.|  
|Duplex|![Case à cocher](media/checkmark.gif "coche")|||Oui|Envoyez à une destination de sauvegarde.|  
  
## <a name="hosting"></a>Hébergement  
 Le service de routage étant implémenté en tant que service WCF, il doit être soit auto-hébergé dans une application, soit hébergé par les services IIS ou WAS. Il est préférable que le service de routage soit hébergé dans les services IIS ou WAS ou dans une application de service Windows afin de tirer parti des fonctionnalités de démarrage automatique et de gestion du cycle de vie, disponibles dans ces environnements d'hébergement.  
  
 L'exemple suivant montre l'hébergement du service de routage dans une application.  
  
```csharp  
using (ServiceHost serviceHost =  
                new ServiceHost(typeof(RoutingService)))  
```  
  
 Pour héberger le service de routage dans les services IIS ou WAS, vous devez créer un fichier de service (.svc) ou utiliser une activation du service basée sur la configuration. Lorsque vous utilisez un fichier de service, vous devez spécifier un objet <xref:System.ServiceModel.Routing.RoutingService> à l'aide du paramètre du service. L'exemple suivant contient un exemple de fichier de service qui peut être utilisé pour héberger le service de routage avec les services IIS ou WAS.  
  
```  
<%@ ServiceHost Language="C#" Debug="true" Service="System.ServiceModel.Routing.RoutingService,   
     System.ServiceModel.Routing, version=4.0.0.0, Culture=neutral,   
     PublicKeyToken=31bf3856ad364e35" %>  
```  
  
## <a name="routing-service-and-impersonation"></a>Service de routage et emprunt d'identité  
 Le service de routage WCF peut être utilisé avec l'emprunt d'identité pour l'envoi et la réception de messages. Toutes les contraintes d'emprunt d'identité habituelles de Windows s'appliquent. Si vous devez configurer des autorisations de service ou de compte pour utiliser l'emprunt d'identité lors de l'écriture de votre service, vous devez effectuer ces mêmes étapes pour utiliser l'emprunt d'identité avec le service de routage. Pour plus d’informations, consultez [délégation et emprunt d’identité](delegation-and-impersonation-with-wcf.md).  
  
 L'emprunt d'identité avec le service de routage requiert l'utilisation de l'emprunt d'identité ASP.NET en mode de compatibilité ASP.NET ou l'utilisation d'informations d'identification Windows qui ont été configurées pour permettre l'emprunt d'identité. Pour plus d’informations sur le mode de compatibilité ASP.NET, consultez [Services WCF et ASP.NET](wcf-services-and-aspnet.md).  
  
> [!WARNING]
>  Le service de routage WCF ne prend pas en charge l'emprunt d'identité avec l'authentification de base.  
  
 Pour utiliser l'emprunt d'identité ASP.NET avec le service de routage, activez le mode de compatibilité ASP.NET sur l'environnement d'hébergement de service. Le service de routage a déjà été marqué comme autorisant le mode de compatibilité ASP.NET et l'emprunt d'identité est automatiquement activé. L'emprunt d'identité est la seule utilisation prise en charge de l'intégration d'ASP.NET au service de routage.  
  
 Pour utiliser l'emprunt d'identité des informations d'identification Windows avec le service de routage, vous devez configurer les informations d'identification et le service. L'objet d'informations d'identification du client (<xref:System.ServiceModel.Security.WindowsClientCredential>, accessible à partir de <xref:System.ServiceModel.ChannelFactory>) définit une propriété <xref:System.ServiceModel.Security.WindowsClientCredential.AllowedImpersonationLevel%2A> qui doit être définie pour permettre l'emprunt d'identité. Enfin, dans le service vous devez configurer le comportement <xref:System.ServiceModel.Description.ServiceAuthorizationBehavior> pour affecter la valeur `ImpersonateCallerForAllOperations` à `true`. Le service de routage utilise cet indicateur pour déterminer s'il faut créer les clients pour transférer des messages lorsque l'emprunt d'identité est activé.  
  
## <a name="see-also"></a>Voir aussi  
 [Filtres de message](message-filters.md)  
 [Contrats de routage](routing-contracts.md)  
 [Choix d’un filtre](choosing-a-filter.md)
