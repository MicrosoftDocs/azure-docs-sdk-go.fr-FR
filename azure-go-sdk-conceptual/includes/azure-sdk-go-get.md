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
<span data-ttu-id="0ba1b-101">[Azure SDK pour Go](https://github.com/Azure/azure-sdk-for-go) est compatible avec les versions 1.8 et ultérieures de Go.</span><span class="sxs-lookup"><span data-stu-id="0ba1b-101">The [Azure SDK for Go](https://github.com/Azure/azure-sdk-for-go) is compatible with Go versions 1.8 and higher.</span></span> <span data-ttu-id="0ba1b-102">Pour les environnements utilisant des [profils Azure Stack](/azure/azure-stack/user/azure-stack-version-profiles-go), la version 1.9 de Go est la configuration minimale requise.</span><span class="sxs-lookup"><span data-stu-id="0ba1b-102">For environments using [Azure Stack Profiles](/azure/azure-stack/user/azure-stack-version-profiles-go), Go version 1.9 is the minimum requirement.</span></span>
<span data-ttu-id="0ba1b-103">Si vous devez installer Go, suivez [les instructions d’installation de Go](https://golang.org/doc/install).</span><span class="sxs-lookup"><span data-stu-id="0ba1b-103">If you need to install Go, follow [the Go installation instructions](https://golang.org/doc/install).</span></span>

<span data-ttu-id="0ba1b-104">Vous pouvez télécharger Azure SDK pour Go et ses dépendances via `go get`.</span><span class="sxs-lookup"><span data-stu-id="0ba1b-104">You can download the Azure SDK for Go and its dependencies via `go get`.</span></span>

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> <span data-ttu-id="0ba1b-105">Assurez-vous de mettre `Azure` en majuscules dans l’URL.</span><span class="sxs-lookup"><span data-stu-id="0ba1b-105">Make sure that you capitalize `Azure` in the URL.</span></span> <span data-ttu-id="0ba1b-106">Procéder autrement peut entraîner des problèmes d’importation liés à la casse lorsque vous travaillez avec le kit de développement logiciel (SDK).</span><span class="sxs-lookup"><span data-stu-id="0ba1b-106">Doing otherwise can cause case-related import problems when working with the SDK.</span></span> <span data-ttu-id="0ba1b-107">Vous devez également mettre `Azure` en majuscules dans vos instructions d’importation.</span><span class="sxs-lookup"><span data-stu-id="0ba1b-107">You also need to capitalize `Azure` in your import statements.</span></span>
