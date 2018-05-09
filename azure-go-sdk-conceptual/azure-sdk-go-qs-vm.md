---
title: Déployer une machine virtuelle Azure à partir de Go
description: Déployer une machine virtuelle à l’aide du kit de développement logiciel Microsoft Azure SDK pour Go.
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 04/03/2018
ms.topic: quickstart
ms.prod: azure
ms.technology: azure-sdk-go
ms.service: virtual-machines
ms.devlang: go
ms.openlocfilehash: 1fbcc54df2a2aebce56c5a5800361f3d3aed1ccc
ms.sourcegitcommit: f08abf902b48f8173aa6e261084ff2cfc9043305
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="quickstart-deploy-an-azure-virtual-machine-from-a-template-with-the-azure-sdk-for-go"></a>Démarrage rapide : déployer une machine virtuelle à partir d’un modèle avec le kit de développement logiciel Microsoft Azure SDK pour Go

Ce démarrage rapide porte sur le déploiement de ressources à partir d’un modèle avec le kit de développement logiciel Microsoft Azure SDK pour Go. Les modèles sont des instantanés de toutes les ressources contenues dans un [groupe de ressources Azure](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview). Durant la procédure, vous vous familiariserez avec les fonctionnalités et les conventions du kit de développement logiciel (SDK) lors de l’exécution d’une tâche utile.

À la fin de ce démarrage rapide, vous devrez connecter une machine virtuelle en cours d’exécution avec un nom d’utilisateur et un mot de passe.

[!INCLUDE [quickstarts-free-trial-note](includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](includes/cloud-shell-try-it.md)]

Si vous utilisez une installation locale de l’interface de ligne de commande Azure, ce démarrage rapide requiert la version d’interface CLI __2.0.28__ ou une version ultérieure. Exécutez `az --version` pour vous assurer que votre installation d’interface de ligne de commande répond à cette exigence. Si vous devez installer ou mettre à niveau, consultez [Installation d’Azure CLI 2.0](/cli/azure/install-azure-cli).

## <a name="install-the-azure-sdk-for-go"></a>Installer le kit de développement logiciel Microsoft Azure SDK pour Go 

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

## <a name="create-a-service-principal"></a>Créer un principal du service


Pour vous connecter en mode non interactif à une application, vous avez besoin d’un principal de service. Les principaux de service font partie du contrôle d’accès basé sur le rôle (RBAC), qui crée une identité d’utilisateur unique. Pour créer un principal de service avec l’interface CLI, exécutez la commande suivante :

```azurecli-interactive
az ad sp create-for-rbac --name az-go-vm-quickstart --sdk-auth > quickstart.auth
```

Définissez la variable d’environnement `AZURE_AUTH_LOCATION` pour qu’elle devienne le chemin d’accès complet à ce fichier. Ensuite, le kit de développement logiciel (SDK) localise et lit les informations d’identification directement à partir de ce fichier, sans que vous ayez à apporter des modifications ou à enregistrer des informations depuis le principal de service.

## <a name="get-the-code"></a>Obtenir le code

Obtenez le code de démarrage rapide et toutes ses dépendances avec `go get`.

```bash
go get -u -d github.com/azure-samples/azure-sdk-for-go-samples/quickstarts/deploy-vm/...
```

Vous n’avez pas besoin d’apporter des modifications au code source si la variable `AZURE_AUTH_LOCATION` est correctement définie. Lorsque le programme s’exécute, il charge toutes les informations d’authentification nécessaires à partir de là.

## <a name="running-the-code"></a>Exécution du code

Exécutez le démarrage rapide avec la commande `go run`.

```bash
cd $GOPATH/src/github.com/azure-samples/azure-sdk-for-go-samples/quickstarts/deploy-vm
go run main.go
```

En cas de défaillance dans le déploiement, vous obtenez un message indiquant qu’un problème a eu lieu, mais il peut ne pas donner assez de détails. À l’aide de l’interface de ligne de commande Azure, obtenez les détails complets de l’échec du déploiement avec la commande suivante :

```azurecli-interactive
az group deployment show -g GoVMQuickstart -n VMDeployQuickstart
```

Si le déploiement réussit, un message comportant le nom d’utilisateur, l’adresse IP et le mot de passe pour se connecter à la machine virtuelle nouvellement créée s’affiche. Connectez-vous avec SSH à cette machine pour vérifier qu’elle est en cours d’exécution.

## <a name="cleaning-up"></a>Nettoyage

Nettoyez les ressources créées au cours de ce démarrage rapide en supprimant le groupe de ressources à l’aide de l’interface CLI.

```azurecli-interactive
az group delete -n GoVMQuickstart
```

## <a name="code-in-depth"></a>Le code en profondeur

Le code de démarrage rapide est divisé en un bloc de variables et plusieurs petites fonctions, qui sont toutes décrites ici.

### <a name="variables-constants-and-types"></a>Variables, constantes et types

Étant donné que le démarrage rapide est autonome, il utilise des variables et des constantes globales.

```go
const (
    resourceGroupName     = "GoVMQuickstart"
    resourceGroupLocation = "eastus"

    deploymentName = "VMDeployQuickstart"
    templateFile   = "vm-quickstart-template.json"
    parametersFile = "vm-quickstart-params.json"
)

// Information loaded from the authorization file to identify the client
type clientInfo struct {
    SubscriptionID string
    VMPassword     string
}

var (
    ctx        = context.Background()
    clientData clientInfo
    authorizer autorest.Authorizer
)
```

Les valeurs sont déclarées, ce qui donne les noms des ressources créées. L’emplacement est également spécifié ici, vous pouvez le modifier pour voir comment les déploiements se comportent dans d’autres centres de données. Chaque centre de données ne dispose pas de toutes les ressources requises disponibles.

Le type `clientInfo` est déclaré pour encapsuler toutes les informations qui doivent être chargées de manière indépendante à partir du fichier d’authentification pour configurer les clients dans le Kit de développement logiciel (SDK) et définir le mot de passe de la machine virtuelle.

Les constantes `templateFile` et `parametersFile` pointent vers les fichiers nécessaires au déploiement. Le `authorizer` sera configuré par le kit de développement logiciel (SDK) Go pour l’authentification, et la variable `ctx` est un [contexte Go](https://blog.golang.org/context) pour les opérations réseau.

### <a name="authentication-and-initialization"></a>Initialisation et authentification

La fonction `init` configure l’authentification. Étant donné que l’authentification est une condition préalable pour tous les éléments du démarrage rapide, il est judicieux d’en disposer dans le cadre de l’initialisation. Elle charge également des informations permettant de configurer les clients et la machine virtuelle à partir du fichier d’authentification.

```go
func init() {
    var err error
    authorizer, err = auth.NewAuthorizerFromFile(azure.PublicCloud.ResourceManagerEndpoint)
    if err != nil {
        log.Fatalf("Failed to get OAuth config: %v", err)
    }

    authInfo, err := readJSON(os.Getenv("AZURE_AUTH_LOCATION"))
    clientData.SubscriptionID = (*authInfo)["subscriptionId"].(string)
    clientData.VMPassword = (*authInfo)["clientSecret"].(string)
}
```

Tout d’abord, [auth.NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile) est appelée pour charger les informations d’authentification à partir du fichier situé dans `AZURE_AUTH_LOCATION`. Ensuite, ce fichier est chargé manuellement par la fonction `readJSON` (omise ici) pour extraire les deux valeurs nécessaires pour exécuter le reste du programme : l’ID d’abonnement du client, et le secret du principal du service, qui est également utilisé pour le mot de passe de la machine virtuelle.

> [!WARNING]
> Pour simplifier le démarrage rapide, le mot de passe du principal de service est réutilisé. En production, veillez à ne __jamais__ réutiliser un mot de passe qui permet d’accéder à vos ressources Azure.

### <a name="flow-of-operations-in-main"></a>Flux des opérations dans main()

La fonction `main` est simple, elle indique uniquement le flux des opérations et exécute la vérification des erreurs.

```go
func main() {
    group, err := createGroup()
    if err != nil {
        log.Fatalf("failed to create group: %v", err)
    }
    log.Printf("Created group: %v", *group.Name)

    log.Printf("Starting deployment: %s", deploymentName)
    result, err := createDeployment()
    if err != nil {
        log.Fatalf("Failed to deploy: %v", err)
    }
    if result.Name != nil {
        log.Printf("Completed deployment %v: %v", deploymentName, *result.Properties.ProvisioningState)
    } else {
        log.Printf("Completed deployment %v (no data returned to SDK)", deploymentName)
    }
    getLogin()
}
```

Les étapes que le code parcourt sont, dans l’ordre :

* Créer le groupe de ressources à déployer sur (`createGroup`)
* Créer le déploiement au sein de ce groupe (`createDeployment`)
* Obtenir et afficher des informations de connexion pour la machine virtuelle déployée (`getLogin`)

### <a name="creating-the-resource-group"></a>Création du groupe de ressources

La fonction `createGroup` crée le groupe de ressources. Examiner le flux des appels et les arguments illustre la façon dont les interactions du service sont structurées dans le kit de développement logiciel (SDK).

```go
func createGroup() (group resources.Group, err error) {
    groupsClient := resources.NewGroupsClient(clientData.SubscriptionID)
    groupsClient.Authorizer = authorizer

        return groupsClient.CreateOrUpdate(
                ctx,
                resourceGroupName,
                resources.Group{
                        Location: to.StringPtr(resourceGroupLocation)})
}
```

Le flux général d’interaction avec un service Azure est :

* Créez le client à l’aide de la méthode `service.New*Client()`, où `*` est le type de ressource du `service` avec lequel vous souhaitez interagir. Cette fonction prend toujours un ID d’abonnement.
* Définissez la méthode d’autorisation pour le client, en lui permettant d’interagir avec l’API distante.
* Faites correspondre l’appel de méthode sur le client avec l’API distante. Les méthodes de client du service prennent généralement le nom de la ressource et un objet de métadonnées.

La fonction [`to.StringPtr`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) est utilisée ici pour effectuer une conversion de type. Les paramètres pour les méthodes du kit de développement logiciel (SDK) acceptent presque exclusivement des pointeurs, des méthodes pratiques sont donc fournies pour faciliter les conversions de type. Consultez la documentation relative au module [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) pour la liste complète des convertisseurs de commodité et de leur comportement.

La méthode `groupsClient.CreateOrUpdate` retourne un pointeur vers un type de données qui représente le groupe de ressources. Une valeur de retour directe de ce type indique une opération d’exécution courte qui est destinée à être synchrone. Dans la section suivante, vous verrez un exemple d’une opération longue et comment interagir avec elle.

### <a name="performing-the-deployment"></a>Effectuer le déploiement

Une fois que le groupe de ressources est créé, il est temps d’exécuter le déploiement. Ce code est divisé en sections plus petites pour mettre en évidence les différentes parties de sa logique.

```go
func createDeployment() (deployment resources.DeploymentExtended, err error) {
    template, err := readJSON(templateFile)
    if err != nil {
        return
    }
    params, err := readJSON(parametersFile)
    if err != nil {
        return
    }
    (*params)["vm_password"] = map[string]string{
        "value": clientData.VMPassword,
    }
        // ...
```

Les fichiers de déploiement sont chargés par `readJSON`, dont détails sont ignorés ici. Cette fonction retourne un `*map[string]interface{}`, le type utilisé pour construire les métadonnées pour l’appel de déploiement de ressources. Le mot de passe de la machine virtuelle est également défini manuellement sur les paramètres de déploiement.

```go
        // ...

    deploymentsClient := resources.NewDeploymentsClient(clientData.SubscriptionID)
    deploymentsClient.Authorizer = authorizer

    deploymentFuture, err := deploymentsClient.CreateOrUpdate(
        ctx,
        resourceGroupName,
        deploymentName,
        resources.Deployment{
            Properties: &resources.DeploymentProperties{
                Template:   template,
                Parameters: params,
                Mode:       resources.Incremental,
            },
        },
    )
    if err != nil {
        return
    }
```

Ce code suit le même modèle que pour la création du groupe de ressources. Un nouveau client est créé, la possibilité de s’authentifier avec Azure lui est donnée, puis une méthode est appelée. La méthode a le même nom (`CreateOrUpdate`) que la méthode correspondante pour les groupes de ressources. Ce modèle est visible dans le Kit de développement logiciel (SDK). Les méthodes qui effectuent un travail similaire portent généralement le même nom.

La différence principale réside dans la valeur de retour de la méthode `deploymentsClient.CreateOrUpdate`. Cette valeur est du type [Perspective](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#Future), qui suit le [modèle de conception de perspective](https://en.wikipedia.org/wiki/Futures_and_promises). Les perspectives représentent une opération longue dans Azure que vous pouvez interroger, annuler ou bloquer dès leur achèvement.

```go
        //...
    err = deploymentFuture.Future.WaitForCompletion(ctx, deploymentsClient.BaseClient.Client)
    if err != nil {
        return
    }
    deployment, err = deploymentFuture.Result(deploymentsClient)

    // Work around possible bugs or late-stage failures
    if deployment.Name == nil || err != nil {
        deployment, _ = deploymentsClient.Get(ctx, resourceGroupName, deploymentName)
    }
    return
```

Pour cet exemple, la meilleure chose à faire est d’attendre que l’opération se termine. L’attente d’une perspective requiert à la fois un [objet de contexte](https://blog.golang.org/context) et le client ayant créé le `Future`. Il existe deux sources d’erreur possibles ici : une erreur côté client lors de la tentative d’appel de la méthode et une réponse d’erreur provenant du serveur. Cette dernière est retournée dans le cadre de l’appel `deploymentFuture.Result`.

Une fois les informations de déploiement récupérées, il existe une solution de contournement des bogues éventuels, à cause desquels les informations de déploiement peuvent être vides, grâce à un appel manuel à `deploymentsClient.Get` pour vous assurer que les données sont remplies.

### <a name="obtaining-the-assigned-ip-address"></a>Obtention de l’adresse IP attribuée

Pour toute opération avec la machine virtuelle nouvellement créée, vous aurez besoin de l’adresse IP attribuée. Les adresses IP sont des ressources Azure distinctes et autonomes, liées aux ressources du contrôleur d’interface réseau.

```go
func getLogin() {
    params, err := readJSON(parametersFile)
    if err != nil {
        log.Fatalf("Unable to read parameters. Get login information with `az network public-ip list -g %s", resourceGroupName)
    }

    addressClient := network.NewPublicIPAddressesClient(clientData.SubscriptionID)
    addressClient.Authorizer = authorizer
    ipName := (*params)["publicIPAddresses_QuickstartVM_ip_name"].(map[string]interface{})
    ipAddress, err := addressClient.Get(ctx, resourceGroupName, ipName["value"].(string), "")
    if err != nil {
        log.Fatalf("Unable to get IP information. Try using `az network public-ip list -g %s", resourceGroupName)
    }

    vmUser := (*params)["vm_user"].(map[string]interface{})

    log.Printf("Log in with ssh: %s@%s, password: %s",
        vmUser["value"].(string),
        *ipAddress.PublicIPAddressPropertiesFormat.IPAddress,
        clientData.VMPassword)
}
```

Cette méthode s’appuie sur les informations stockées dans le fichier de paramètres. Le code peut interroger la machine virtuelle directement pour obtenir sa carte d’interface réseau, interroger la carte d’interface réseau pour obtenir sa ressource IP et interroger directement la ressource IP. C’est une longue chaîne de dépendances et d’opérations à résoudre, ce qui rend le processus coûteux. Étant donné que les informations JSON sont locales, elles peuvent être chargées à la place.

La valeur de l’utilisateur de la machine virtuelle est également chargée à partir de JSON. Le mot de passe de la machine virtuelle a été chargé précédemment à partir du fichier d’authentification.

## <a name="next-steps"></a>Étapes suivantes

Dans ce guide de démarrage rapide, vous avez pris un modèle existant et l’avez déployé via Go. Puis vous l’avez connecté à la machine virtuelle nouvellement créée via le protocole SSH pour vous assurer qu’il s’exécute.

Pour en savoir sur l’utilisation des machines virtuelles dans l’environnement Azure avec Go, consultez les [exemples de calcul Azure pour Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) ou les [exemples de gestion de ressources Azure pour Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).

Pour en savoir plus sur les méthodes d’authentification disponibles dans le Kit de développement logiciel (SDK), et sur les types d’authentification qu’elles prennent en charge, consultez [Authentification avec le Kit de développement logiciel (SDK) Azure pour Go](azure-sdk-go-authorization.md).
