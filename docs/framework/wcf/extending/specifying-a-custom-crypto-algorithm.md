---
title: "Spécification d'un algorithme de chiffrement personnalisé"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology: dotnet-clr
ms.tgt_pltfrm: 
ms.topic: article
ms.assetid: d662a305-8e09-451d-9a59-b0f12b012f1d
caps.latest.revision: "2"
author: dotnet-bot
ms.author: dotnetcontent
manager: wpickett
ms.workload: dotnet
ms.openlocfilehash: 965f121faa851722e6e2e7f92e805252f7e927c5
ms.sourcegitcommit: 16186c34a957fdd52e5db7294f291f7530ac9d24
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/22/2017
---
# <a name="specifying-a-custom-crypto-algorithm"></a>Spécification d'un algorithme de chiffrement personnalisé
WCF vous permet de spécifier un algorithme de chiffrement personnalité pour le chiffrement des données ou le calcul de signatures numériques. Voici les étapes qui permettent d'effectuer cette opération :  
  
1.  Dériver une classe de <xref:System.ServiceModel.Security.SecurityAlgorithmSuite>  
  
2.  Enregistrer l'algorithme  
  
3.  Configurez la liaison avec la classe dérivée <xref:System.ServiceModel.Security.SecurityAlgorithmSuite>.  
  
## <a name="derive-a-class-from-securityalgorithmsuite"></a>Dériver une classe de SecurityAlgorithmSuite  
 La classe <xref:System.ServiceModel.Security.SecurityAlgorithmSuite> est une classe de base abstraite qui vous permet de spécifier l'algorithme à utiliser pour plusieurs opérations liées à la sécurité. Par exemple, calculer un hachage pour une signature numérique ou chiffrer un message. Le code suivant montre comment dériver une classe à partir de <xref:System.ServiceModel.Security.SecurityAlgorithmSuite> :  
  
```csharp  
public class MyCustomAlgorithmSuite : SecurityAlgorithmSuite  
    {  
        public override string DefaultAsymmetricKeyWrapAlgorithm  
        {  
            get { return SecurityAlgorithms.RsaOaepKeyWrap; }  
        }  
  
        public override string DefaultAsymmetricSignatureAlgorithm  
        {  
            get { return SecurityAlgorithms.RsaSha1Signature; }  
        }  
  
        public override string DefaultCanonicalizationAlgorithm  
        {  
            get { return SecurityAlgorithms.ExclusiveC14n; ; }  
        }  
  
        public override string DefaultDigestAlgorithm  
        {  
            get { return SecurityAlgorithms.MyCustomHashAlgorithm; }  
        }  
  
        public override string DefaultEncryptionAlgorithm  
        {  
            get { return SecurityAlgorithms.Aes128Encryption; }  
        }  
  
        public override int DefaultEncryptionKeyDerivationLength  
        {  
            get { return 128; }  
        }  
  
        public override int DefaultSignatureKeyDerivationLength  
        {  
            get { return 128; }  
        }  
  
        public override int DefaultSymmetricKeyLength  
        {  
            get { return 128; }  
        }  
  
        public override string DefaultSymmetricKeyWrapAlgorithm  
        {  
            get { return SecurityAlgorithms.Aes128Encryption; }  
        }  
  
        public override string DefaultSymmetricSignatureAlgorithm  
        {  
            get { return SecurityAlgorithms.HmacSha1Signature; }  
        }  
  
        public override bool IsAsymmetricKeyLengthSupported(int length)  
        {  
            return length >= 1024 && length <= 4096;  
        }  
  
        public override bool IsSymmetricKeyLengthSupported(int length)  
        {  
            return length >= 128 && length <= 256;  
        }  
    }  
```  
  
## <a name="register-the-custom-algorithm"></a>Enregistrer l'algorithme personnalisé  
 L'enregistrement peut être effectué dans un fichier de configuration ou en code impératif. L'enregistrement d'un algorithme personnalisé s'effectue en créant un mappage entre une classe qui implémente un fournisseur de service de chiffrement et un alias. L'alias est ensuite mappé sur un URI qui est utilisé pour spécifier l'algorithme dans la liaison du service WCF. L'extrait de configuration suivant illustre la méthode d'enregistrement d'un algorithme personnalisé :  
  
```xml  
<configuration>  
   <mscorlib>  
      <cryptographySettings>  
         <cryptoNameMapping>  
           <cryptoClasses>  
              <cryptoClass SHA256CSP="System.Security.Cryptography.SHA256CryptoServiceProvider, System.Core, Version=3.5.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />  
           </cryptoClasses>  
           <nameEntry name="http://constoso.com/CustomAlgorithms/CustomHashAlgorithm"  
              class="SHA256CSP" />  
           </cryptoNameMapping>  
        </cryptographySettings>  
    </mscorlib>  
</configuration>  
```  
  
 La section sous le <`cryptoClasses`> élément crée le mappage entre le SHA256CryptoServiceProvider et l’alias « SHA256CSP ». Le <`nameEntry`> élément crée le mappage entre l’alias « SHA256CSP » et l’URL spécifiée (http://constoso.com/CustomAlgorithms/CustomHashAlgorithm).  
  
 Pour enregistrer l'algorithme personnalisé en code, utilisez la méthode <xref:System.Security.Cryptography.CryptoConfig.AddAlgorithm(System.Type,System.String[])>. Cette méthode crée ces deux mappages. L'exemple suivant illustre l'appel de cette méthode :  
  
```  
// Register the custom URI string defined for the hashAlgorithm in MyCustomAlgorithmSuite class to create the   
// SHA256CryptoServiceProvider hash algorithm object.  
CryptoConfig.AddAlgorithm(typeof(SHA256CryptoServiceProvider), "http://constoso.com/CustomAlgorithms/CustomHashAlgorithm");  
```  
  
## <a name="configure-the-binding"></a>Configurer la liaison  
 Vous pouvez configurer la liaison en spécifiant la classe personnalisée dérivée de <xref:System.ServiceModel.Security.SecurityAlgorithmSuite> dans les paramètres de liaison, comme indiqué dans l'extrait de code suivant :  
  
```csharp  
WSHttpBinding binding = new WSHttpBinding();  
            binding.Security.Message.AlgorithmSuite = new MyCustomAlgorithmSuite();  
```  
  
 Pour obtenir un exemple de code complet, consultez la [agilité de chiffrement dans sécurité WCF](../../../../docs/framework/wcf/samples/cryptographic-agility-in-wcf-security.md) exemple.  
  
## <a name="see-also"></a>Voir aussi  
 [Sécurisation des services et des clients](../../../../docs/framework/wcf/feature-details/securing-services-and-clients.md)  
 [Sécurisation de services](../../../../docs/framework/wcf/securing-services.md)  
 [Vue d’ensemble de la sécurité](../../../../docs/framework/wcf/feature-details/security-overview.md)  
 [Concepts relatifs à la sécurité](../../../../docs/framework/wcf/feature-details/security-concepts.md)
