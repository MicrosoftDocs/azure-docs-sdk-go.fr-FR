---
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 09/05/2018
ms.topic: include
ms.prod: azure
ms.technology: azure-cli
ms.openlocfilehash: 5df14f939efdd0550b49261c88c8dc6518ada459
ms.sourcegitcommit: 8b9e10b960150dc08f046ab840d6a5627410db29
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44059254"
---
[Azure SDK pour Go](https://github.com/Azure/azure-sdk-for-go) est compatible avec les versions 1.8 et ultérieures de Go. Pour les environnements utilisant des [profils Azure Stack](/azure/azure-stack/user/azure-stack-version-profiles-go), la version 1.9 de Go est la configuration minimale requise.
Si vous devez installer Go, suivez [les instructions d’installation de Go](https://golang.org/doc/install).

Vous pouvez télécharger Azure SDK pour Go et ses dépendances via `go get`.

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> Assurez-vous de mettre `Azure` en majuscules dans l’URL. Procéder autrement peut entraîner des problèmes d’importation liés à la casse lorsque vous travaillez avec le kit de développement logiciel (SDK). Vous devez également mettre `Azure` en majuscules dans vos instructions d’importation.
