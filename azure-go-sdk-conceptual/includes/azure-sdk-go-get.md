---
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 02/14/2018
ms.topic: include
ms.prod: azure
ms.technology: azure-cli
ms.openlocfilehash: d021dd8ef4744b7c50b296b231bf63481f92411a
ms.sourcegitcommit: 2a3bd491e087a1d0e7d269bed896c029357d62a6
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38988062"
---
<span data-ttu-id="9e5af-101">Le [kit de développement logiciel Microsoft Azure SDK pour Go](https://github.com/Azure/azure-sdk-for-go) est compatible avec les versions de Go 1.8 et ultérieures.</span><span class="sxs-lookup"><span data-stu-id="9e5af-101">The [Azure SDK for Go](https://github.com/Azure/azure-sdk-for-go) is compatible with Go versions 1.8 and later.</span></span> <span data-ttu-id="9e5af-102">Pour les environnements utilisant des [profils Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-version-profiles), la version 1.9 de Go est la configuration minimale requise.</span><span class="sxs-lookup"><span data-stu-id="9e5af-102">For environments using [Azure Stack Profiles](https://docs.microsoft.com/azure/azure-stack/azure-stack-version-profiles), Go version 1.9 is the minimum requirement.</span></span>
<span data-ttu-id="9e5af-103">Si Go n’est pas disponible sur votre système, suivez [les instructions d’installation Go](https://golang.org/doc/install).</span><span class="sxs-lookup"><span data-stu-id="9e5af-103">If you do not have Go available on your system, follow [the Go installation instructions](https://golang.org/doc/install).</span></span>

<span data-ttu-id="9e5af-104">Vous pouvez obtenir le kit de développement logiciel (SDK) Azure pour Go et ses dépendances via `go get`.</span><span class="sxs-lookup"><span data-stu-id="9e5af-104">You can obtain the Azure SDK for Go and its dependencies via `go get`.</span></span>

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> <span data-ttu-id="9e5af-105">Assurez-vous de mettre `Azure` en majuscules dans l’URL.</span><span class="sxs-lookup"><span data-stu-id="9e5af-105">Make sure that you capitalize `Azure` in the URL.</span></span> <span data-ttu-id="9e5af-106">Procéder autrement peut entraîner des problèmes d’importation liés à la casse lorsque vous travaillez avec le kit de développement logiciel (SDK).</span><span class="sxs-lookup"><span data-stu-id="9e5af-106">Doing otherwise can cause case-related import problems when working with the SDK.</span></span> <span data-ttu-id="9e5af-107">Vous devez également mettre `Azure` en majuscules dans vos instructions d’importation.</span><span class="sxs-lookup"><span data-stu-id="9e5af-107">You also need to capitalize `Azure` in your import statements.</span></span>
