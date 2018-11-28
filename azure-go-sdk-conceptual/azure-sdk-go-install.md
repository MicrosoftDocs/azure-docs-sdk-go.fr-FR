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
ms.openlocfilehash: 7990ec8bde5622078aa822fc7e66ba5c4384d682
ms.sourcegitcommit: 3d26b464f196f8675c636ae792637d4c882fb92c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/27/2018
ms.locfileid: "52337141"
---
# <a name="install-the-azure-sdk-for-go"></a><span data-ttu-id="10c8d-103">Installer le kit de développement logiciel Microsoft Azure SDK pour Go</span><span class="sxs-lookup"><span data-stu-id="10c8d-103">Install the Azure SDK for Go</span></span>

<span data-ttu-id="10c8d-104">Bienvenue dans le kit de développement logiciel Microsoft Azure SDK pour Go !</span><span class="sxs-lookup"><span data-stu-id="10c8d-104">Welcome to the Azure SDK for Go!</span></span> <span data-ttu-id="10c8d-105">Le kit de développement logiciel (SDK) vous permet de gérer et d’interagir avec les services Azure à partir de vos applications Go.</span><span class="sxs-lookup"><span data-stu-id="10c8d-105">The SDK allows you to manage and interact with Azure services from your Go applications.</span></span>

## <a name="get-the-azure-sdk-for-go"></a><span data-ttu-id="10c8d-106">Obtenir le kit de développement logiciel Microsoft Azure SDK pour Go</span><span class="sxs-lookup"><span data-stu-id="10c8d-106">Get the Azure SDK for Go</span></span>

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

<span data-ttu-id="10c8d-107">Certains services Azure ont leurs propres Kitw de développement logiciel (SDK) et ne sont pas inclus dans le Kit de développement logiciel Azure de base pour le package Go.</span><span class="sxs-lookup"><span data-stu-id="10c8d-107">Some Azure services have their own Go SDK and aren't included in the core Azure SDK for Go package.</span></span> <span data-ttu-id="10c8d-108">Le tableau suivant répertorie les services disposant de leurs propres Kits de développement logiciel (SDK) et noms de package.</span><span class="sxs-lookup"><span data-stu-id="10c8d-108">The following table lists the services with their own SDKs and their package names.</span></span> <span data-ttu-id="10c8d-109">Ces packages sont tous considérés comme étant en préversion.</span><span class="sxs-lookup"><span data-stu-id="10c8d-109">These packages are all considered to be in preview.</span></span>

| <span data-ttu-id="10c8d-110">de diffusion en continu</span><span class="sxs-lookup"><span data-stu-id="10c8d-110">Service</span></span> | <span data-ttu-id="10c8d-111">Package</span><span class="sxs-lookup"><span data-stu-id="10c8d-111">Package</span></span> |
|---------|---------|
| <span data-ttu-id="10c8d-112">Stockage Blob</span><span class="sxs-lookup"><span data-stu-id="10c8d-112">Blob Storage</span></span> | [<span data-ttu-id="10c8d-113">github.com/Azure/azure-storage-blob-go</span><span class="sxs-lookup"><span data-stu-id="10c8d-113">github.com/Azure/azure-storage-blob-go</span></span>](https://github.com/Azure/azure-storage-blob-go) |
| <span data-ttu-id="10c8d-114">Stockage Fichier</span><span class="sxs-lookup"><span data-stu-id="10c8d-114">File Storage</span></span> | [<span data-ttu-id="10c8d-115">github.com/Azure/azure-storage-file-go</span><span class="sxs-lookup"><span data-stu-id="10c8d-115">github.com/Azure/azure-storage-file-go</span></span>](https://github.com/Azure/azure-storage-file-go) |
| <span data-ttu-id="10c8d-116">File d’attente de stockage</span><span class="sxs-lookup"><span data-stu-id="10c8d-116">Storage Queue</span></span> | [<span data-ttu-id="10c8d-117">github.com/Azure/azure-storage-queue-go</span><span class="sxs-lookup"><span data-stu-id="10c8d-117">github.com/Azure/azure-storage-queue-go</span></span>](https://github.com/Azure/azure-storage-queue-go) |
| <span data-ttu-id="10c8d-118">Event Hub</span><span class="sxs-lookup"><span data-stu-id="10c8d-118">Event Hub</span></span> | [<span data-ttu-id="10c8d-119">github.com/Azure/azure-event-hubs-go</span><span class="sxs-lookup"><span data-stu-id="10c8d-119">github.com/Azure/azure-event-hubs-go</span></span>](https://github.com/Azure/azure-event-hubs-go) |
| <span data-ttu-id="10c8d-120">Service Bus</span><span class="sxs-lookup"><span data-stu-id="10c8d-120">Service Bus</span></span> | [<span data-ttu-id="10c8d-121">github.com/Azure/azure-service-bus-go</span><span class="sxs-lookup"><span data-stu-id="10c8d-121">github.com/Azure/azure-service-bus-go</span></span>](https://github.com/Azure/azure-service-bus-go) |
| <span data-ttu-id="10c8d-122">Application Insights</span><span class="sxs-lookup"><span data-stu-id="10c8d-122">Application Insights</span></span> | [<span data-ttu-id="10c8d-123">github.com/Microsoft/ApplicationInsights-go</span><span class="sxs-lookup"><span data-stu-id="10c8d-123">github.com/Microsoft/ApplicationInsights-go</span></span>](https://github.com/Microsoft/ApplicationInsights-go) |

## <a name="vendor-the-azure-sdk-for-go"></a><span data-ttu-id="10c8d-124">Fournir le kit de développement logiciel (SDK) Azure pour Go</span><span class="sxs-lookup"><span data-stu-id="10c8d-124">Vendor the Azure SDK for Go</span></span>

<span data-ttu-id="10c8d-125">Le kit de développement logiciel Microsoft Azure SDK pour Go peut être fourni via [dep](https://github.com/golang/dep).</span><span class="sxs-lookup"><span data-stu-id="10c8d-125">The Azure SDK for Go may be vendored through [dep](https://github.com/golang/dep).</span></span> <span data-ttu-id="10c8d-126">Pour des raisons de stabilité, le vendoring est recommandé.</span><span class="sxs-lookup"><span data-stu-id="10c8d-126">For stability reasons, vendoring is recommended.</span></span> <span data-ttu-id="10c8d-127">Pour utiliser `dep` dans votre projet, ajoutez `github.com/Azure/azure-sdk-for-go` à une section `[[constraint]]` de votre élément `Gopkg.toml`.</span><span class="sxs-lookup"><span data-stu-id="10c8d-127">To use `dep` in your own project, add `github.com/Azure/azure-sdk-for-go` to a `[[constraint]]` section of your `Gopkg.toml`.</span></span> <span data-ttu-id="10c8d-128">Par exemple, pour fournir la version `14.0.0`, ajoutez l’entrée suivante :</span><span class="sxs-lookup"><span data-stu-id="10c8d-128">For example, to vendor on version `14.0.0`, add the following entry:</span></span>

```toml
[[constraint]]
name = "github.com/Azure/azure-sdk-for-go"
version = "14.0.0"
```

## <a name="include-the-azure-sdk-for-go-in-your-project"></a><span data-ttu-id="10c8d-129">Inclure le kit de développement logiciel (SDK) Azure pour Go dans votre projet</span><span class="sxs-lookup"><span data-stu-id="10c8d-129">Include the Azure SDK for Go in your project</span></span>

<span data-ttu-id="10c8d-130">Pour utiliser les Services Azure à partir de votre code Go, importer tous les services avec lesquels vous interagissez et les modules `autorest` requis.</span><span class="sxs-lookup"><span data-stu-id="10c8d-130">To use Azure services from your Go code, import any services you interact with and the required `autorest` modules.</span></span>
<span data-ttu-id="10c8d-131">Vous obtenez une liste complète des modules disponibles à partir de GoDoc pour les [services disponibles](https://godoc.org/github.com/Azure/azure-sdk-for-go) et les [packages AutoRest](https://godoc.org/github.com/Azure/go-autorest).</span><span class="sxs-lookup"><span data-stu-id="10c8d-131">You get a complete list of the available modules from GoDoc for [available services](https://godoc.org/github.com/Azure/azure-sdk-for-go) and [AutoRest packages](https://godoc.org/github.com/Azure/go-autorest).</span></span> <span data-ttu-id="10c8d-132">Les packages courants que vous devez obtenir de `go-autorest` sont :</span><span class="sxs-lookup"><span data-stu-id="10c8d-132">The most common packages you need from `go-autorest` are:</span></span>

| <span data-ttu-id="10c8d-133">Package</span><span class="sxs-lookup"><span data-stu-id="10c8d-133">Package</span></span> | <span data-ttu-id="10c8d-134">Description</span><span class="sxs-lookup"><span data-stu-id="10c8d-134">Description</span></span> |
|---------|-------------|
| <span data-ttu-id="10c8d-135">[github.com/Azure/go-autorest/autorest][autorest]</span><span class="sxs-lookup"><span data-stu-id="10c8d-135">[github.com/Azure/go-autorest/autorest][autorest]</span></span> | <span data-ttu-id="10c8d-136">Objets pour la gestion de l’authentification client du service</span><span class="sxs-lookup"><span data-stu-id="10c8d-136">Objects for handling service client authentication</span></span> |
| <span data-ttu-id="10c8d-137">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span><span class="sxs-lookup"><span data-stu-id="10c8d-137">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span></span> | <span data-ttu-id="10c8d-138">Constantes pour les interactions avec les Services Azure</span><span class="sxs-lookup"><span data-stu-id="10c8d-138">Constants for interactions with Azure services</span></span> |
| <span data-ttu-id="10c8d-139">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span><span class="sxs-lookup"><span data-stu-id="10c8d-139">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span></span> | <span data-ttu-id="10c8d-140">Mécanismes d’authentification pour l’accès aux Services Azure</span><span class="sxs-lookup"><span data-stu-id="10c8d-140">Authentication mechanisms for accessing Azure services</span></span> |
| <span data-ttu-id="10c8d-141">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span><span class="sxs-lookup"><span data-stu-id="10c8d-141">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span></span> | <span data-ttu-id="10c8d-142">Programmes d’assistance d’assertion de type pour l’utilisation des structures de données du kit de développement logiciel (SDK) Azure</span><span class="sxs-lookup"><span data-stu-id="10c8d-142">Type assertion helpers for working with Azure SDK data structures</span></span> |

[autorest]: https://godoc.org/github.com/Azure/go-autorest/autorest
[autorest/azure]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure
[autorest/adal]: https://godoc.org/github.com/Azure/go-autorest/autorest/adal
[autorest/to]: https://godoc.org/github.com/Azure/go-autorest/autorest/to

<span data-ttu-id="10c8d-143">Les versions des packages Go et des services Azure sont gérées de façon indépendante.</span><span class="sxs-lookup"><span data-stu-id="10c8d-143">Go packages and Azure services are versioned independently.</span></span> <span data-ttu-id="10c8d-144">Les versions du service font partie du chemin d’accès d’importation du module, en dessous du module `services`.</span><span class="sxs-lookup"><span data-stu-id="10c8d-144">The service versions are part of the module import path, underneath the `services` module.</span></span> <span data-ttu-id="10c8d-145">Le chemin d’accès complet du module est constitué du nom du service, suivi par la version au format `YYYY-MM-DD`, suivi à nouveau du nom du service.</span><span class="sxs-lookup"><span data-stu-id="10c8d-145">The full path for the module is the name of the service, followed by the version in `YYYY-MM-DD` format, followed by the service name again.</span></span> <span data-ttu-id="10c8d-146">Par exemple, pour importer la version `2017-03-30` du service Compute :</span><span class="sxs-lookup"><span data-stu-id="10c8d-146">For example, to import the `2017-03-30` version of the Compute service:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/services/compute/mgmt/2017-03-30/compute"
```

<span data-ttu-id="10c8d-147">Il est recommandé d’utiliser la dernière version d’un service au début du développement et d’assurer en permanence sa cohérence.</span><span class="sxs-lookup"><span data-stu-id="10c8d-147">It's recommended that you use the latest version of a service when starting development and keep it consistent.</span></span>
<span data-ttu-id="10c8d-148">Les exigences relatives au service peuvent varier entre des versions qui peuvent interrompre votre code, même si aucune mise à jour du Kit de développement logiciel (SDK) Go n’est disponible pendant cette période.</span><span class="sxs-lookup"><span data-stu-id="10c8d-148">Service requirements may change between versions that could break your code, even if there are no Go SDK updates during that time.</span></span>

<span data-ttu-id="10c8d-149">Si vous avez besoin d’un instantané collectif des services, vous pouvez également sélectionner une version de profil unique.</span><span class="sxs-lookup"><span data-stu-id="10c8d-149">If you need a collective snapshot of services, you can also select a single profile version.</span></span> <span data-ttu-id="10c8d-150">Actuellement, le seul profil verrouillé est la version `2017-03-09`, qui ne dispose peut-être pas des fonctionnalités les plus récentes des services.</span><span class="sxs-lookup"><span data-stu-id="10c8d-150">Right now, the only locked profile is version `2017-03-09`, which may not have the latest features of services.</span></span> <span data-ttu-id="10c8d-151">Les profils sont situés sous le module `profiles`, avec leur version au format `YYYY-MM-DD`.</span><span class="sxs-lookup"><span data-stu-id="10c8d-151">Profiles are located under the `profiles` module, with their version in the `YYYY-MM-DD` format.</span></span> <span data-ttu-id="10c8d-152">Les services sont regroupées sous leur version de profil.</span><span class="sxs-lookup"><span data-stu-id="10c8d-152">Services are grouped under their profile version.</span></span> <span data-ttu-id="10c8d-153">Par exemple, pour importer le module de Gestion des ressources Azure à partir du profil `2017-03-09` :</span><span class="sxs-lookup"><span data-stu-id="10c8d-153">For example, to import the Azure Resources management module from the `2017-03-09` profile:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/resources/mgmt/resources"
```

> [!WARNING]
> <span data-ttu-id="10c8d-154">Les profils `preview` et `latest` sont également disponibles.</span><span class="sxs-lookup"><span data-stu-id="10c8d-154">There are also `preview` and `latest` profiles available.</span></span> <span data-ttu-id="10c8d-155">Leur utilisation n’est pas recommandée.</span><span class="sxs-lookup"><span data-stu-id="10c8d-155">Using them is not recommended.</span></span> <span data-ttu-id="10c8d-156">Ces profils sont des versions continues et le comportement du service peut changer à tout moment.</span><span class="sxs-lookup"><span data-stu-id="10c8d-156">These profiles are rolling versions and service behavior may change at any time.</span></span>

## <a name="next-steps"></a><span data-ttu-id="10c8d-157">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="10c8d-157">Next steps</span></span>

<span data-ttu-id="10c8d-158">Pour commencer à utiliser le kit de développement logiciel Microsoft Azure SDK pour Go, essayez un démarrage rapide.</span><span class="sxs-lookup"><span data-stu-id="10c8d-158">To begin using the Azure SDK for Go, try out a quickstart.</span></span>

* [<span data-ttu-id="10c8d-159">Déploiement d’une machine virtuelle à partir d’un modèle</span><span class="sxs-lookup"><span data-stu-id="10c8d-159">Deploy a virtual machine from a template</span></span>](azure-sdk-go-qs-vm.md)
* [<span data-ttu-id="10c8d-160">Transférer des objets vers le Stockage Blob Azure à l’aide du kit de développement logiciel (SDK) Blob Azure pour Go</span><span class="sxs-lookup"><span data-stu-id="10c8d-160">Transfer objects to Azure Blob Storage with the Azure Blob SDK for Go</span></span>](/azure/storage/blobs/storage-quickstart-blobs-go?toc=%2fgo%2fazure%2ftoc.json)
* [<span data-ttu-id="10c8d-161">Se connecter à Azure Database pour PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="10c8d-161">Connect to Azure Database for PostgreSQL</span></span>](/azure/postgresql/connect-go?toc=%2fgo%2fazure%2ftoc.json)

<span data-ttu-id="10c8d-162">Si vous souhaitez débuter immédiatement avec d’autres services dans le kit de développement logiciel (SDK) Go, examinez les exemples de code disponibles.</span><span class="sxs-lookup"><span data-stu-id="10c8d-162">If you want to get started with other services in the Go SDK immediately, take a look at some of the available sample code.</span></span>

* [<span data-ttu-id="10c8d-163">S’authentifier avec les Services Azure</span><span class="sxs-lookup"><span data-stu-id="10c8d-163">Authenticate with Azure services</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/internal/iam)
* [<span data-ttu-id="10c8d-164">Déployer de nouvelles machines virtuelles avec l’authentification SSH</span><span class="sxs-lookup"><span data-stu-id="10c8d-164">Deploy new virtual machines with SSH authentication</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute)
* [<span data-ttu-id="10c8d-165">Déployer une image de conteneur sur Azure Container Instances</span><span class="sxs-lookup"><span data-stu-id="10c8d-165">Deploy a container image to Azure Container Instances</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerinstance)
* [<span data-ttu-id="10c8d-166">Créer un cluster dans le service Azure Kubernetes</span><span class="sxs-lookup"><span data-stu-id="10c8d-166">Create a cluster in Azure Kubernetes Service</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerservice)
* [<span data-ttu-id="10c8d-167">Utiliser des services de Stockage Azure</span><span class="sxs-lookup"><span data-stu-id="10c8d-167">Work with Azure Storage services</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/storage)
* [<span data-ttu-id="10c8d-168">Tous les exemples pour le kit de développement logiciel Microsoft Azure SDK pour Go</span><span class="sxs-lookup"><span data-stu-id="10c8d-168">All samples for the Azure SDK for Go</span></span>](https://github.com/azure-samples/azure-sdk-for-go-samples)
