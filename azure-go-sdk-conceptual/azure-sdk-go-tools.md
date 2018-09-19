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
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a><span data-ttu-id="e09b0-103">Outils pour les développeurs qui utilisent le kit de développement logiciel Microsoft Azure SDK pour Go</span><span class="sxs-lookup"><span data-stu-id="e09b0-103">Tools for developers using the Azure SDK for Go</span></span>

<span data-ttu-id="e09b0-104">Pour plus d’efficacité dans l’écriture du code Go et pour un fonctionnement transparent avec les services Azure, voici certains outils recommandés.</span><span class="sxs-lookup"><span data-stu-id="e09b0-104">To be effective at writing Go code and have it work seamlessly with Azure services, here are some recommended tools.</span></span>

## <a name="azure-cli"></a><span data-ttu-id="e09b0-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="e09b0-105">Azure CLI</span></span>

<span data-ttu-id="e09b0-106">Azure CLI fournit une interface de ligne de commande pour créer et configurer des ressources Azure dans vos abonnements.</span><span class="sxs-lookup"><span data-stu-id="e09b0-106">The Azure CLI provides a command-line interface to create and configure Azure resources in your subscriptions.</span></span> <span data-ttu-id="e09b0-107">L’interface CLI peut vous aider à découvrir rapidement la création de ressources Azure communes et partagées, afin de pouvoir vous concentrer sur l’utilisation de services plus complexe.</span><span class="sxs-lookup"><span data-stu-id="e09b0-107">The CLI can help you get started building common and shared Azure resources quickly, so that you can focus on more complex usage of services.</span></span> <span data-ttu-id="e09b0-108">L’interface CLI dispose de fonctionnalités de filtrage et de requête vous permettant de diriger la sortie vers votre outil de ligne de commande préféré.</span><span class="sxs-lookup"><span data-stu-id="e09b0-108">The CLI has query and filtering features so you can pipe output directly to your favorite command-line tools.</span></span> <span data-ttu-id="e09b0-109">L’interface CLI est disponible pour l’installation sur votre système local, en tant qu’image Docker ou via [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).</span><span class="sxs-lookup"><span data-stu-id="e09b0-109">The CLI is available for installation on your local system, as a Docker image, or through [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="e09b0-110">Installer l’interface de ligne de commande Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="e09b0-110">Install the Azure CLI</span></span>](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a><span data-ttu-id="e09b0-111">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e09b0-111">Visual Studio Code</span></span>

<span data-ttu-id="e09b0-112">Visual Studio Code est un éditeur léger qui prend en charge Go.</span><span class="sxs-lookup"><span data-stu-id="e09b0-112">Visual Studio Code is a lightweight editor that offers Go support.</span></span> <span data-ttu-id="e09b0-113">Cette extension inclut des fonctionnalités telles que l’auto-complétion, les modèles `impl`, la refactorisation et le débogage.</span><span class="sxs-lookup"><span data-stu-id="e09b0-113">This extension offers features like autocomplete, `impl` templates, refactoring, and debugging.</span></span> <span data-ttu-id="e09b0-114">Visual Studio Code offre également la prise en charge d’un accès au sein de l’éditeur pour le contrôle de code source, ainsi que des extensions pour utiliser les services Azure.</span><span class="sxs-lookup"><span data-stu-id="e09b0-114">Visual Studio Code also offers support for in-editor access to source control, and extensions for working with Azure services.</span></span>

* [<span data-ttu-id="e09b0-115">Installation de Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e09b0-115">Install Visual Studio Code</span></span>](https://code.visualstudio.com/Download)
* [<span data-ttu-id="e09b0-116">Obtenir l’extension Go de Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e09b0-116">Get the Visual Studio Code Go extension</span></span>](https://code.visualstudio.com/docs/languages/go)
* [<span data-ttu-id="e09b0-117">Obtenir l’extension Azure Tools de Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e09b0-117">Get the Visual Studio Code Azure Tools extension</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="cicd-with-azure-devops-project"></a><span data-ttu-id="e09b0-118">CI/CD avec Azure DevOps Project</span><span class="sxs-lookup"><span data-stu-id="e09b0-118">CI/CD with Azure DevOps Project</span></span>

<span data-ttu-id="e09b0-119">Les pipelines de projet Azure DevOps vous permettent de configurer un système d’intégration continue pour vos applications Go.</span><span class="sxs-lookup"><span data-stu-id="e09b0-119">Azure DevOps Project pipelines allow you to set up a continuous integration system for your Go applications.</span></span> <span data-ttu-id="e09b0-120">Vous avez uniquement besoin d’un référentiel Git pour pouvoir effectuer des déploiements et des tests directement sur Azure.</span><span class="sxs-lookup"><span data-stu-id="e09b0-120">All it takes is a git repo, and you can deploy and test directly on Azure.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="e09b0-121">Découvrez comment créer un pipeline CI/CD avec Azure DevOps Project</span><span class="sxs-lookup"><span data-stu-id="e09b0-121">Learn how to create a CI/CD pipeline with Azure DevOps Project</span></span>](/azure/devops-project/azure-devops-project-go)

## <a name="dependency-management-with-dep"></a><span data-ttu-id="e09b0-122">Gestion des dépendances avec dep</span><span class="sxs-lookup"><span data-stu-id="e09b0-122">Dependency management with dep</span></span>

<span data-ttu-id="e09b0-123">Azure SDK pour Go utilise dep pour la gestion des dépendances.</span><span class="sxs-lookup"><span data-stu-id="e09b0-123">The Azure SDK for Go uses dep for dependency management.</span></span> <span data-ttu-id="e09b0-124">La commande dep vous permet d’extraire les exigences de fournisseurs pour votre application Go, ce qui évite les conflits de versions et garantit le bon fonctionnement de votre projet.</span><span class="sxs-lookup"><span data-stu-id="e09b0-124">The dep command allows you to pull and vendor requirements for your Go application, avoiding version conflicts and ensuring that your project works correctly.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="e09b0-125">Obtenir le gestionnaire de dépendances dep</span><span class="sxs-lookup"><span data-stu-id="e09b0-125">Get the dep dependency manager</span></span>](https://github.com/golang/dep)
