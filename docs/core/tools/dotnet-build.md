---
title: Commande dotnet build - Interface CLI .NET Core
description: "La commande dotnet build permet de générer un projet et l’ensemble de ses dépendances."
author: mairaw
ms.author: mairaw
ms.date: 03/10/2018
ms.topic: article
ms.prod: .net-core
ms.technology: dotnet-cli
ms.workload:
- dotnetcore
ms.openlocfilehash: e7181f502e2a25b17077366da9d9f071e7e94d33
ms.sourcegitcommit: 83dd5ec003e788ccb3e33f3412a7af39ae347646
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/15/2018
---
# <a name="dotnet-build"></a>dotnet-build

[!INCLUDE [topic-appliesto-net-core-all](../../../includes/topic-appliesto-net-core-all.md)]

## <a name="name"></a>Name

`dotnet build` : Génère un projet et l’ensemble de ses dépendances.

## <a name="synopsis"></a>Résumé

# <a name="net-core-2xtabnetcore2x"></a>[.NET Core 2.x](#tab/netcore2x)
```
dotnet build [<PROJECT>] [-c|--configuration] [-f|--framework] [--force] [--no-dependencies] [--no-incremental]
    [--no-restore] [-o|--output] [-r|--runtime] [-v|--verbosity] [--version-suffix]
dotnet build [-h|--help]
```
# <a name="net-core-1xtabnetcore1x"></a>[.NET Core 1.x](#tab/netcore1x)
```
dotnet build [<PROJECT>] [-c|--configuration] [-f|--framework] [--no-dependencies] [--no-incremental] [-o|--output]
    [-r|--runtime] [-v|--verbosity] [--version-suffix]
dotnet build [-h|--help]
```
---

## <a name="description"></a>Description

La commande `dotnet build` génère le projet et ses dépendances dans un ensemble de fichiers binaires. Les fichiers binaires incluent le code du projet dans des fichiers en langage intermédiaire (IL) avec une extension *.dll* et les fichiers de symboles utilisés pour le débogage avec une extension *.pdb*. Un fichier JSON de dépendances (*\*.deps.json*) est généré. Il répertorie les dépendances de l’application. Un fichier *\*.runtimeconfig.json* est généré. Il spécifie le runtime partagé et sa version pour l’application.

Si le projet a des dépendances tierces, comme des bibliothèques NuGet, elles sont résolues à partir du cache NuGet et ne sont pas disponibles avec la sortie générée du projet. Par conséquent, le produit de `dotnet build` ne peut pas être transféré en l’état vers un autre ordinateur pour exécution. Ce comportement contraste avec celui du .NET Framework dans lequel la génération d’un projet exécutable (une application) produit une sortie exécutable sur n’importe quel ordinateur où le .NET Framework est installé. Pour obtenir un résultat similaire dans .NET Core, vous devez utiliser la commande [dotnet publish](dotnet-publish.md). Pour plus d’informations, consultez [Déploiement d’applications .NET Core](../deploying/index.md).

La génération requiert le fichier *project.assets.json* qui répertorie les dépendances de votre application. Le fichier est créé quand la commande [`dotnet restore`](dotnet-restore.md) est exécutée. Si le fichier de ressources est absent, les outils ne peuvent pas résoudre les assemblys de référence, ce qui entraîne des erreurs. Avec le SDK .NET Core 1.x, vous deviez explicitement exécuter `dotnet restore` avant d’exécuter `dotnet build`. À compter du SDK .NET Core 2.0, `dotnet restore` s’exécute implicitement quand vous exécutez `dotnet build`. Si vous souhaitez désactiver la restauration implicite au moment d’exécuter la commande de génération, vous pouvez passer l’option `--no-restore`.

[!INCLUDE[dotnet restore note + options](~/includes/dotnet-restore-note-options.md)]

La commande `dotnet build` utilise MSBuild pour générer le projet. Elle prend donc en charge les builds parallèles et les builds incrémentielles. Pour plus d’informations, consultez [Builds incrémentielles](/visualstudio/msbuild/incremental-builds).

En plus de ses options, la commande `dotnet build` accepte des options MSBuild, comme `/p` pour définir des propriétés ou `/l` pour définir un enregistreur d’événements. En savoir plus sur ces options dans les [Informations de référence sur la ligne de commande MSBuild](/visualstudio/msbuild/msbuild-command-line-reference). 

La possibilité d’exécuter le projet ou non est déterminée par la propriété `<OutputType>` dans le fichier projet. L’exemple suivant illustre un projet qui génère du code exécutable :

```xml
<PropertyGroup>
  <OutputType>Exe</OutputType>
</PropertyGroup>
```

Pour générer une bibliothèque, omettez la propriété `<OutputType>`. La principale différence dans la sortie générée est que la DLL de langage intermédiaire pour une bibliothèque ne contient pas de points d’entrée et ne peut pas être exécutée. 

## <a name="arguments"></a>Arguments

`PROJECT`

Fichier projet à générer. Si vous ne spécifiez pas de fichier projet, MSBuild recherche dans le répertoire de travail actuel un fichier avec une extension se terminant par *proj* et l’utilise.

## <a name="options"></a>Options

# <a name="net-core-2xtabnetcore2x"></a>[.NET Core 2.x](#tab/netcore2x)

`-c|--configuration {Debug|Release}`

Définit la configuration de build. La valeur par défaut est `Debug`.

`-f|--framework <FRAMEWORK>`

Compile pour un [framework](../../standard/frameworks.md) spécifique. Le framework doit être défini dans le [fichier projet](csproj.md).

`--force`

 Force la résolution de toutes les dépendances même si la dernière restauration a réussi. Cette opération équivaut à supprimer le fichier *project.assets.json*.

`-h|--help`

Affiche une aide brève pour la commande.

`--no-dependencies`

Ignore les références entre projets (P2P) et génère uniquement le projet racine spécifié pour la génération.

`--no-incremental`

Marque la build comme unsafe pour la génération incrémentielle. Cela désactive la compilation incrémentielle et force une régénération du graphique de dépendance du projet.

`--no-restore`

Ne pas effectuer de restauration implicite pendant la génération.

`-o|--output <OUTPUT_DIRECTORY>`

Répertoire dans lequel placer les fichiers binaires générés. Vous devez également définir `--framework` lorsque vous spécifiez cette option.

`-r|--runtime <RUNTIME_IDENTIFIER>`

Spécifie le runtime cible. Pour connaître les identificateurs de runtime, consultez le [catalogue des identificateurs de runtime](../rid-catalog.md).

`-v|--verbosity <LEVEL>`

Définit le niveau de détail de la commande. Les valeurs autorisées sont `q[uiet]`, `m[inimal]`, `n[ormal]`, `d[etailed]` et `diag[nostic]`.

`--version-suffix <VERSION_SUFFIX>`

Définit le suffixe de version pour un astérisque (`*`) dans le champ de version du fichier projet. Le format respecte les instructions de version de NuGet.

# <a name="net-core-1xtabnetcore1x"></a>[.NET Core 1.x](#tab/netcore1x)

`-c|--configuration {Debug|Release}`

Définit la configuration de build. La valeur par défaut est `Debug`.

`-f|--framework <FRAMEWORK>`

Compile pour un [framework](../../standard/frameworks.md) spécifique. Le framework doit être défini dans le [fichier projet](csproj.md).

`-h|--help`

Affiche une aide brève pour la commande.

`--no-dependencies`

Ignore les références entre projets (P2P) et génère uniquement le projet racine spécifié pour la génération.

`--no-incremental`

Marque la build comme unsafe pour la génération incrémentielle. Cela désactive la compilation incrémentielle et force une régénération du graphique de dépendance du projet.

`-o|--output <OUTPUT_DIRECTORY>`

Répertoire dans lequel placer les fichiers binaires générés. Vous devez également définir `--framework` lorsque vous spécifiez cette option.

`-r|--runtime <RUNTIME_IDENTIFIER>`

Spécifie le runtime cible. Pour connaître les identificateurs de runtime, consultez le [catalogue des identificateurs de runtime](../rid-catalog.md).

`-v|--verbosity <LEVEL>`

Définit le niveau de détail de la commande. Les valeurs autorisées sont `q[uiet]`, `m[inimal]`, `n[ormal]`, `d[etailed]` et `diag[nostic]`.

`--version-suffix <VERSION_SUFFIX>`

Définit le suffixe de version pour un astérisque (`*`) dans le champ de version du fichier projet. Le format respecte les instructions de version de NuGet.

---

## <a name="examples"></a>Exemples

Générer un projet et ses dépendances :

`dotnet build`

Générer un projet et ses dépendances à l’aide de la configuration Release :

`dotnet build --configuration Release`

Générer un projet et ses dépendances pour un runtime spécifique (dans cet exemple, Ubuntu 16.04) :

`dotnet build --runtime ubuntu.16.04-x64`

Générer le projet et utiliser la source de package NuGet spécifiée pendant l’opération de restauration (SDK .NET Core 2.0 et versions ultérieures) :

`dotnet build --source c:\packages\mypackages`