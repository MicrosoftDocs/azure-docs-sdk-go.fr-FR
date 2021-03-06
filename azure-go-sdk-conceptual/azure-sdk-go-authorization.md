---
title: Authentification avec le Kit de développement logiciel (SDK) Azure pour Go
description: En savoir plus sur les méthodes d’authentification disponibles dans le Kit de développement logiciel (SDK) Azure pour Go et comment les utiliser.
services: azure
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 09/05/2018
ms.topic: conceptual
ms.technology: azure-sdk-go
ms.devlang: go
ms.subservice: authentication
ms.openlocfilehash: c66d6e65f36586bbe5b030cc3b783057b39668db
ms.sourcegitcommit: be967d413a8bf906470e65a97e61688faa9dd98a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/26/2019
ms.locfileid: "55071148"
---
# <a name="authentication-methods-in-the-azure-sdk-for-go"></a>Méthodes d’authentification dans le Kit de développement logiciel (SDK) Azure pour Go

Azure SDK pour Go propose plusieurs modes d’authentification avec Azure. Ces _types_ d’authentification sont appelés grâce à différentes _méthodes_ d’authentification. Cet article aborde les types et méthodes d’authentification disponibles, et vous aide à déterminer celles adaptées pour votre application.

## <a name="available-authentication-types-and-methods"></a>Méthodes et types d’authentification disponibles

Le Kit de développement logiciel (SDK) Azure pour Go offre différents types d’authentification, utilisant des informations d’identification différentes. Chaque type d’authentification est disponible via différentes méthodes d’authentification, qui correspondent à la façon dont le Kit de développement logiciel (SDK) accepte ces informations d’identification en tant qu’entrées. Le tableau suivant décrit les types d’authentification disponibles et les situations dans lesquelles leur utilisation est recommandée pour votre application.

| Type d'authentification | Recommandé lorsque... |
|---------------------|---------------------|
| Authentification par certificat | Vous avez un certificat X509 qui a été configuré pour un utilisateur ou un principal de service Azure Active Directory (AAD). Pour en savoir plus, consultez [Bien démarrer avec l’authentification par certificat dans Azure Active Directory]. |
| Informations d'identification du client | Vous avez un principal de service configuré pour cette application ou pour la classe d’applications à laquelle elle appartient. Pour en savoir plus, consultez [Créer un principal de service avec l’interface de ligne de commande Azure 2.0]. |
| Identités gérées pour les ressources Azure | Votre application s’exécute sur une ressource Azure qui a été configurée avec une identité managée. Pour en savoir plus, consultez [Identités managées pour les ressources Azure]. |
| Jeton d’appareil | Votre application est destinée à être utilisée de manière interactive __uniquement__. Les utilisateurs peuvent avoir l’authentification multifacteur activée. Les utilisateurs ont accès à un navigateur web pour se connecter. Pour plus d’informations, consultez [Utilisation de l’authentification par jeton d’appareil](#use-device-token-authentication).|
| Nom d’utilisateur/mot de passe | Vous avez une application interactive qui ne peut pas utiliser d’autres méthodes d’authentification. L’authentification multifacteur n’est pas activée pour la connexion AAD de vos utilisateurs. |

> [!IMPORTANT]
> Si vous utilisez un type d’authentification autre que les informations d’identification du client, votre application doit être inscrite dans Azure Active Directory. Pour en savoir plus, consultez [Intégration d’applications dans Azure Active Directory](/azure/active-directory/develop/active-directory-integrating-applications).
>
> [!NOTE]
> Évitez l’authentification par nom d’utilisateur/mot de passe, à moins que vous n’ayez des exigences particulières. Dans les situations où la connexion basée sur l’utilisateur est nécessaire, l’authentification par jeton d’appareil peut en général être utilisée à la place.

[Bien démarrer avec l’authentification par certificat dans Azure Active Directory]: /azure/active-directory/active-directory-certificate-based-authentication-get-started
[Créer un principal de service avec l’interface de ligne de commande Azure 2.0]: /cli/azure/create-an-azure-service-principal-azure-cli
[Identités managées pour les ressources Azure]: /azure/active-directory/managed-identities-azure-resources/overview

Ces types d’authentification sont disponibles par le biais de différentes méthodes.

* [_L’authentification basée sur l’environnement_](#use-environment-based-authentication) lit les informations d’identification directement à partir de l’environnement du programme.
* [_L’authentification basée sur un fichier_](#use-file-based-authentication) charge un fichier contenant les informations d’identification du principal de service.
* [_L’authentification basée sur le client_](#use-an-authentication-client) utilise un objet dans le code et vous charge de fournir les informations d’identification lors de l’exécution du programme.
* [_L’authentification par jeton d’appareil_](#use-device-token-authentication) nécessite que les utilisateurs se connectent de manière interactive via un navigateur web avec un jeton.

Tous les types et fonctions d’authentification sont disponibles dans le package `github.com/Azure/go-autorest/autorest/azure/auth`.

> [!NOTE]
> Évitez l’authentification basée sur le client, à moins que vous n’ayez des exigences particulières. Cette méthode d’authentification favorise les mauvaises pratiques. En utilisant l’authentification basée sur le client en particulier, il devient tentant de coder en dur les informations d’identification. L’écriture d’un code personnalisé pour l’authentification pourrait aussi s’interrompre sous les versions ultérieures du Kit de développement logiciel (SDK) si les exigences d’authentification sont modifiées.

## <a name="use-environment-based-authentication"></a>Utiliser l’authentification basée sur l’environnement

Si vous exécutez votre application dans un environnement contrôlé, l’authentification basée sur l’environnement est un choix évident. Cette méthode d’authentification vous permet de configurer l’environnement de l’interpréteur de commandes avant l’exécution de votre application. Lors de l’exécution, le Kit de développement logiciel (SDK) Go lit ces variables d’environnement pour s’authentifier auprès d’Azure.

L’authentification basée sur l’environnement prend en charge toutes les méthodes d’authentification, à l’exception des jetons d’appareil, évaluées dans l’ordre suivant :

* Informations d'identification du client
* Certificats X509
* Nom d’utilisateur/mot de passe
* Identités gérées pour les ressources Azure

Si les valeurs d’un type d’authentification sont annulées ou s’il est refusé, le Kit de développement logiciel (SDK) essaie automatiquement d’utiliser le type d’authentification suivant. Quand tous les autres types d’authentification disponibles ont été testés, le Kit de développement logiciel (SDK) renvoie une erreur.

Le tableau suivant répertorie les variables d’environnement qui doivent être définies pour chaque type d’authentification pris en charge par l’authentification basée sur l’environnement.


|  Type d'authentification   |     Variable d’environnement     |                                                                                                     Description                                                                                                      |
|------------------------|------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Informations d’identification du client** |      `AZURE_TENANT_ID`       |                                                                    L’ID du locataire Active Directory auquel appartient le principal du service.                                                                     |
|                        |      `AZURE_CLIENT_ID`       |                                                                                       Le nom ou ID du principal du service.                                                                                       |
|                        |    `AZURE_CLIENT_SECRET`     |                                                                                  Secret associé au principal du service.                                                                                   |
|    **Certificate**     |      `AZURE_TENANT_ID`       |                                                                   L’ID du locataire Active Directory avec lequel le certificat est enregistré.                                                                    |
|                        |      `AZURE_CLIENT_ID`       |                                                                              L’ID client d’application associé au certificat.                                                                              |
|                        |   `AZURE_CERTIFICATE_PATH`   |                                                                                       Le chemin d’accès au fichier de certificat client.                                                                                       |
|                        | `AZURE_CERTIFICATE_PASSWORD` |                                                                                       Le mot de passe du certificat client.                                                                                       |
| **Nom d’utilisateur/mot de passe**  |      `AZURE_TENANT_ID`       |                                                                           L’ID du locataire Active Directory auquel appartient l’utilisateur.                                                                           |
|                        |      `AZURE_CLIENT_ID`       |                                                                                              L’ID du client d’application.                                                                                              |
|                        |       `AZURE_USERNAME`       |                                                                                            Le nom d’utilisateur avec lequel se connecter.                                                                                             |
|                        |       `AZURE_PASSWORD`       |                                                                                            Le mot de passe avec lequel se connecter.                                                                                             |
|  **Identité gérée**  |                              | Aucune information d’identification n’est nécessaire pour l’authentification avec identité managée. L’application doit être exécutée sur une ressource Azure configurée pour utiliser des identités managées. Pour plus d’informations, consultez [Identités managées pour les ressources Azure]. |

Pour vous connecter à un point de terminaison cloud ou de gestion autre que le cloud public Azure par défaut, définissez les variables d’environnement suivantes. Voici les raisons les plus courantes pour lesquelles vous devez les définir : si vous utilisez Azure Stack, un cloud dans une autre région géographique ou le modèle de déploiement classique.

| Variable d’environnement | Description  |
|----------------------|--------------|
| `AZURE_ENVIRONMENT` | Le nom de l’environnement de cloud auquel se connecter. |
| `AZURE_AD_RESOURCE` | L’ID de ressource Active Directory à utiliser pour vous connecter, comme une URI, à votre point de terminaison de gestion. |

Lorsque vous utilisez l’authentification basée sur l’environnement, appelez la fonction [NewAuthorizerFromEnvironment](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromEnvironment)afin d’obtenir votre objet d’agent d’autorisation. Cet objet est ensuite défini sur la propriété `Authorizer` des clients pour leur permettre d’accéder à Azure.

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
authorizer, err := auth.NewAuthorizerFromEnvironment()
```

### <a name="authentication-on-azure-stack"></a>Authentification sur Azure Stack

Pour s’authentifier sur Azure Stack, vous devez définir les variables suivantes :

| Variable d’environnement | Description  |
|----------------------|--------------|
| `AZURE_AD_ENDPOINT` | Le point de terminaison Active Directory. |
| `AZURE_AD_RESOURCE` | L’ID de ressource Active Directory. |

Ces variables peuvent être récupérés à partir des informations de métadonnées Azure Stack. Pour récupérer les métadonnées, ouvrez un navigateur web dans votre environnement Azure Stack et utilisez l’url : `(ResourceManagerURL)/metadata/endpoints?api-version=1.0`

L’élément `ResourceManagerURL` dépend du nom de la région, du nom de la machine, ainsi que du nom de domaine complet (FQDN) externe de votre déploiement Azure Stack :

| Environnement | ResourceManagerURL |
|----------------------|--------------|
| Kit de développement | `https://management.local.azurestack.external/` |
| Systèmes intégrés | `https://management.(region).ext-(machine-name).(FQDN)` |

Pour plus d’informations sur l’utilisation d’Azure SDK pour Go sur Azure Stack, consultez [Utilisez des profils de version des API avec Go dans Azure Stack](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-version-profiles-go)

## <a name="use-file-based-authentication"></a>Utiliser l’authentification basée sur un fichier

L’authentification basée sur un fichier utilise un format de fichier généré par [Azure CLI](/cli/azure). Vous pouvez facilement créer ce fichier lors de la création d’un principal de service avec le paramètre `--sdk-auth`. Si vous prévoyez d’utiliser l’authentification basée sur un fichier, assurez-vous que cet argument est fourni lors de la création d’un principal de service. Étant donné que l’interface CLI imprime la sortie vers `stdout`, redirigez la sortie vers un fichier.

```azurecli
az ad sp create-for-rbac --sdk-auth > azure.auth
```

Définissez la variable d’environnement `AZURE_AUTH_LOCATION` dans laquelle se trouve le fichier d’autorisation. Cette variable d’environnement est lue par l’application, et les informations d’identification qu’elle contient sont analysées. Si vous avez besoin de sélectionner le fichier d’autorisation lors de l’exécution, manipulez l’environnement du programme à l’aide de la fonction [os.Setenv](https://golang.org/pkg/os/#Setenv).

Pour charger les informations d’authentification, appelez la fonction [NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile). Contrairement à l’autorisation basée sur l’environnement, l’autorisation basée sur un fichier nécessite un point de terminaison de ressource.

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
authorizer, err := NewAuthorizerFromFile(azure.PublicCloud.ResourceManagerEndpoint)
```

Pour plus d’informations sur l’utilisation des principaux de service et la gestion de leurs autorisations d’accès, consultez [Créer un principal de service avec l’interface de ligne de commande Azure 2.0].

## <a name="use-device-token-authentication"></a>Utiliser l’authentification par jeton d’appareil

Si vous souhaitez que les utilisateurs se connectent de manière interactive, l’authentification par jeton d’appareil est la meilleure option. Ce flux d’authentification passe à l’utilisateur un jeton à coller dans un site de connexion Microsoft, sur lequel il peut ensuite s’authentifier avec un compte Azure Active Directory (AAD). Cette méthode d’authentification prend en charge les comptes pour lesquels l’authentification multifacteur est activée, contrairement à l’authentification par nom d’utilisateur/mot de passe standard.

Pour utiliser l’authentification par jeton d’appareil, vous devez créer un agent d’autorisation [DeviceFlowConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig) avec la fonction [NewDeviceFlowConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewDeviceFlowConfig). Appelez l’[Agent d’autorisation](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig.Authorizer) sur l’objet résultant pour démarrer le processus d’authentification. L’authentification par flux d’appareil bloque l’exécution du programme jusqu’à ce que le flux d’authentification soit entièrement terminé.

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
deviceConfig := auth.NewDeviceFlowConfig(applicationID, tenantID)
authorizer, err := deviceConfig.Authorizer()
```

## <a name="use-an-authentication-client"></a>Utiliser un client d’authentification

Si vous avez besoin d’un type d’authentification spécifique et que vous acceptez que votre programme s’occupe de charger les informations d’authentification de l’utilisateur, vous pouvez utiliser n’importe quel client qui est conforme à l’interface [auth.AuthorizerConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#AuthorizerConfig). Utilisez un type qui implémente cette interface quand :

* vous écrivez un programme interactif ;
* vous utilisez des fichiers config spécialisés ;
* vous avez une exigence qui empêche l’utilisation d’une méthode d’authentification intégrée.

> [!WARNING]
> Ne codez jamais les informations d’identification en dur dans une application. Placer des secrets dans un binaire d’application facilite leur extraction par un intrus, que l’application soit en cours d’exécution ou non. Cela expose toutes les ressources Azure, pour lesquelles les informations d’identification sont autorisées, à des risques !

Le tableau suivant répertorie les types dans le Kit de développement logiciel (SDK) conformes à l’interface `AuthorizerConfig`.

| Type d'authentification | Type d’agent d’autorisation |
|---------------------|-----------------------|
| Authentification par certificat | [ClientCertificateConfig] |
| Informations d'identification du client | [ClientCredentialsConfig] |
| Identités gérées pour les ressources Azure | [MSIConfig] |
| Nom d’utilisateur/mot de passe | [UsernamePasswordConfig] |

[ClientCertificateConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#ClientCertificateConfig
[ClientCredentialsConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#ClientCredentialsConfig
[MSIConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#MSIConfig
[DeviceFlowConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig
[UsernamePasswordConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#UsernamePasswordConfig

Créez un authentificateur avec sa fonction `New` associée, puis appelez `Authorize` sur l’objet résultant pour vous authentifier. Par exemple, pour l’authentification par certificat :

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
certificateAuthorizer := auth.NewClientCertificateConfig(certificatePath, certificatePassword, clientID, tenantID)
authorizerToken, err := certificateAuthorizer.Authorize()
```
