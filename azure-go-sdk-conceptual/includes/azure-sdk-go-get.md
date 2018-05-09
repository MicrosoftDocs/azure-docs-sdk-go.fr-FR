---
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 02/14/2018
ms.topic: include
ms.prod: azure
ms.technology: azure-cli
ms.openlocfilehash: ddd58efdbc0c2d3ded068a9bebf2466db702566f
ms.sourcegitcommit: f08abf902b48f8173aa6e261084ff2cfc9043305
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
Le [kit de développement logiciel Microsoft Azure SDK pour Go](https://github.com/Azure/azure-sdk-for-go) est compatible avec les versions de Go 1.8 et ultérieures. Pour les environnements utilisant des [profils Azure Stack](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-version-profiles), la version 1.9 de Go est la configuration minimale requise.
Si Go n’est pas disponible sur votre système, suivez [les instructions d’installation Go](https://golang.org/doc/install).

Vous pouvez obtenir le kit de développement logiciel (SDK) Azure pour Go et ses dépendances via `go get`.

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> Assurez-vous de mettre `Azure` en majuscules dans l’URL. Procéder autrement peut entraîner des problèmes d’importation liés à la casse lorsque vous travaillez avec le kit de développement logiciel (SDK). Vous devez également mettre `Azure` en majuscules dans vos instructions d’importation.

