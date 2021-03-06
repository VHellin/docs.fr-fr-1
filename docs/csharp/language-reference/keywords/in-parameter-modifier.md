---
title: in, modificateur de paramètre (référence C#)
ms.date: 03/06/2018
ms.prod: .net
ms.technology:
- devlang-csharp
ms.topic: article
helpviewer_keywords:
- parameters [C#], in
- in parameters [C#]
author: BillWagner
ms.author: wiwagn
ms.openlocfilehash: 9b8b21e2bdc95829c831ee71f24b47986321b7d0
ms.sourcegitcommit: c883637b41ee028786edceece4fa872939d2e64c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/23/2018
---
# <a name="in-parameter-modifier-c-reference"></a>in, modificateur de paramètre (référence C#)

Le mot clé `in` entraîne le passage des arguments par référence. Il est similaire aux mots clés [ref](ref.md) ou [out](out-parameter-modifier.md), sauf que les arguments `in` ne peuvent pas être modifiés par la méthode appelée, tandis que les arguments `ref` peuvent l’être, les arguments `out` doivent être modifiés par l’appelant et ces modifications sont observables dans le contexte d’appel.

[!code-csharp-interactive[cs-in-keyword](../../../../samples/snippets/csharp/language-reference/keywords/in-ref-out-modifier/InParameterModifier.cs#1)]  

L’exemple précédent montre que le modificateur `in` n’est pas nécessaire sur le site d’appel. Il l’est uniquement dans la déclaration de méthode.

> [!NOTE] 
> Le mot clé `in` peut également être utilisé avec un paramètre de type générique pour spécifier que le paramètre de type est contravariant, dans le cadre d’une instruction `foreach` ou d’une clause `join` dans une requête LINQ. Pour plus d’informations sur l’utilisation du mot clé `in` dans ces contextes, consultez [in](in.md) qui fournit des liens vers toutes ces utilisations.
  
 Les variables passées comme des arguments `in` doivent être initialisées avant d’être passées dans un appel de méthode. Toutefois, la méthode appelée ne peut pas attribuer de valeur ou modifier l’argument.  
  
 Bien que les mots clés `in`, `ref` et `out` entraînent un comportement différent au moment de l’exécution, ils ne sont pas considérés comme faisant partie de la signature de la méthode au moment de la compilation. Par conséquent, les méthodes ne peuvent pas être surchargées si la seule différence est que l’une d’elles accepte un argument `ref` ou `in` et que l’autre accepte un argument `out`. Le code suivant, par exemple, ne se compilera pas :  
  
```csharp
class CS0663_Example
{
    // Compiler error CS0663: "Cannot define overloaded 
    // methods that differ only on in, ref and out".
    public void SampleMethod(in int i) { }
    public void SampleMethod(ref int i) { }
}
```
  
La surcharge basée sur la présence de `in` est autorisée, mais génère un avertissement du compilateur :  
  
```csharp
class InOverloads
{
    // Discouraged. Calling SampleMethod(value) is ambiguous.
    public void SampleMethod(in int i) { }
    public void SampleMethod(int i) { }
}
```

Les constantes ou les propriétés peuvent être passées comme des paramètres `in`, car la méthode d’appel peut ne pas modifier leurs valeurs.
  
Vous ne pouvez pas utiliser les mots clés `in`, `ref` ou `out` pour les types de méthodes suivants :  
  
- Méthodes async, que vous définissez à l’aide du modificateur [async](../../../csharp/language-reference/keywords/async.md).  
  
- Les méthodes Iterator, qui incluent une instruction [yield return](../../../csharp/language-reference/keywords/yield.md) ou `yield break`.  

En général, vous déclarez des arguments `in` afin d’éviter les opérations de copie nécessaires pour passer des arguments par valeur. C’est particulièrement utile lorsque les arguments sont des types valeur tels que les structures où les opérations de copie sont plus coûteuses que le passage par référence.

> [!WARNING]
>  Les paramètres `in` peuvent être encore plus coûteux s’ils sont utilisés à mauvais escient. Le compilateur risque de ne pas savoir si les méthodes de membres modifient l’état du struct. Quand le compilateur ne peut pas assurer que l’objet n’est pas modifié, il crée une copie défensive et appelle les références de membre à l’aide de cette copie. Toutes les modifications possibles sont apportées à cette copie défensive. Les deux façons d’éviter ces copies consistent à passer les paramètres `in` comme arguments `in` ou à définir des structures comme `readonly struct`.

## <a name="c-language-specification"></a>Spécification du langage C#  
 [!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]  
  
## <a name="see-also"></a>Voir aussi  
 [Référence C#](../../../csharp/language-reference/index.md)  
 [Guide de programmation C#](../../../csharp/programming-guide/index.md)  
 [Mots clés C#](../../../csharp/language-reference/keywords/index.md)  
 [Paramètres de méthodes](../../../csharp/language-reference/keywords/method-parameters.md)  
 [Sémantique de référence avec les types valeur](../../../csharp/reference-semantics-with-value-types.md)
