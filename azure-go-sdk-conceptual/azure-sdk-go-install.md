---
title: Installer le kit de développement logiciel Microsoft Azure SDK pour Go
description: Installation, fournisseur et configuration du kit de développement logiciel (SDK) Azure pour Go.
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 03/14/2018
ms.topic: conceptual
ms.prod: azure
ms.technology: azure-sdk-go
ms.devlang: go
ms.openlocfilehash: ad77bdff881770512a828b19dc7af4821f4a55ad
ms.sourcegitcommit: f08abf902b48f8173aa6e261084ff2cfc9043305
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="install-the-azure-sdk-for-go"></a><span data-ttu-id="12eb2-103">Installer le kit de développement logiciel Microsoft Azure SDK pour Go</span><span class="sxs-lookup"><span data-stu-id="12eb2-103">Install the Azure SDK for Go</span></span>

<span data-ttu-id="12eb2-104">Bienvenue dans le kit de développement logiciel Microsoft Azure SDK pour Go !</span><span class="sxs-lookup"><span data-stu-id="12eb2-104">Welcome to the Azure SDK for Go!</span></span> <span data-ttu-id="12eb2-105">Le kit de développement logiciel (SDK) vous permet de gérer et d’interagir avec les services Azure à partir de vos applications Go.</span><span class="sxs-lookup"><span data-stu-id="12eb2-105">The SDK allows you to manage and interact with Azure services from your Go applications.</span></span>

## <a name="get-the-azure-sdk-for-go"></a><span data-ttu-id="12eb2-106">Obtenir le kit de développement logiciel Microsoft Azure SDK pour Go</span><span class="sxs-lookup"><span data-stu-id="12eb2-106">Get the Azure SDK for Go</span></span>

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

<span data-ttu-id="12eb2-107">L’utilisation des objets blobs de Stockage Azure nécessite un kit de développement logiciel distinct.</span><span class="sxs-lookup"><span data-stu-id="12eb2-107">Working with Azure Storage Blobs requires a separate SDK.</span></span>

```bash
go get -u -d github.com/Azure/azure-storage-blob-go/...
```

## <a name="vendor-the-azure-sdk-for-go"></a><span data-ttu-id="12eb2-108">Fournir le kit de développement logiciel (SDK) Azure pour Go</span><span class="sxs-lookup"><span data-stu-id="12eb2-108">Vendor the Azure SDK for Go</span></span>

<span data-ttu-id="12eb2-109">Le kit de développement logiciel Microsoft Azure SDK pour Go peut être fourni via [dep](https://github.com/golang/dep).</span><span class="sxs-lookup"><span data-stu-id="12eb2-109">The Azure SDK for Go may be vendored through [dep](https://github.com/golang/dep).</span></span> <span data-ttu-id="12eb2-110">Pour des raisons de stabilité, le vendoring est recommandé.</span><span class="sxs-lookup"><span data-stu-id="12eb2-110">For stability reasons, vendoring is recommended.</span></span> <span data-ttu-id="12eb2-111">Pour pouvoir utiliser la prise en charge `dep`, ajoutez `github.com/Azure/azure-sdk-for-go` à une section `[[constraint]]` de votre `Gopkg.toml`.</span><span class="sxs-lookup"><span data-stu-id="12eb2-111">In order to use `dep` support, add `github.com/Azure/azure-sdk-for-go` to a `[[constraint]]` section of your `Gopkg.toml`.</span></span> <span data-ttu-id="12eb2-112">Par exemple, pour fournir la version `14.0.0`, ajoutez l’entrée suivante :</span><span class="sxs-lookup"><span data-stu-id="12eb2-112">For example, to vendor on version `14.0.0`, add the following entry:</span></span>

```
[[constraint]]
name = "github.com/Azure/azure-sdk-for-go"
version = "14.0.0"
```

## <a name="include-the-azure-sdk-for-go-in-your-project"></a><span data-ttu-id="12eb2-113">Inclure le kit de développement logiciel (SDK) Azure pour Go dans votre projet</span><span class="sxs-lookup"><span data-stu-id="12eb2-113">Include the Azure SDK for Go in your project</span></span>

<span data-ttu-id="12eb2-114">Pour utiliser les Services Azure à partir de votre code Go, importer tous les services avec lesquels vous interagissez et les modules `autorest` requis.</span><span class="sxs-lookup"><span data-stu-id="12eb2-114">To use Azure services from your Go code, import any services you interact with and the required `autorest` modules.</span></span>
<span data-ttu-id="12eb2-115">Vous obtenez une liste complète des modules disponibles à partir de GoDoc pour les [services disponibles](https://godoc.org/github.com/Azure/azure-sdk-for-go) et les [packages AutoRest](https://godoc.org/github.com/Azure/go-autorest).</span><span class="sxs-lookup"><span data-stu-id="12eb2-115">You get a complete list of the available modules from GoDoc for [available services](https://godoc.org/github.com/Azure/azure-sdk-for-go) and [AutoRest packages](https://godoc.org/github.com/Azure/go-autorest).</span></span> <span data-ttu-id="12eb2-116">Les packages courants que vous devez obtenir de `go-autorest` sont :</span><span class="sxs-lookup"><span data-stu-id="12eb2-116">The most common packages you need from `go-autorest` are:</span></span>

| <span data-ttu-id="12eb2-117">Package</span><span class="sxs-lookup"><span data-stu-id="12eb2-117">Package</span></span> | <span data-ttu-id="12eb2-118">Description</span><span class="sxs-lookup"><span data-stu-id="12eb2-118">Description</span></span> |
|---------|-------------|
| <span data-ttu-id="12eb2-119">[github.com/Azure/go-autorest/autorest][autorest]</span><span class="sxs-lookup"><span data-stu-id="12eb2-119">[github.com/Azure/go-autorest/autorest][autorest]</span></span> | <span data-ttu-id="12eb2-120">Objets pour la gestion de l’authentification client du service</span><span class="sxs-lookup"><span data-stu-id="12eb2-120">Objects for handling service client authentication</span></span> |
| <span data-ttu-id="12eb2-121">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span><span class="sxs-lookup"><span data-stu-id="12eb2-121">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span></span> | <span data-ttu-id="12eb2-122">Constantes pour les interactions avec les Services Azure</span><span class="sxs-lookup"><span data-stu-id="12eb2-122">Constants for interactions with Azure services</span></span> |
| <span data-ttu-id="12eb2-123">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span><span class="sxs-lookup"><span data-stu-id="12eb2-123">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span></span> | <span data-ttu-id="12eb2-124">Mécanismes d’authentification pour l’accès aux Services Azure</span><span class="sxs-lookup"><span data-stu-id="12eb2-124">Authentication mechanisms for accessing Azure services</span></span> |
| <span data-ttu-id="12eb2-125">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span><span class="sxs-lookup"><span data-stu-id="12eb2-125">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span></span> | <span data-ttu-id="12eb2-126">Programmes d’assistance d’assertion de type pour l’utilisation des structures de données du kit de développement logiciel (SDK) Azure</span><span class="sxs-lookup"><span data-stu-id="12eb2-126">Type assertion helpers for working with Azure SDK data structures</span></span> |

[autorest]: https://godoc.org/github.com/Azure/go-autorest/autorest
[autorest/azure]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure
[autorest/adal]: https://godoc.org/github.com/Azure/go-autorest/autorest/adal
[autorest/to]: https://godoc.org/github.com/Azure/go-autorest/autorest/to

<span data-ttu-id="12eb2-127">Les modules pour les Services Azure sont gérés indépendamment des API du kit de développement logiciel (SDK) pour ces derniers.</span><span class="sxs-lookup"><span data-stu-id="12eb2-127">Modules for Azure services are versioned independently from the SDK APIs for them.</span></span> <span data-ttu-id="12eb2-128">Ces versions font partie du chemin d’accès d’importation du module et proviennent d’une _version du service_ ou d’un _profil_.</span><span class="sxs-lookup"><span data-stu-id="12eb2-128">These versions are part of the module import path, and are from either a _service version_ or a _profile_.</span></span> <span data-ttu-id="12eb2-129">Actuellement, il est recommandé d’utiliser une version spécifique du service pour le développement et la mise en production.</span><span class="sxs-lookup"><span data-stu-id="12eb2-129">Currently, it is recommended that you use a specific service version for both development and release.</span></span> <span data-ttu-id="12eb2-130">Les services se trouvent sous le module `services`.</span><span class="sxs-lookup"><span data-stu-id="12eb2-130">Services are located under the `services` module.</span></span> <span data-ttu-id="12eb2-131">Le chemin d’accès complet pour l’importation est le nom du service, suivi par la version au format `YYYY-MM-DD`, suivi du nom de service à nouveau.</span><span class="sxs-lookup"><span data-stu-id="12eb2-131">The full path for the import is the name of the service, followed by the version in `YYYY-MM-DD` format, followed by the service name again.</span></span> <span data-ttu-id="12eb2-132">Par exemple, pour inclure la version `2017-03-30` du service de calcul :</span><span class="sxs-lookup"><span data-stu-id="12eb2-132">For example, to include the `2017-03-30` version of the Compute service:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/services/compute/mgmt/2017-03-30/compute"
```

<span data-ttu-id="12eb2-133">Actuellement, il est recommandé d’utiliser la version la plus récente d’un service, à moins que vous n’ayez une raison de faire autrement.</span><span class="sxs-lookup"><span data-stu-id="12eb2-133">Right now it is recommended that you use the latest version of a service, unless you have a reason to do otherwise.</span></span>

<span data-ttu-id="12eb2-134">Si vous avez besoin d’un instantané collectif des services, vous pouvez également sélectionner une version de profil unique.</span><span class="sxs-lookup"><span data-stu-id="12eb2-134">If you need a collective snapshot of services, you can also select a single profile version.</span></span> <span data-ttu-id="12eb2-135">Actuellement, le seul profil verrouillé est la version `2017-03-09`, qui ne dispose peut-être pas des fonctionnalités les plus récentes des services.</span><span class="sxs-lookup"><span data-stu-id="12eb2-135">Right now, the only locked profile is version `2017-03-09`, which may not have the latest features of services.</span></span> <span data-ttu-id="12eb2-136">Les profils sont situés sous le module `profiles`, avec leur version au format `YYYY-MM-DD`.</span><span class="sxs-lookup"><span data-stu-id="12eb2-136">Profiles are located under the `profiles` module, with their version in the `YYYY-MM-DD` format.</span></span> <span data-ttu-id="12eb2-137">Les services sont regroupées sous leur version de profil.</span><span class="sxs-lookup"><span data-stu-id="12eb2-137">Services are grouped under their profile version.</span></span> <span data-ttu-id="12eb2-138">Par exemple, pour importer le module de Gestion des ressources Azure à partir du profil `2017-03-09` :</span><span class="sxs-lookup"><span data-stu-id="12eb2-138">For example, to import the Azure Resources management module from the `2017-03-09` profile:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/resources/mgmt/resources"
```

> [!WARNING]
> <span data-ttu-id="12eb2-139">Les profils `preview` et `latest` sont également disponibles.</span><span class="sxs-lookup"><span data-stu-id="12eb2-139">There are also `preview` and `latest` profiles available.</span></span> <span data-ttu-id="12eb2-140">Leur utilisation n’est pas recommandée.</span><span class="sxs-lookup"><span data-stu-id="12eb2-140">Using them is not recommended.</span></span> <span data-ttu-id="12eb2-141">Ces profils sont des versions continues et le comportement du service peut changer à tout moment.</span><span class="sxs-lookup"><span data-stu-id="12eb2-141">These profiles are rolling versions and service behavior may change at any time.</span></span>

## <a name="next-steps"></a><span data-ttu-id="12eb2-142">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="12eb2-142">Next steps</span></span>

<span data-ttu-id="12eb2-143">Pour commencer à utiliser le kit de développement logiciel Microsoft Azure SDK pour Go, essayez un démarrage rapide.</span><span class="sxs-lookup"><span data-stu-id="12eb2-143">To begin using the Azure SDK for Go, try out a quickstart.</span></span>

* [<span data-ttu-id="12eb2-144">Déploiement d’une machine virtuelle à partir d’un modèle</span><span class="sxs-lookup"><span data-stu-id="12eb2-144">Deploy a virtual machine from a template</span></span>](azure-sdk-go-qs-vm.md)
* [<span data-ttu-id="12eb2-145">Transférer des objets vers le Stockage Blob Azure à l’aide du kit de développement logiciel (SDK) Blob Azure pour Go</span><span class="sxs-lookup"><span data-stu-id="12eb2-145">Transfer objects to Azure Blob Storage with the Azure Blob SDK for Go</span></span>](/azure/storage/blobs/storage-quickstart-blobs-go?toc=%2fgo%2fazure%2ftoc.json)
* [<span data-ttu-id="12eb2-146">Se connecter à Azure Database pour PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="12eb2-146">Connect to Azure Database for PostgreSQL</span></span>](/azure/postgresql/connect-go?toc=%2fgo%2fazure%2ftoc.json)

<span data-ttu-id="12eb2-147">Si vous souhaitez débuter immédiatement avec d’autres services dans le kit de développement logiciel (SDK) Go, examinez les exemples de code disponibles.</span><span class="sxs-lookup"><span data-stu-id="12eb2-147">If you want to get started with other services in the Go SDK immediately, take a look at some of the available sample code.</span></span>

* [<span data-ttu-id="12eb2-148">S’authentifier avec les Services Azure</span><span class="sxs-lookup"><span data-stu-id="12eb2-148">Authenticate with Azure services</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/iam)
* [<span data-ttu-id="12eb2-149">Déployer de nouvelles machines virtuelles avec l’authentification SSH</span><span class="sxs-lookup"><span data-stu-id="12eb2-149">Deploy new virtual machines with SSH authentication</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute)
* [<span data-ttu-id="12eb2-150">Déployer une image de conteneur sur Azure Container Instances</span><span class="sxs-lookup"><span data-stu-id="12eb2-150">Deploy a container image to Azure Container Instances</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerinstance)
* [<span data-ttu-id="12eb2-151">Créer un cluster dans le service Azure Kubernetes</span><span class="sxs-lookup"><span data-stu-id="12eb2-151">Create a cluster in Azure Kubernetes Service</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerservice)
* [<span data-ttu-id="12eb2-152">Utiliser des services de Stockage Azure</span><span class="sxs-lookup"><span data-stu-id="12eb2-152">Work with Azure Storage services</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/storage)
* [<span data-ttu-id="12eb2-153">Tous les exemples pour le kit de développement logiciel Microsoft Azure SDK pour Go</span><span class="sxs-lookup"><span data-stu-id="12eb2-153">All samples for the Azure SDK for Go</span></span>](https://github.com/azure-samples/azure-sdk-for-go-samples)
