---
title: Outils pour les développeurs Go
description: Outils permettant de travailler avec le kit de développement logiciel Microsoft Azure SDK pour Go et les services Azure
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 07/13/2018
ms.topic: conceptual
ms.prod: azure
ms.technology: azure-sdk-go
ms.devlang: go
ms.openlocfilehash: dfa3912ac13e6f6d52d607f9dcc150f3a5b57602
ms.sourcegitcommit: d1790b317a8fcb4d672c654dac2a925a976589d4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39039503"
---
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a><span data-ttu-id="654c6-103">Outils pour les développeurs qui utilisent le kit de développement logiciel Microsoft Azure SDK pour Go</span><span class="sxs-lookup"><span data-stu-id="654c6-103">Tools for developers using the Azure SDK for Go</span></span>

<span data-ttu-id="654c6-104">Pour plus d’efficacité dans l’écriture du code Go et pour un fonctionnement transparent avec les services Azure, voici certains outils recommandés.</span><span class="sxs-lookup"><span data-stu-id="654c6-104">To be effective at writing Go code and have it work seamlessly with Azure services, here are some recommended tools.</span></span>

## <a name="azure-cli"></a><span data-ttu-id="654c6-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="654c6-105">Azure CLI</span></span>

<span data-ttu-id="654c6-106">Azure CLI fournit une interface de ligne de commande pour créer et configurer des ressources Azure dans vos abonnements.</span><span class="sxs-lookup"><span data-stu-id="654c6-106">The Azure CLI provides a command-line interface to create and configure Azure resources in your subscriptions.</span></span> <span data-ttu-id="654c6-107">L’interface CLI peut vous aider à découvrir rapidement la création de ressources Azure communes et partagées, afin de pouvoir vous concentrer sur l’utilisation de services plus complexe.</span><span class="sxs-lookup"><span data-stu-id="654c6-107">The CLI can help you get started building common and shared Azure resources quickly, so that you can focus on more complex usage of services.</span></span> <span data-ttu-id="654c6-108">L’interface CLI dispose de fonctionnalités de filtrage et de requête vous permettant de diriger la sortie vers votre outil de ligne de commande préféré.</span><span class="sxs-lookup"><span data-stu-id="654c6-108">The CLI has query and filtering features so you can pipe output directly to your favorite command-line tools.</span></span> <span data-ttu-id="654c6-109">L’interface CLI est disponible pour l’installation sur votre système local, en tant qu’image Docker ou via [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).</span><span class="sxs-lookup"><span data-stu-id="654c6-109">The CLI is available for installation on your local system, as a Docker image, or through [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="654c6-110">Installer l’interface de ligne de commande Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="654c6-110">Install the Azure CLI</span></span>](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a><span data-ttu-id="654c6-111">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="654c6-111">Visual Studio Code</span></span>

<span data-ttu-id="654c6-112">Visual Studio Code est un éditeur léger offrant une prise en charge complète pour le langage Go via des extensions.</span><span class="sxs-lookup"><span data-stu-id="654c6-112">Visual Studio Code is a lightweight editor that has comprehensive support for the Go language through extensions.</span></span> <span data-ttu-id="654c6-113">Ces extensions incluent la prise en charge de fonctionnalités telles que la saisie semi-automatique, les modèles `impl`, la refactorisation et le débogage.</span><span class="sxs-lookup"><span data-stu-id="654c6-113">These extensions include support for features like autocomplete, `impl` templates, refactoring, and debugging.</span></span> <span data-ttu-id="654c6-114">Visual Studio Code offre également de nombreuses extensions pour les outils de développement courants tels que le contrôle de code source et offre même des extensions pour des interactions directes avec les services Azure.</span><span class="sxs-lookup"><span data-stu-id="654c6-114">Visual Studio Code also offers many extensions for common developer tools such as source control, and even offers extensions for direct interactions with Azure services.</span></span> <span data-ttu-id="654c6-115">Microsoft gère une extension de métadonnées officielle comprenant ces extensions Azure, dont une interface interactive pour l’interface de ligne de commande Azure.</span><span class="sxs-lookup"><span data-stu-id="654c6-115">Microsoft maintains an official meta-extension including these Azure extensions, including an interactive interface for the Azure CLI.</span></span>

* [<span data-ttu-id="654c6-116">Installation de Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="654c6-116">Install Visual Studio Code</span></span>](https://code.visualstudio.com/Download)
* [<span data-ttu-id="654c6-117">Obtenir l’extension Go de Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="654c6-117">Get the Visual Studio Code Go extension</span></span>](https://code.visualstudio.com/docs/languages/go)
* [<span data-ttu-id="654c6-118">Obtenir l’extension Azure Tools</span><span class="sxs-lookup"><span data-stu-id="654c6-118">Get the Azure Tools extension</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="cicd-with-azure-devops-project"></a><span data-ttu-id="654c6-119">CI/CD avec Azure DevOps Project</span><span class="sxs-lookup"><span data-stu-id="654c6-119">CI/CD with Azure DevOps Project</span></span>

<span data-ttu-id="654c6-120">Avec le pipeline Azure DevOps Project, vous pouvez configurer une intégration continue et un déploiement continu pour vos applications Go.</span><span class="sxs-lookup"><span data-stu-id="654c6-120">With the Azure DevOps Project pipeline, you can set up a continuous build and deploy for your Go applications.</span></span> <span data-ttu-id="654c6-121">Vous n’avez besoin que d’un référentiel Git disponible, et vous pouvez configurer le déploiement et le test directement sur vos ressources Azure.</span><span class="sxs-lookup"><span data-stu-id="654c6-121">All you need is an available git repo, and you can set up to deploy and test directly on your Azure resources.</span></span> <span data-ttu-id="654c6-122">Le pipeline de configuration est facile à créer et à gérer, et étant approvisionné directement sur Azure, vous pouvez le contrôler de la même façon que vos autres ressources Azure.</span><span class="sxs-lookup"><span data-stu-id="654c6-122">The configuration pipeline is easy to create and manage, and since it's provisioned directly on Azure, you can control it in the same way that you handle your other Azure resources.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="654c6-123">Découvrez comment créer un pipeline CI/CD avec Azure DevOps Project</span><span class="sxs-lookup"><span data-stu-id="654c6-123">Learn how to create a CI/CD pipeline with Azure DevOps Project</span></span>](/devops-project/azure-devops-project-go)

## <a name="dependency-management-with-dep"></a><span data-ttu-id="654c6-124">Gestion des dépendances avec dep</span><span class="sxs-lookup"><span data-stu-id="654c6-124">Dependency management with dep</span></span>

<span data-ttu-id="654c6-125">Il existe plusieurs façons de gérer vos dépendances de package et de faire du vendoring avec Go, car il n’existe encore aucune solution officielle.</span><span class="sxs-lookup"><span data-stu-id="654c6-125">There are many ways to manage your package dependencies and do vendoring with Go, since there is no official solution yet.</span></span> <span data-ttu-id="654c6-126">La méthode recommandée pour effectuer cette gestion est d’utiliser le gestionnaire de dépendances `dep`.</span><span class="sxs-lookup"><span data-stu-id="654c6-126">The recommended way to perform this management is with the `dep` dependency manager.</span></span> <span data-ttu-id="654c6-127">Le kit de développement logiciel (SDK) Azure pour Go utilise dep pour son vendoring et est assuré d’obtenir correctement les dépendances pour n’importe quel autre projet à l’aide de dep.</span><span class="sxs-lookup"><span data-stu-id="654c6-127">The Azure SDK for Go uses dep for its vendoring, and is guaranteed to correctly get dependencies for any other project using dep.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="654c6-128">Obtenir le gestionnaire de dépendances dep</span><span class="sxs-lookup"><span data-stu-id="654c6-128">Get the dep dependency manager</span></span>](https://github.com/golang/dep)
