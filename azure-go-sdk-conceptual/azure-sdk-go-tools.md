---
title: Outils pour les développeurs qui utilisent le kit de développement logiciel Microsoft Azure SDK pour Go
description: Outils permettant de travailler avec le kit de développement logiciel Microsoft Azure SDK pour Go et les services Azure
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 09/05/2018
ms.topic: conceptual
ms.prod: azure
ms.technology: azure-sdk-go
ms.devlang: go
ms.openlocfilehash: 70cf7d645f47df29e8e42599a0acd75858144783
ms.sourcegitcommit: 8b9e10b960150dc08f046ab840d6a5627410db29
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44059201"
---
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a>Outils pour les développeurs qui utilisent le kit de développement logiciel Microsoft Azure SDK pour Go

Pour plus d’efficacité dans l’écriture du code Go et pour un fonctionnement transparent avec les services Azure, voici certains outils recommandés.

## <a name="azure-cli"></a>Azure CLI

Azure CLI fournit une interface de ligne de commande pour créer et configurer des ressources Azure dans vos abonnements. L’interface CLI peut vous aider à découvrir rapidement la création de ressources Azure communes et partagées, afin de pouvoir vous concentrer sur l’utilisation de services plus complexe. L’interface CLI dispose de fonctionnalités de filtrage et de requête vous permettant de diriger la sortie vers votre outil de ligne de commande préféré. L’interface CLI est disponible pour l’installation sur votre système local, en tant qu’image Docker ou via [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).

> [!div class="nextstepaction"]
> [Installer l’interface de ligne de commande Microsoft Azure](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a>Visual Studio Code

Visual Studio Code est un éditeur léger qui prend en charge Go. Cette extension inclut des fonctionnalités telles que l’auto-complétion, les modèles `impl`, la refactorisation et le débogage. Visual Studio Code offre également la prise en charge d’un accès au sein de l’éditeur pour le contrôle de code source, ainsi que des extensions pour utiliser les services Azure.

* [Installation de Visual Studio Code](https://code.visualstudio.com/Download)
* [Obtenir l’extension Go de Visual Studio Code](https://code.visualstudio.com/docs/languages/go)
* [Obtenir l’extension Azure Tools de Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="cicd-with-azure-devops-project"></a>CI/CD avec Azure DevOps Project

Les pipelines de projet Azure DevOps vous permettent de configurer un système d’intégration continue pour vos applications Go. Vous avez uniquement besoin d’un référentiel Git pour pouvoir effectuer des déploiements et des tests directement sur Azure.

> [!div class="nextstepaction"]
> [Découvrez comment créer un pipeline CI/CD avec Azure DevOps Project](/azure/devops-project/azure-devops-project-go)

## <a name="dependency-management-with-dep"></a>Gestion des dépendances avec dep

Azure SDK pour Go utilise dep pour la gestion des dépendances. La commande dep vous permet d’extraire les exigences de fournisseurs pour votre application Go, ce qui évite les conflits de versions et garantit le bon fonctionnement de votre projet.

> [!div class="nextstepaction"]
> [Obtenir le gestionnaire de dépendances dep](https://github.com/golang/dep)
