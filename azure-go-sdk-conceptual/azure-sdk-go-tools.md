---
title: Outils pour les développeurs Go
description: Outils permettant de travailler avec le kit de développement logiciel Microsoft Azure SDK pour Go et les services Azure
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 01/30/2018
ms.topic: conceptual
ms.prod: azure
ms.technology: azure-sdk-go
ms.devlang: go
ms.openlocfilehash: 006d140bffb66fdd769a14511232d4ea5081811d
ms.sourcegitcommit: 181d4e0b164cf39b3feac346f559596bd19c94db
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38066980"
---
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a>Outils pour les développeurs qui utilisent le kit de développement logiciel Microsoft Azure SDK pour Go

Pour plus d’efficacité dans l’écriture du code Go et pour un fonctionnement transparent avec les services Azure, voici certains outils recommandés.

## <a name="azure-cli-20"></a>Azure CLI 2.0

Azure CLI 2.0 fournit une interface de ligne de commande pour créer et configurer des ressources Azure dans vos abonnements. L’interface CLI peut vous aider à découvrir rapidement la création de ressources Azure communes et partagées, afin de pouvoir vous concentrer sur l’utilisation de services plus complexe. L’interface CLI dispose de fonctionnalités de filtrage et de requête vous permettant de diriger la sortie vers votre outil de ligne de commande préféré. L’interface CLI est disponible pour l’installation sur votre système local, en tant qu’image Docker ou via [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).

> [!div class="nextstepaction"]
> [Installer Azure CLI 2.0](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a>Visual Studio Code

Visual Studio Code est un éditeur léger offrant une prise en charge complète pour le langage Go via des extensions. Ces extensions incluent la prise en charge de fonctionnalités telles que la saisie semi-automatique, les modèles `impl`, la refactorisation et le débogage. Visual Studio Code offre également de nombreuses extensions pour les outils de développement courants tels que le contrôle de code source et offre même des extensions pour des interactions directes avec les services Azure. Microsoft gère une extension de métadonnées officielle comprenant ces extensions Azure, dont une interface interactive pour l’interface de ligne de commande Azure.

* [Installation de Visual Studio Code](https://code.visualstudio.com/Download)
* [Obtenir l’extension Go de Visual Studio Code](https://code.visualstudio.com/docs/languages/go)
* [Obtenir l’extension Azure Tools](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="dependency-management-with-dep"></a>Gestion des dépendances avec dep

Il existe plusieurs façons de gérer vos dépendances de package et de faire du vendoring avec Go, car il n’existe encore aucune solution officielle. La méthode recommandée pour effectuer cette gestion est d’utiliser le gestionnaire de dépendances `dep`. Le kit de développement logiciel (SDK) Azure pour Go utilise dep pour son vendoring et est assuré d’obtenir correctement les dépendances pour n’importe quel autre projet à l’aide de dep.

> [!div class="nextstepaction"]
> [Obtenir le gestionnaire de dépendances dep](https://github.com/golang/dep)
