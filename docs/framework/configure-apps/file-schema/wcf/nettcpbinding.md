---
title: '&lt;netTcpBinding&gt;'
ms.custom: ''
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: ''
ms.suite: ''
ms.technology:
- dotnet-clr
ms.tgt_pltfrm: ''
ms.topic: article
helpviewer_keywords:
- netTcpBinding Element
ms.assetid: 5c5104a7-8754-4335-8233-46a45322503e
caps.latest.revision: ''
author: dotnet-bot
ms.author: dotnetcontent
manager: wpickett
ms.workload:
- dotnet
ms.openlocfilehash: 057baf6d18cba61c0ceeb7d5152edcf082392310
ms.sourcegitcommit: c883637b41ee028786edceece4fa872939d2e64c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2018
---
# <a name="ltnettcpbindinggt"></a>&lt;netTcpBinding&gt;
Spécifie une liaison sécurisée, fiable et optimisée, adaptée à la communication entre ordinateurs. Par défaut, elle génère une pile de communication du runtime avec Windows Security pour la sécurité et l'authentification des messages, TCP pour la remise de messages et un encodage de message binaire.  
  
 \<system.ServiceModel>  
\<bindings>  
\<netTcpBinding>  
  
## <a name="syntax"></a>Syntaxe  
  
```xml  
<netTcpBinding>  
   <binding   
      closeTimeout="TimeSpan"  
      hostNameComparisonMode="StrongWildCard/Exact/WeakWildcard"  
      listenBacklog="Integer"  
      maxBufferPoolSize="integer"  
      maxBufferSize="Integer"  
      maxConnections="Integer"   
      maxReceivedMessageSize="Integer"  
      name="string"  
      openTimeout="TimeSpan"  
      portSharingEnabled="Boolean"  
      receiveTimeout="TimeSpan"  
      sendTimeout="TimeSpan"  
      transactionFlow="Boolean"   
      transactionProtocol="OleTransactions/WSAtomicTransactionOctober2004"   
            transferMode="Buffered/Streamed/StreamedRequest/StreamedResponse"  
  
      <reliableSession ordered="Boolean"  
            inactivityTimeout="TimeSpan"  
            enabled="Boolean" />  
      <security mode="None/Transport/Message/Both">  
            <message clientCredentialType="None/Windows/UserName/Certificate/CardSpace"  
                defaultProtectionLevel="None/Sign/EncryptAndSign"   
algorithmSuite="Basic128/Basic192/Basic256/Basic128Rsa15/Basic256Rsa15/TripleDes/TripleDesRsa15/Basic128Sha256/Basic192Sha256/TripleDesSha256/Basic128Sha256Rsa15/Basic192Sha256Rsa15/Basic256Sha256Rsa15/TripleDesSha256Rsa15" />  
            <transport clientCredentialType="None/Windows/Certificate"  
                protectionLevel="None/Sign/EncryptAndSign" />  
      </security>  
       <readerQuotas             maxArrayLength="Integer"            maxBytesPerRead="Integer"            maxDepth="Integer"             maxNameTableCharCount="Integer"                     maxStringContentLength="Integer" />   </binding>  
</netTcpBinding>  
```  
  
## <a name="attributes-and-elements"></a>Attributs et éléments  
 Les sections suivantes décrivent des attributs, des éléments enfants et des éléments parents.  
  
### <a name="attributes"></a>Attributs  
  
|Attribut|Description|  
|---------------|-----------------|  
|closeTimeout|<xref:System.TimeSpan> qui spécifie l'intervalle de temps prévu pour la réalisation d'une opération de fermeture. Cette valeur doit être supérieure ou égale à <xref:System.TimeSpan.Zero>. La valeur par défaut est 00:01:00.|  
|hostnameComparisonMode|Spécifie le mode de comparaison du nom d'hôte HTTP utilisé pour analyser des URI. Cet attribut est de type `System.ServiceModel.HostnameComparisonMode`, ce qui indique si le nom d'hôte est utilisé pour atteindre le service en cas de correspondance sur l'URI. La valeur par défaut est `StrongWildcard`, qui ignore le nom d'hôte dans la correspondance.|  
|listenBacklog|Entier positif qui spécifie le nombre maximal des canaux en attente d'être acceptés sur l'écouteur. Les connexions dépassant cette limite sont mises en file d'attente jusqu'à ce que de l'espace soit disponible en dessous cette limite. L'attribut `connectionTimeout` limite le temps que devra attendre un client pour être connecté avant de lever une exception de connexion. La valeur par défaut est 10.|  
|maxBufferPoolSize|Entier qui spécifie la taille maximale du pool de mémoires tampons pour cette liaison. La valeur par défaut est 512 x 1024 octets. De nombreuses parties de Windows Communication Foundation (WCF) utilisent des mémoires tampons. La création et la destruction des mémoires tampons à chaque utilisation sont chères, tout comme leur nettoyage. Avec les pools de mémoires tampons, vous pouvez prendre une mémoire tampon du pool, l'utiliser et la retourner au pool une fois que vous avez terminé. Ainsi, la surcharge de la création et de la destruction des mémoires tampons est évitée.|  
|maxBufferSize|Entier positif qui spécifie la taille maximale, en octets, de la mémoire tampon utilisée pour stocker des messages en mémoire.<br /><br /> Si l'attribut `transferMode` est égal à `Buffered`, cet attribut doit être égal à la valeur de l'attribut `maxReceivedMessageSize`.<br /><br /> Si l'attribut `transferMode` est égal à `Streamed`, cet attribut ne peut pas être supérieur à la valeur de l'attribut `maxReceivedMessageSize` et doit être au moins de la taille des en-têtes.<br /><br /> La valeur par défaut est 65536. Pour plus d'informations, consultez <xref:System.ServiceModel.Configuration.NetNamedPipeBindingElement.MaxBufferSize%2A>.|  
|maxConnections|Entier qui spécifie le nombre maximal de connexions sortantes et entrantes que le service créera/acceptera. Les connexions entrantes et sortantes sont comptées par rapport à une limite distincte spécifiée par cet attribut.<br /><br /> Les connexions entrantes dépassant cette limite sont mises en file d'attente jusqu'à ce que de l'espace soit disponible sous cette limite.<br /><br /> Les connexions sortantes dépassant cette limite sont mises en file d'attente jusqu'à ce que de l'espace soit disponible sous cette limite.<br /><br /> La valeur par défaut est 10.|  
|maxReceivedMessageSize|Entier positif qui spécifie la taille maximale du message, en octets, y compris les en-têtes, pouvant être reçu sur un canal configuré avec cette liaison. L'expéditeur d'un message qui dépasse cette limite se verra notifier une erreur SOAP. Ce dernier abandonne le message et crée une entrée d'événement dans le journal de suivi. La valeur par défaut est 65536.|  
|name|Chaîne qui contient le nom de configuration de la liaison. Cette valeur doit être unique car elle permet d'identifier la liaison. Depuis [!INCLUDE[netfx40_short](../../../../../includes/netfx40-short-md.md)], les liaisons et les comportements ne sont pas obligés d'avoir un nom. Pour plus d’informations sur la configuration par défaut et nommées liaisons et comportements, consultez [Configuration simplifiée](../../../../../docs/framework/wcf/simplified-configuration.md) et [simplifié la Configuration des Services WCF](../../../../../docs/framework/wcf/samples/simplified-configuration-for-wcf-services.md).|  
|openTimeout|<xref:System.TimeSpan> qui spécifie l'intervalle de temps prévu pour la réalisation d'une opération d'ouverture. Cette valeur doit être supérieure ou égale à <xref:System.TimeSpan.Zero>. La valeur par défaut est 00:01:00.|  
|portSharingEnabled|Valeur booléenne qui spécifie si le partage de port TCP est activé pour cette connexion. Si elle est définie à `false`, chaque de liaison utilise son propre port exclusif. Ce paramètre est uniquement pertinent aux services, du fait que les clients ne sont pas affectés.|  
|receiveTimeout|<xref:System.TimeSpan> qui spécifie l'intervalle de temps prévu pour la réalisation d'une opération de réception. Cette valeur doit être supérieure ou égale à <xref:System.TimeSpan.Zero>. La valeur par défaut est 00:10:00.|  
|sendTimeout|<xref:System.TimeSpan> qui spécifie l'intervalle de temps prévu pour la réalisation d'une opération d'envoi. Cette valeur doit être supérieure ou égale à <xref:System.TimeSpan.Zero>. La valeur par défaut est 00:01:00.|  
|transactionFlow|Valeur booléenne qui spécifie si la liaison prend en charge le flux WS-Transactions. La valeur par défaut est `false`.|  
|transactionProtocol|Spécifie le protocole de transaction à utiliser avec cette liaison. Les valeurs valides sont les suivantes :<br /><br /> -   OleTransactions<br />-   WSAtomicTransactionOctober2004<br /><br /> La valeur par défaut est OleTransactions. Cet attribut est de type <xref:System.ServiceModel.TransactionProtocol>.|  
|transferMode|Valeur <xref:System.ServiceModel.TransferMode> qui spécifie si les messages sont mis en mémoire tampon ou transmis en continu ou s'il s'agit d'une demande ou d'une réponse.|  
  
### <a name="child-elements"></a>Éléments enfants  
  
|Élément|Description|  
|-------------|-----------------|  
|[\<security>](../../../../../docs/framework/configure-apps/file-schema/wcf/security-of-nettcpbinding.md)|Définit les paramètres de sécurité de la liaison. Cet élément est de type <xref:System.ServiceModel.Configuration.NetTcpSecurityElement>.|  
|[\<readerQuotas>](http://msdn.microsoft.com/library/3e5e42ff-cef8-478f-bf14-034449239bfd)|Définit les contraintes sur la complexité des messages SOAP pouvant être traités par les points de terminaison configurés avec cette liaison. Cet élément est de type <xref:System.ServiceModel.Configuration.XmlDictionaryReaderQuotasElement>.|  
|[reliableSession](http://msdn.microsoft.com/library/9c93818a-7dfa-43d5-b3a1-1aafccf3a00b)|Spécifie si des sessions fiables sont établies entre les points de terminaison du canal.|  
  
### <a name="parent-elements"></a>Éléments parents  
  
|Élément|Description|  
|-------------|-----------------|  
|[\<bindings>](../../../../../docs/framework/configure-apps/file-schema/wcf/bindings.md)|Cet élément conserve une collection de liaisons standard et personnalisées.|  
  
## <a name="remarks"></a>Notes  
 Par défaut, cette liaison génère une pile de communication au moment de l’exécution, qui utilise la sécurité de transport, le protocole TCP pour la remise des messages et un encodage de message binaire. Cette liaison est une solution fournie par le système [!INCLUDE[indigo1](../../../../../includes/indigo1-md.md)] adaptée à la communication sur un réseau intranet.  
  
 La configuration par défaut de `netTcpBinding` est plus rapide que la configuration proposée par `wsHttpBinding`, mais elle n'est destinée qu'à la communication de [!INCLUDE[indigo2](../../../../../includes/indigo2-md.md)] à [!INCLUDE[indigo2](../../../../../includes/indigo2-md.md)]. Le comportement de sécurité est configurable à l'aide de l'attribut facultatif `securityMode`. L'utilisation de WS-ReliableMessaging peut être configurée à l'aide de l'attribut facultatif `reliableSessionEnabled`. Mais les fonctionnalités de messagerie fiable sont désactivées par défaut. Plus généralement, les liaisons fournies par le système HTTP telles que `wsHttpBinding` et `basicHttpBinding` sont configurées pour activer certains éléments par défaut, alors que la liaison `netTcpBinding` en désactive par défaut. Par conséquent, vous devez demander de l'aide explicitement, par exemple pour les spécifications WS - *. Ainsi, la configuration TCP par défaut permet d’échanger des messages entre points de terminaison plus rapidement que ceux configurés par défaut pour les liaisons HTTP.  
  
## <a name="example"></a>Exemple  
 La liaison est spécifiée dans les fichiers de configuration pour le client et le service. Le type de liaison est spécifié dans l'attribut `binding` de l'élément `<endpoint>`. Si vous souhaitez configurer la liaison netTcpBinding et modifier quelques-uns de ses paramètres, il est nécessaire de définir une configuration de liaison. Le point de terminaison doit référencer la configuration de liaison avec un attribut `bindingConfiguration`. Dans l’exemple suivant, une configuration de liaison est définie.  
  
```xml  
<services>  
  <service name="Microsoft.ServiceModel.Samples.CalculatorService"  
           behaviorConfiguration="CalculatorServiceBehavior">  
    ...  
    <endpoint address=""  
              binding="netTcpBinding"  
              contract="Microsoft.ServiceModel.Samples.ICalculator" />  
    ...  
  </service>  
</services>  
  
<bindings>  
  <netTcpBinding>  
    <binding   
             closeTimeout="00:01:00"  
             openTimeout="00:01:00"   
             receiveTimeout="00:10:00"   
             sendTimeout="00:01:00"  
             transactionFlow="false"   
             transferMode="Buffered"   
             transactionProtocol="OleTransactions"  
             hostNameComparisonMode="StrongWildcard"   
             listenBacklog="10"  
             maxBufferPoolSize="524288"   
             maxBufferSize="65536"   
             maxConnections="10"  
             maxReceivedMessageSize="65536">  
      <readerQuotas maxDepth="32"   
                    maxStringContentLength="8192"   
                    maxArrayLength="16384"  
                    maxBytesPerRead="4096"   
                    maxNameTableCharCount="16384" />  
      <reliableSession ordered="true"   
                       inactivityTimeout="00:10:00"  
                       enabled="false" />  
      <security mode="Transport">  
        <transport clientCredentialType="Windows" protectionLevel="EncryptAndSign" />  
      </security>  
    </binding>  
  </netTcpBinding>  
</bindings>  
```  
  
## <a name="see-also"></a>Voir aussi  
 <xref:System.ServiceModel.NetTcpBinding>  
 <xref:System.ServiceModel.Configuration.NetTcpBindingElement>  
 [Liaisons](../../../../../docs/framework/wcf/bindings.md)  
 [Configuration des liaisons fournies par le système](../../../../../docs/framework/wcf/feature-details/configuring-system-provided-bindings.md)  
 [Utilisation de liaisons pour configurer les Clients et les Services Windows Communication Foundation](http://msdn.microsoft.com/library/bd8b277b-932f-472f-a42a-b02bb5257dfb)  
 [\<binding>](../../../../../docs/framework/misc/binding.md)
