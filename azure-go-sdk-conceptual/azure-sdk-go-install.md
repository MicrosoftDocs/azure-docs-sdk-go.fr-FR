---
title: "Installation du kit de développement logiciel Microsoft Azure SDK pour Go"
description: "Installation, fournisseur et configuration du kit de développement logiciel (SDK) Azure pour Go."
keywords: "Azure, SDK, Go, Golang, Kit de développement logiciel (SDK) Azure"
author: sptramer
ms.author: sttramer
ms.date: 01/30/2018
ms.topic: article
ms.devlang: go
manager: routlaw
ms.openlocfilehash: f822a9304a4744e0b0e93286303aa8bb80fec852
ms.sourcegitcommit: aaa8c37880332625f858a38f5918e6cf581bf48d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/15/2018
---
# <a name="installing-the-azure-sdk-for-go"></a>Installation du kit de développement logiciel Microsoft Azure SDK pour Go

Bienvenue dans le kit de développement logiciel Microsoft Azure SDK pour Go ! Ce kit de développement logiciel (SDK) vous permet de gérer et d’interagir avec les services Azure à partir de vos applications Go.

## <a name="get-the-azure-sdk-for-go"></a>Obtenir le kit de développement logiciel Microsoft Azure SDK pour Go

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

L’utilisation des objets blobs de Stockage Azure nécessite un kit de développement logiciel distinct.

```bash
go get -u -d github.com/Azure/azure-storage-blob-go/...
```

## <a name="vendoring-the-azure-sdk-for-go"></a>Fournir le kit de développement logiciel Microsoft Azure SDK pour Go

Le kit de développement logiciel Microsoft Azure SDK pour Go peut être fourni via [dep](https://github.com/golang/dep). Pour des raisons de stabilité, le vendoring est recommandé. Pour pouvoir utiliser la prise en charge `dep`, ajoutez `gitub.com/Azure/azure-sdk-for-go` à une section `[[constraint]]` de votre `Gopkg.toml`. Par exemple, pour fournir la version `14.0.0`, ajoutez l’entrée suivante :

```
[[constraint]]
name = "github.com/Azure/azure-sdk-for-go"
version = "14.0.0"
```

## <a name="including-the-azure-sdk-for-go-in-your-project"></a>Inclure le kit de développement logiciel Microsoft Azure SDK pour Go dans votre projet

Pour utiliser les Services Azure à partir de votre code Go, importer tous les services avec lesquels vous interagissez et les modules `autorest` requis.
Vous obtenez une liste complète des modules disponibles à partir de GoDoc pour les [services disponibles](https://godoc.org/github.com/Azure/azure-sdk-for-go) et les [packages AutoRest](https://godoc.org/github.com/Azure/go-autorest). Les packages courants que vous devez obtenir de `go-autorest` sont :

| Package | DESCRIPTION |
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

Si vous avez besoin d’un instantané collectif des services, vous pouvez également sélectionner une version de profil unique. Actuellement, le seul profil verrouillé est la version `2017-03-30`, qui ne dispose peut-être pas des fonctionnalités les plus récentes des services. Les profils sont situés sous le module `profiles`, avec leur version au format `YYYY-MM-DD`. Les services sont regroupées sous leur version de profil. Par exemple, pour importer le module de Gestion des ressources Azure à partir du profil `2017-03-09` :

```go
import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/resources/mgmt/resources"
```

> [!WARNING]
> Les profils `preview` et `latest` sont également disponibles. Leur utilisation n’est pas recommandée. Ces profils sont des versions continues et le comportement du service peut changer à tout moment.

## <a name="next-steps"></a>étapes suivantes

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
