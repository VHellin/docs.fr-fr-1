---
title: "Attributs qui contrôlent la sérialisation XML"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net
ms.reviewer: 
ms.suite: 
ms.tgt_pltfrm: 
ms.topic: article
helpviewer_keywords:
- classes, serializing
- XmlSerializer class, serializing
- XML serialization, attributes
- attributes [.NET Framework], XML serialization
- serialization, attributes
- XML Schema, serializing
ms.assetid: 414b820f-a696-4206-b576-2711d85490c7
caps.latest.revision: "7"
author: dotnet-bot
ms.author: dotnetcontent
manager: wpickett
ms.workload:
- dotnet
- dotnetcore
ms.openlocfilehash: e93912fc221d5c76dd5a462e6141fae2d0f4ca16
ms.sourcegitcommit: e7f04439d78909229506b56935a1105a4149ff3d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/23/2017
---
# <a name="attributes-that-control-xml-serialization"></a>Attributs qui contrôlent la sérialisation XML
Vous pouvez appliquer les attributs du tableau suivant à des classes et des membres de classe pour contrôler la manière dont <xref:System.Xml.Serialization.XmlSerializer> sérialise ou désérialise une instance de la classe. Pour comprendre comment ces attributs contrôlent la sérialisation XML, consultez [Contrôle de la sérialisation XML à l’aide d’attributs](../../../docs/standard/serialization/controlling-xml-serialization-using-attributes.md).  
  
 Ces attributs peuvent également être utilisés pour contrôler les messages SOAP de style littéral générés par un service Web XML. Pour plus d’informations sur l’application de ces attributs à une méthode de services web XML, consultez [Sérialisation XML avec les services Web XML](../../../docs/standard/serialization/xml-serialization-with-xml-web-services.md).  
  
 Pour plus d’informations sur les attributs, consultez [Attributs](../../../docs/standard/attributes/index.md).  
  
|Attribut|S'applique à|Informations fournies|  
|---------------|----------------|---------------|  
|<xref:System.Xml.Serialization.XmlAnyAttributeAttribute>|Champ public, propriété, paramètre ou valeur de retour qui retourne un tableau d'objets <xref:System.Xml.XmlAttribute>.|Lors de la désérialisation, le tableau est rempli avec les objets <xref:System.Xml.XmlAttribute> qui représentent tous les attributs XML inconnus du schéma.|  
|<xref:System.Xml.Serialization.XmlAnyElementAttribute>|Champ public, propriété, paramètre ou valeur de retour qui retourne un tableau d'objets <xref:System.Xml.XmlElement>.|Lors de la désérialisation, le tableau est rempli avec les objets <xref:System.Xml.XmlElement> qui représentent tous les éléments XML inconnus du schéma.|  
|<xref:System.Xml.Serialization.XmlArrayAttribute>|Champ public, propriété, paramètre ou valeur de retour qui retourne un tableau d'objets complexes.|Les membres du tableau sont générés en tant que membres d'un tableau XML.|  
|<xref:System.Xml.Serialization.XmlArrayItemAttribute>|Champ public, propriété, paramètre ou valeur de retour qui retourne un tableau d'objets complexes.|Types dérivés qui peuvent être insérés dans un tableau. S'applique habituellement avec <xref:System.Xml.Serialization.XmlArrayAttribute>.|  
|<xref:System.Xml.Serialization.XmlAttributeAttribute>|Champ public, propriété, paramètre ou valeur de retour.|Le membre est sérialisé en tant qu'attribut XML.|  
|<xref:System.Xml.Serialization.XmlChoiceIdentifierAttribute>|Champ public, propriété, paramètre ou valeur de retour.|L'ambiguïté du membre peut être levée à l'aide d'une énumération.|  
|<xref:System.Xml.Serialization.XmlElementAttribute>|Champ public, propriété, paramètre ou valeur de retour.|Le champ ou la propriété est sérialisé en tant qu'élément XML.|  
|<xref:System.Xml.Serialization.XmlEnumAttribute>|Champ public qui est un identificateur d'énumération.|Nom d'élément d'un membre d'énumération.|  
|<xref:System.Xml.Serialization.XmlIgnoreAttribute>|Champs et propriétés publics.|La propriété ou le champ doit être ignoré lorsque la classe conteneur est sérialisée.|  
|<xref:System.Xml.Serialization.XmlIncludeAttribute>|Déclarations de classe dérivée publiques et valeurs de retour de méthodes publiques pour les documents WSDL (Web Services Description Language).|La classe doit être incluse lors de la génération de schémas (afin d'être reconnue en cas de sérialisation).|  
|<xref:System.Xml.Serialization.XmlRootAttribute>|Déclarations de classe publiques.|Contrôle la sérialisation XML de l'attribut cible en tant qu'élément racine XML. Utilisez l'attribut pour préciser l'espace de noms et le nom d'élément.|  
|<xref:System.Xml.Serialization.XmlTextAttribute>|Champs et propriétés publics.|La propriété ou le champ doit être sérialisé en tant que texte XML.|  
|<xref:System.Xml.Serialization.XmlTypeAttribute>|Déclarations de classe publiques.|Nom et espace de noms du type XML.|  
  
 En plus de ces attributs, qui se trouvent tous dans l'espace de noms <xref:System.Xml.Serialization>, vous pouvez également appliquer l'attribut <xref:System.ComponentModel.DefaultValueAttribute> à un champ. **DefaultValueAttribute** définit la valeur qui sera assignée automatiquement au membre si aucune valeur n’est spécifiée.  
  
 Pour contrôler la sérialisation XML encodée selon le protocole SOAP, consultez [Attributs qui contrôlent la sérialisation encodée selon le protocole SOAP](../../../docs/standard/serialization/attributes-that-control-encoded-soap-serialization.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Sérialisation XML et SOAP](../../../docs/standard/serialization/xml-and-soap-serialization.md)  
 <xref:System.Xml.Serialization.XmlSerializer>  
 [Contrôle de la sérialisation XML à l’aide d’attributs](../../../docs/standard/serialization/controlling-xml-serialization-using-attributes.md)  
 [Guide pratique pour spécifier un nom d’élément différent pour un flux XML](../../../docs/standard/serialization/how-to-specify-an-alternate-element-name-for-an-xml-stream.md)  
 [Guide pratique pour sérialiser un objet](../../../docs/standard/serialization/how-to-serialize-an-object.md)  
 [Guide pratique pour désérialiser un objet](../../../docs/standard/serialization/how-to-deserialize-an-object.md)
