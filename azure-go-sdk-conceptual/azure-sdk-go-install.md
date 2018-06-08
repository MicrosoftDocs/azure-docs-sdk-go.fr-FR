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
ms.openlocfilehash: 8423b3fedd89e57662bf96f777a5b30926914da9
ms.sourcegitcommit: b81b17cbb934399c195bfdcb87137aee935f5234
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34755513"
---
# <a name="install-the-azure-sdk-for-go"></a>Installer le kit de développement logiciel Microsoft Azure SDK pour Go

Bienvenue dans le kit de développement logiciel Microsoft Azure SDK pour Go ! Le kit de développement logiciel (SDK) vous permet de gérer et d’interagir avec les services Azure à partir de vos applications Go.

## <a name="get-the-azure-sdk-for-go"></a>Obtenir le kit de développement logiciel Microsoft Azure SDK pour Go

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

Certains services Azure ont leurs propres Kitw de développement logiciel (SDK) et ne sont pas inclus dans le Kit de développement logiciel Azure de base pour le package Go. Le tableau suivant répertorie les services disposant de leurs propres Kits de développement logiciel (SDK) et noms de package. Ces packages sont tous considérés comme étant en préversion.

| de diffusion en continu | Package |
|---------|---------|
| Stockage Blob | [github.com/Azure/azure-storage-blob-go](https://github.com/Azure/azure-storage-blob-go) |
| Stockage Fichier | [github.com/Azure/azure-storage-file-go](https://github.com/Azure/azure-storage-file-go) |
| Event Hub | [github.com/Azure/azure-event-hubs-go](https://github.com/Azure/azure-event-hubs-go) |
| Application Insights | [github.com/Microsoft/ApplicationInsights-go](https://github.com/Microsoft/ApplicationInsights-go) |

## <a name="vendor-the-azure-sdk-for-go"></a>Fournir le kit de développement logiciel (SDK) Azure pour Go

Le kit de développement logiciel Microsoft Azure SDK pour Go peut être fourni via [dep](https://github.com/golang/dep). Pour des raisons de stabilité, le vendoring est recommandé. Pour pouvoir utiliser la prise en charge `dep`, ajoutez `github.com/Azure/azure-sdk-for-go` à une section `[[constraint]]` de votre `Gopkg.toml`. Par exemple, pour fournir la version `14.0.0`, ajoutez l’entrée suivante :

```
[[constraint]]
name = "github.com/Azure/azure-sdk-for-go"
version = "14.0.0"
```

## <a name="include-the-azure-sdk-for-go-in-your-project"></a>Inclure le kit de développement logiciel (SDK) Azure pour Go dans votre projet

Pour utiliser les Services Azure à partir de votre code Go, importer tous les services avec lesquels vous interagissez et les modules `autorest` requis.
Vous obtenez une liste complète des modules disponibles à partir de GoDoc pour les [services disponibles](https://godoc.org/github.com/Azure/azure-sdk-for-go) et les [packages AutoRest](https://godoc.org/github.com/Azure/go-autorest). Les packages courants que vous devez obtenir de `go-autorest` sont :

| Package | Description |
|---------|-------------|
| [github.com/Azure/go-autorest/autorest][autorest] | Objets pour la gestion de l’authentification client du service |
| [github.com/Azure/go-autorest/autorest/azure][autorest/azure] | Constantes pour les interactions avec les Services Azure |
| [github.com/Azure/go-autorest/autorest/adal][autorest/adal] | Mécanismes d’authentification pour l’accès aux Services Azure |
| [github.com/Azure/go-autorest/autorest/to][autorest/to] | Programmes d’assistance d’assertion de type pour l’utilisation des structures de données du kit de développement logiciel (SDK) Azure |

[autorest]: https://godoc.org/github.com/Azure/go-autorest/autorest
[autorest/azure]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure
[autorest/adal]: https://godoc.org/github.com/Azure/go-autorest/autorest/adal
[autorest/to]: https://godoc.org/github.com/Azure/go-autorest/autorest/to

Les modules pour les Services Azure sont gérés indépendamment des API du kit de développement logiciel (SDK) pour ces derniers. Ces versions font partie du chemin d’accès d’importation du module et proviennent d’une _version du service_ ou d’un _profil_. Actuellement, il est recommandé d’utiliser une version spécifique du service pour le développement et la mise en production. Les services se trouvent sous le module `services`. Le chemin d’accès complet pour l’importation est le nom du service, suivi par la version au format `YYYY-MM-DD`, suivi du nom de service à nouveau. Par exemple, pour inclure la version `2017-03-30` du service de calcul :

```go
import "github.com/Azure/azure-sdk-for-go/services/compute/mgmt/2017-03-30/compute"
```

Actuellement, il est recommandé d’utiliser la version la plus récente d’un service, à moins que vous n’ayez une raison de faire autrement.

Si vous avez besoin d’un instantané collectif des services, vous pouvez également sélectionner une version de profil unique. Actuellement, le seul profil verrouillé est la version `2017-03-09`, qui ne dispose peut-être pas des fonctionnalités les plus récentes des services. Les profils sont situés sous le module `profiles`, avec leur version au format `YYYY-MM-DD`. Les services sont regroupés sous leur version de profil. Par exemple, pour importer le module de Gestion des ressources Azure à partir du profil `2017-03-09` :

```go
import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/resources/mgmt/resources"
```

> [!WARNING]
> Les profils `preview` et `latest` sont également disponibles. Leur utilisation n’est pas recommandée. Ces profils sont des versions continues et le comportement du service peut changer à tout moment.

## <a name="next-steps"></a>Étapes suivantes

Pour commencer à utiliser le kit de développement logiciel Microsoft Azure SDK pour Go, essayez un démarrage rapide.

* [Déploiement d’une machine virtuelle à partir d’un modèle](azure-sdk-go-qs-vm.md)
* [Transférer des objets vers le Stockage Blob Azure à l’aide du kit de développement logiciel (SDK) Blob Azure pour Go](/azure/storage/blobs/storage-quickstart-blobs-go?toc=%2fgo%2fazure%2ftoc.json)
* [Se connecter à Azure Database pour PostgreSQL](/azure/postgresql/connect-go?toc=%2fgo%2fazure%2ftoc.json)

Si vous souhaitez débuter immédiatement avec d’autres services dans le kit de développement logiciel (SDK) Go, examinez les exemples de code disponibles.

* [S’authentifier avec les Services Azure](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/iam)
* [Déployer de nouvelles machines virtuelles avec l’authentification SSH](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute)
* [Déployer une image de conteneur sur Azure Container Instances](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerinstance)
* [Créer un cluster dans le service Azure Kubernetes](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerservice)
* [Utiliser des services de Stockage Azure](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/storage)
* [Tous les exemples pour le kit de développement logiciel Microsoft Azure SDK pour Go](https://github.com/azure-samples/azure-sdk-for-go-samples)
