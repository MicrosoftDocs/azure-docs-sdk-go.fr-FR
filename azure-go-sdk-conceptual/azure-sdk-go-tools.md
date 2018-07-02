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
ms.openlocfilehash: 25b46e3a1636c39e261ba11c6f8939d8721cc693
ms.sourcegitcommit: 79d216c6b0442d0f3b3660ff2a34dc8b2049390c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37093156"
---
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a><span data-ttu-id="8e14f-103">Outils pour les développeurs qui utilisent le kit de développement logiciel Microsoft Azure SDK pour Go</span><span class="sxs-lookup"><span data-stu-id="8e14f-103">Tools for developers using the Azure SDK for Go</span></span>

<span data-ttu-id="8e14f-104">Pour plus d’efficacité dans l’écriture du code Go et pour un fonctionnement transparent avec les services Azure, voici certains outils recommandés.</span><span class="sxs-lookup"><span data-stu-id="8e14f-104">To be effective at writing Go code and have it work seamlessly with Azure services, here are some recommended tools.</span></span>

## <a name="azure-cli-20"></a><span data-ttu-id="8e14f-105">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="8e14f-105">Azure CLI 2.0</span></span>

<span data-ttu-id="8e14f-106">Azure CLI 2.0 fournit une interface de ligne de commande pour créer et configurer des ressources Azure dans vos abonnements.</span><span class="sxs-lookup"><span data-stu-id="8e14f-106">The Azure 2.0 CLI provides a command-line interface to create and configure Azure resources in your subscriptions.</span></span> <span data-ttu-id="8e14f-107">L’interface CLI peut vous aider à découvrir rapidement la création de ressources Azure communes et partagées, afin de pouvoir vous concentrer sur l’utilisation de services plus complexe.</span><span class="sxs-lookup"><span data-stu-id="8e14f-107">The CLI can help you get started building common and shared Azure resources quickly, so that you can focus on more complex usage of services.</span></span> <span data-ttu-id="8e14f-108">L’interface CLI dispose de fonctionnalités de filtrage et de requête vous permettant de diriger la sortie vers votre outil de ligne de commande préféré.</span><span class="sxs-lookup"><span data-stu-id="8e14f-108">The CLI has query and filtering features so you can pipe output directly to your favorite command-line tools.</span></span> <span data-ttu-id="8e14f-109">L’interface CLI est disponible pour l’installation sur votre système local, en tant qu’image Docker ou via [Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview).</span><span class="sxs-lookup"><span data-stu-id="8e14f-109">The CLI is available for installation on your local system, as a Docker image, or through [Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview).</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="8e14f-110">Installer Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="8e14f-110">Install the Azure CLI 2.0</span></span>](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a><span data-ttu-id="8e14f-111">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8e14f-111">Visual Studio Code</span></span>

<span data-ttu-id="8e14f-112">Visual Studio Code est un éditeur léger offrant une prise en charge complète pour le langage Go via des extensions.</span><span class="sxs-lookup"><span data-stu-id="8e14f-112">Visual Studio Code is a lightweight editor that has comprehensive support for the Go language through extensions.</span></span> <span data-ttu-id="8e14f-113">Ces extensions incluent la prise en charge de fonctionnalités telles que la saisie semi-automatique, les modèles `impl`, la refactorisation et le débogage.</span><span class="sxs-lookup"><span data-stu-id="8e14f-113">These extensions include support for features like autocomplete, `impl` templates, refactoring, and debugging.</span></span> <span data-ttu-id="8e14f-114">Visual Studio Code offre également de nombreuses extensions pour les outils de développement courants tels que le contrôle de code source et offre même des extensions pour des interactions directes avec les services Azure.</span><span class="sxs-lookup"><span data-stu-id="8e14f-114">Visual Studio Code also offers many extensions for common developer tools such as source control, and even offers extensions for direct interactions with Azure services.</span></span> <span data-ttu-id="8e14f-115">Microsoft gère une extension de métadonnées officielle comprenant ces extensions Azure, dont une interface interactive pour l’interface de ligne de commande Azure.</span><span class="sxs-lookup"><span data-stu-id="8e14f-115">Microsoft maintains an official meta-extension including these Azure extensions, including an interactive interface for the Azure CLI.</span></span>

* [<span data-ttu-id="8e14f-116">Installation de Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8e14f-116">Install Visual Studio Code</span></span>](https://code.visualstudio.com/Download)
* [<span data-ttu-id="8e14f-117">Obtenir l’extension Go de Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8e14f-117">Get the Visual Studio Code Go extension</span></span>](https://code.visualstudio.com/docs/languages/go)
* [<span data-ttu-id="8e14f-118">Obtenir l’extension Azure Tools</span><span class="sxs-lookup"><span data-stu-id="8e14f-118">Get the Azure Tools extension</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="dependency-management-with-dep"></a><span data-ttu-id="8e14f-119">Gestion des dépendances avec dep</span><span class="sxs-lookup"><span data-stu-id="8e14f-119">Dependency management with dep</span></span>

<span data-ttu-id="8e14f-120">Il existe plusieurs façons de gérer vos dépendances de package et de faire du vendoring avec Go, car il n’existe encore aucune solution officielle.</span><span class="sxs-lookup"><span data-stu-id="8e14f-120">There are many ways to manage your package dependencies and do vendoring with Go, since there is no official solution yet.</span></span> <span data-ttu-id="8e14f-121">La méthode recommandée pour effectuer cette gestion est d’utiliser le gestionnaire de dépendances `dep`.</span><span class="sxs-lookup"><span data-stu-id="8e14f-121">The recommended way to perform this management is with the `dep` dependency manager.</span></span> <span data-ttu-id="8e14f-122">Le kit de développement logiciel (SDK) Azure pour Go utilise dep pour son vendoring et est assuré d’obtenir correctement les dépendances pour n’importe quel autre projet à l’aide de dep.</span><span class="sxs-lookup"><span data-stu-id="8e14f-122">The Azure SDK for Go uses dep for its vendoring, and is guaranteed to correctly get dependencies for any other project using dep.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="8e14f-123">Obtenir le gestionnaire de dépendances dep</span><span class="sxs-lookup"><span data-stu-id="8e14f-123">Get the dep dependency manager</span></span>](https://github.com/golang/dep)
