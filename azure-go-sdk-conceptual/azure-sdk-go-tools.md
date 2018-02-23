---
title: "Outils pour les développeurs Go"
description: "Outils permettant de travailler avec le kit de développement logiciel Microsoft Azure SDK pour Go et les services Azure"
keywords: Azure, Go, Golang, Azure, Visual Studio, Visual Studio Code
author: sptramer
ms.author: sttramer
ms.date: 01/30/2018
ms.topic: article
ms.devlang: go
manager: routlaw
ms.openlocfilehash: 4753775e608b39c6da43d64fd08c1532e03d5810
ms.sourcegitcommit: aaa8c37880332625f858a38f5918e6cf581bf48d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/15/2018
---
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a><span data-ttu-id="87516-104">Outils pour les développeurs qui utilisent le kit de développement logiciel Microsoft Azure SDK pour Go</span><span class="sxs-lookup"><span data-stu-id="87516-104">Tools for developers using the Azure SDK for Go</span></span>

<span data-ttu-id="87516-105">Pour plus d’efficacité dans l’écriture du code Go et pour un fonctionnement transparent avec les services Azure, voici certains outils recommandés.</span><span class="sxs-lookup"><span data-stu-id="87516-105">To be effective at writing Go code and have it work seamlessly with Azure services, here are some recommended tools.</span></span>

## <a name="azure-cli-20"></a><span data-ttu-id="87516-106">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="87516-106">Azure CLI 2.0</span></span>

<span data-ttu-id="87516-107">Azure CLI 2.0 fournit une interface de ligne de commande pour créer et configurer des ressources Azure dans vos abonnements.</span><span class="sxs-lookup"><span data-stu-id="87516-107">The Azure 2.0 CLI provides a command-line interface to create and configure Azure resources in your subscriptions.</span></span> <span data-ttu-id="87516-108">L’interface CLI peut vous aider à découvrir rapidement la création de ressources Azure communes et partagées, afin de pouvoir vous concentrer sur l’utilisation de services plus complexe.</span><span class="sxs-lookup"><span data-stu-id="87516-108">The CLI can help you get started building common and shared Azure resources quickly, so that you can focus on more complex usage of services.</span></span> <span data-ttu-id="87516-109">L’interface CLI dispose de fonctionnalités de filtrage et de requête vous permettant de diriger la sortie vers votre outil de ligne de commande préféré.</span><span class="sxs-lookup"><span data-stu-id="87516-109">The CLI has query and filtering features so you can pipe output directly to your favorite command-line tools.</span></span> <span data-ttu-id="87516-110">L’interface CLI est disponible pour l’installation sur votre système local, en tant qu’image Docker ou via [Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview).</span><span class="sxs-lookup"><span data-stu-id="87516-110">The CLI is available for installation on your local system, as a Docker image, or through [Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview).</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="87516-111">Installer Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="87516-111">Install the Azure CLI 2.0</span></span>](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a><span data-ttu-id="87516-112">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="87516-112">Visual Studio Code</span></span>

<span data-ttu-id="87516-113">Visual Studio Code est un éditeur léger offrant une prise en charge complète pour le langage Go via des extensions.</span><span class="sxs-lookup"><span data-stu-id="87516-113">Visual Studio Code is a lightweight editor that has comprehensive support for the Go language through extensions.</span></span> <span data-ttu-id="87516-114">Ces extensions incluent la prise en charge de fonctionnalités telles que la saisie semi-automatique, les modèles `impl`, la refactorisation et le débogage.</span><span class="sxs-lookup"><span data-stu-id="87516-114">These extensions include support for features like autocomplete, `impl` templates, refactoring, and debugging.</span></span> <span data-ttu-id="87516-115">Visual Studio Code offre également de nombreuses extensions pour les outils de développement courants tels que le contrôle de code source et offre même des extensions pour des interactions directes avec les services Azure.</span><span class="sxs-lookup"><span data-stu-id="87516-115">Visual Studio Code also offers many extensions for common developer tools such as source control, and even offers extensions for direct interactions with Azure services.</span></span> <span data-ttu-id="87516-116">Microsoft gère une extension de métadonnées officielle comprenant ces extensions Azure, dont une interface interactive pour l’interface de ligne de commande Azure.</span><span class="sxs-lookup"><span data-stu-id="87516-116">Microsoft maintains an official meta-extension including these Azure extensions, including an interactive interface for the Azure CLI.</span></span>

* [<span data-ttu-id="87516-117">Installation de Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="87516-117">Install Visual Studio Code</span></span>](https://code.visualstudio.com/Download)
* [<span data-ttu-id="87516-118">Obtenir l’extension Go de Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="87516-118">Get the Visual Studio Code Go extension</span></span>](https://code.visualstudio.com/docs/languages/go)
* [<span data-ttu-id="87516-119">Obtenir l’extension Azure Tools</span><span class="sxs-lookup"><span data-stu-id="87516-119">Get the Azure Tools extension</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="dependency-management-with-dep"></a><span data-ttu-id="87516-120">Gestion des dépendances avec dep</span><span class="sxs-lookup"><span data-stu-id="87516-120">Dependency management with dep</span></span>

<span data-ttu-id="87516-121">Il existe plusieurs façons de gérer vos dépendances de package et de faire du vendoring avec Go, car il n’existe encore aucune solution officielle.</span><span class="sxs-lookup"><span data-stu-id="87516-121">There are many ways to manage your package dependencies and do vendoring with Go, since there is no official solution yet.</span></span> <span data-ttu-id="87516-122">La méthode recommandée pour effectuer cette gestion est d’utiliser le gestionnaire de dépendances `dep`.</span><span class="sxs-lookup"><span data-stu-id="87516-122">The recommended way to perform this management is with the `dep` dependency manager.</span></span> <span data-ttu-id="87516-123">Le kit de développement logiciel (SDK) Azure pour Go utilise dep pour son vendoring et est assuré d’obtenir correctement les dépendances pour n’importe quel autre projet à l’aide de dep.</span><span class="sxs-lookup"><span data-stu-id="87516-123">The Azure SDK for Go uses dep for its vendoring, and is guaranteed to correctly get dependencies for any other project using dep.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="87516-124">Obtenir le gestionnaire de dépendances dep</span><span class="sxs-lookup"><span data-stu-id="87516-124">Get the dep dependency manager</span></span>](https://github.com/tools/godep)

## <a name="telemetry-with-application-insights"></a><span data-ttu-id="87516-125">Télémétrie avec Application Insights</span><span class="sxs-lookup"><span data-stu-id="87516-125">Telemetry with Application Insights</span></span>

<span data-ttu-id="87516-126">[Application Insights](https://azure.microsoft.com/en-us/services/application-insights/) est un produit d’analytique qui vous permet de recueillir des informations de télémétrie à partir de vos applications et s’intègre avec l’écosystème d’Azure, Visual Studio Team Services et GitHub.</span><span class="sxs-lookup"><span data-stu-id="87516-126">[Application Insights](https://azure.microsoft.com/en-us/services/application-insights/) is an analytics product that allows you to easily collect telemetry information from your applications and integrates with the Azure ecosystem, Visual Studio Team Services, and GitHub.</span></span> <span data-ttu-id="87516-127">Il peut être utilisé dans de nombreuses applications, et Microsoft propose un kit de développement logiciel (SDK) Go pour l’utilisation avec Application Insights.</span><span class="sxs-lookup"><span data-stu-id="87516-127">It can be used in many applications, and Microsoft offers a Go SDK for working with Application Insights.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="87516-128">Obtenir Application Insights pour le kit de développement logiciel (SDK) Go</span><span class="sxs-lookup"><span data-stu-id="87516-128">Get the Application Insights for Go SDK</span></span>](https://github.com/Microsoft/ApplicationInsights-Go) 
