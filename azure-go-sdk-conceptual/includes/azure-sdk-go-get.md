---
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 02/14/2018
ms.topic: include
ms.prod: azure
ms.technology: azure-cli
ms.openlocfilehash: 0febc2cc42ad95a1b7a5032c7987e37cc82f374e
ms.sourcegitcommit: 181d4e0b164cf39b3feac346f559596bd19c94db
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38067031"
---
<span data-ttu-id="c10cb-101">Le [kit de développement logiciel Microsoft Azure SDK pour Go](https://github.com/Azure/azure-sdk-for-go) est compatible avec les versions de Go 1.8 et ultérieures.</span><span class="sxs-lookup"><span data-stu-id="c10cb-101">The [Azure SDK for Go](https://github.com/Azure/azure-sdk-for-go) is compatible with Go versions 1.8 and later.</span></span> <span data-ttu-id="c10cb-102">Pour les environnements utilisant des [profils Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-version-profiles), la version 1.9 de Go est la configuration minimale requise.</span><span class="sxs-lookup"><span data-stu-id="c10cb-102">For environments using [Azure Stack Profiles](https://docs.microsoft.com/azure/azure-stack/azure-stack-version-profiles), Go version 1.9 is the minimum requirement.</span></span>
<span data-ttu-id="c10cb-103">Si Go n’est pas disponible sur votre système, suivez [les instructions d’installation Go](https://golang.org/doc/install).</span><span class="sxs-lookup"><span data-stu-id="c10cb-103">If you do not have Go available on your system, follow [the Go installation instructions](https://golang.org/doc/install).</span></span>

<span data-ttu-id="c10cb-104">Vous pouvez obtenir le kit de développement logiciel (SDK) Azure pour Go et ses dépendances via `go get`.</span><span class="sxs-lookup"><span data-stu-id="c10cb-104">You can obtain the Azure SDK for Go and its dependencies via `go get`.</span></span>

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> <span data-ttu-id="c10cb-105">Assurez-vous de mettre `Azure` en majuscules dans l’URL.</span><span class="sxs-lookup"><span data-stu-id="c10cb-105">Make sure that you capitalize `Azure` in the URL.</span></span> <span data-ttu-id="c10cb-106">Procéder autrement peut entraîner des problèmes d’importation liés à la casse lorsque vous travaillez avec le kit de développement logiciel (SDK).</span><span class="sxs-lookup"><span data-stu-id="c10cb-106">Doing otherwise can cause case-related import problems when working with the SDK.</span></span> <span data-ttu-id="c10cb-107">Vous devez également mettre `Azure` en majuscules dans vos instructions d’importation.</span><span class="sxs-lookup"><span data-stu-id="c10cb-107">You also need to capitalize `Azure` in your import statements.</span></span>

