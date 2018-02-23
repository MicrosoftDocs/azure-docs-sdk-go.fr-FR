---
title: "Déployer une machine virtuelle Azure à partir de Go"
description: "Déployer une machine virtuelle à l’aide du kit de développement logiciel Microsoft Azure SDK pour Go."
keywords: Azure, machine virtuelle, Go, Golang, SDK Azure
author: sptramer
ms.author: sttramer
ms.date: 02/08/2018
ms.topic: quickstart
ms.devlang: go
manager: routlaw
ms.openlocfilehash: e530d944deca40e9e6c29b6c2768e2367822714e
ms.sourcegitcommit: aaa8c37880332625f858a38f5918e6cf581bf48d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/15/2018
---
# <a name="quickstart-deploy-an-azure-virtual-machine-from-a-template-with-the-azure-sdk-for-go"></a>Démarrage rapide : déployer une machine virtuelle à partir d’un modèle avec le kit de développement logiciel Microsoft Azure SDK pour Go

Ce démarrage rapide porte sur le déploiement de ressources à partir d’un modèle avec le kit de développement logiciel Microsoft Azure SDK pour Go. Les modèles sont des instantanés de toutes les ressources contenues dans un [groupe de ressources Azure](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview). Durant la procédure, vous vous familiariserez avec les fonctionnalités et les conventions du kit de développement logiciel (SDK) lors de l’exécution d’une tâche utile.

À la fin de ce démarrage rapide, vous devrez connecter une machine virtuelle en cours d’exécution avec un nom d’utilisateur et un mot de passe.

[!INCLUDE [quickstarts-free-trial-note](includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](includes/cloud-shell-try-it.md)]

Si vous utilisez une installation locale de l’interface de ligne de commande Azure, ce démarrage rapide requiert la version d’interface CLI 2.0.24 ou une version ultérieure. Exécutez `az --version` pour vous assurer que votre installation d’interface de ligne de commande répond à cette exigence. Si vous devez installer ou mettre à niveau, consultez [Installation d’Azure CLI 2.0](/cli/azure/install-azure-cli).

## <a name="install-the-azure-sdk-for-go"></a>Installer le kit de développement logiciel Microsoft Azure SDK pour Go 

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

## <a name="create-a-service-principal"></a>Créer un principal du service

Pour vous connecter en mode non interactif à une application, vous avez besoin d’un principal de service. Les principaux de service font partie de l’authentification basée sur le rôle (RBAC), qui crée une identité d’utilisateur unique. Pour créer un principal de service avec l’interface CLI, exécutez la commande suivante :

```azurecli-interactive
az ad sp create-for-rbac --name az-go-vm-quickstart
```

__Assurez-vous__ d’enregistrer les valeurs `appId`, `password`, et `tenant` dans la sortie. Ces valeurs sont utilisées par l’application pour s’authentifier auprès d’Azure.

Pour plus d’informations sur la création et la gestion de principaux de service avec Azure CLI 2.0, consultez [Créer un principal du service Azure avec Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli).

## <a name="get-the-code"></a>Obtenir le code

Obtenez le code de démarrage rapide et toutes ses dépendances avec `go get`.

```bash
go get -u -d github.com/azure-samples/azure-sdk-for-go-samples/quickstart/deploy-vm/...
```

Ce code se compile, mais pour qu’il s’exécute correctement vous devez fournir plus d’informations sur votre compte Azure et le principal de service créé. Dans `main.go` il y a une variable, `config`, qui contient un struct `authInfo`. Les valeurs de champ de ce struct doivent être remplacées pour pouvoir effectuer l’authentification correctement.

```go
    config = authInfo{ // Your application credentials
        TenantID:               "", // Azure account tenantID
        SubscriptionID:         "", // Azure subscription subscriptionID
        ServicePrincipalID:     "", // Service principal appId
        ServicePrincipalSecret: "", // Service principal password/secret
    }
```

* `SubscriptionID` : votre ID d’abonnement, qui peut être obtenu à partir de la commande d’interface CLI

  ```azurecli-interactive
  az account show --query id -o tsv
  ```

* `TenantID` : votre ID client, la `tenant` valeur enregistrée lors de la création du principal de service
* `ServicePrincipalID` : la valeur `appId` enregistrée lors de la création du principal de service
* `ServicePrincipalSecret` : la valeur `password` enregistrée lors de la création du principal de service

Vous devez également modifier une valeur dans le fichier `vm-quickstart-params.json`.

```json
    "vm_password": {
        "value": "_"
    }
```

* `vm_password` : le mot de passe du compte d’utilisateur de la machine virtuelle. Il doit comporter 6 à 72 caractères et contenir 3 des caractères suivants :
  * une lettre minuscule
  * une lettre majuscule
  * un chiffre
  * un symbole

## <a name="running-the-code"></a>Exécution du code

Exécutez le démarrage rapide avec la commande `go run`.

```bash
cd $GOPATH/src/github.com/azure-samples/azure-sdk-for-go-samples/quickstart/deploy-vm
go run main.go
```

En cas de défaillance dans le déploiement, vous obtenez un message indiquant qu’un problème a eu lieu, mais sans détails spécifiques. À l’aide de l’interface de ligne de commande Azure, obtenez les détails de l’échec du déploiement avec la commande suivante :

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

### <a name="variable-assignments-and-structs"></a>Les structs et les attribution de variables

Étant donné que le démarrage rapide est autonome, il utilise les variables globales plutôt que les options de ligne de commande ou les variables d’environnement.

```go
type authInfo struct {
        TenantID               string
        SubscriptionID         string
        ServicePrincipalID     string
        ServicePrincipalSecret string
}
```

Le struct `authInfo` est déclaré pour encapsuler toutes les informations nécessaires pour l’autorisation avec les services Azure.

```go
const (
    resourceGroupName     = "GoVMQuickstart"
    resourceGroupLocation = "eastus"

    deploymentName = "VMDeployQuickstart"
    templateFile   = "vm-quickstart-template.json"
    parametersFile = "vm-quickstart-params.json"
)

var (
    config = authInfo{ // Your application credentials
        TenantID:               "", // Azure account tenantID
        SubscriptionID:         "", // Azure subscription subscriptionID
        ServicePrincipalID:     "", // Service principal appId
        ServicePrincipalSecret: "", // Service principal password/secret
    }

    ctx = context.Background()

    token *adal.ServicePrincipalToken
)
```

Les valeurs sont déclarées, ce qui donne les noms des ressources créées. L’emplacement est également spécifié ici, vous pouvez le modifier pour voir comment les déploiements se comportent dans d’autres centres de données. Chaque centre de données ne dispose pas de toutes les ressources requises disponibles.

Les constantes `templateFile` et `parametersFile` pointent vers les fichiers nécessaires au déploiement. Le jeton du principal de service est décrit ultérieurement, et la variable `ctx` est un [contexte Go](https://blog.golang.org/context) pour les opérations de réseau.

### <a name="init-and-authorization"></a>init() et l’autorisation

La méthode `init()` pour le code définit l’autorisation. Étant donné que l’autorisation est une condition préalable pour tous les éléments du démarrage rapide, il est judicieux d’en disposer dans le cadre de l’initialisation. 

```go
// Authenticate with the Azure services over OAuth, using a service principal.
func init() {
    oauthConfig, err := adal.NewOAuthConfig(azure.PublicCloud.ActiveDirectoryEndpoint, config.TenantID)
    if err != nil {
        log.Fatalf("Failed to get OAuth config: %v\n", err)
    }
    token, err = adal.NewServicePrincipalToken(
        *oauthConfig,
        config.ServicePrincipalID,
        config.ServicePrincipalSecret,
        azure.PublicCloud.ResourceManagerEndpoint)
    if err != nil {
        log.Fatalf("faled to get token: %v\n", err)
    }
}
```

Ce code effectue deux étapes pour l’autorisation :

* Les informations de configuration OAuth pour le `TenantID` sont récupérées en créant une interface avec Azure Active Directory. L’objet [`azure.PublicCloud`](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#PublicCloud) contient des points de terminaison utilisés dans la configuration Azure standard.
* La Fonction [`adal.NewServicePrincipalToken()`](https://godoc.org/github.com/Azure/go-autorest/autorest/adal#NewServicePrincipalToken) est appelée. Cette fonction accepte les informations OAuth, ainsi que la connexion du service principal et le style de gestion Azure qui est utilisé. Sauf si vous avez des exigences spécifiques et savez ce que vous faites, cette valeur doit toujours être `.ResourceManagerEndpoint`.

### <a name="flow-of-operations-in-main"></a>Flux des opérations dans main()

La fonction `main()` est simple, elle indique uniquement le flux des opérations et exécute la vérification des erreurs.

```go
func main() {
    group, err := createGroup()
    if err != nil {
        log.Fatalf("failed to create group: %v", err)
    }
    log.Printf("created group: %v\n", *group.Name)

    log.Println("starting deployment")
    result, err := createDeployment()
    if err != nil {
        log.Fatalf("Failed to deploy correctly: %v", err)
    }
    log.Printf("Completed deployment: %v", *result.Name)
    getLogin()
}
```

Les étapes que le code parcourt sont, dans l’ordre :

* Créer le groupe de ressources à déployer sur (`createGroup()`)
* Créer le déploiement au sein de ce groupe (`createDeployment()`)
* Obtenir et afficher des informations de connexion pour la machine virtuelle déployée (`getLogin()`)

### <a name="creating-the-resource-group"></a>Création du groupe de ressources

La fonction `createGroup()` crée le groupe de ressources. Examiner le flux des appels et les arguments illustre la façon dont les interactions du service sont structurées dans le kit de développement logiciel (SDK).

```go
func createGroup() (group resources.Group, err error) {
        groupsClient := resources.NewGroupsClient(config.SubscriptionID)
        groupsClient.Authorizer = autorest.NewBearerAuthorizer(token)

        return groupsClient.CreateOrUpdate(
                ctx,
                resourceGroupName,
                resources.Group{
                        Location: to.StringPtr(resourceGroupLocation)})
}
```

Le flux général d’interaction avec un service Azure est :

* Créez le client à l’aide de la méthode `service.NewXClient()`, où `X` est le type de ressource du `service` avec lequel vous souhaitez interagir. Cette fonction prend toujours un ID d’abonnement.
* Définissez la méthode d’autorisation pour le client, en lui permettant d’interagir avec l’API distante.
* Faites correspondre l’appel de méthode sur le client avec l’API distante. Les méthodes de client du service prennent généralement le nom de la ressource et un objet de métadonnées.

La fonction [`to.StringPtr()`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) est utilisée ici pour effectuer une conversion de type. Les structs de paramètres pour les méthodes du kit de développement logiciel (SDK) acceptent presque exclusivement des pointeurs, ces méthodes sont fournies pour faciliter les conversions de type. Consultez la documentation relative au module [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) pour la liste complète et le comportement des convertisseurs de commodité.

L’opération `groupsClient.CreateOrUpdate()` retourne un pointeur vers un struct de données qui représente le groupe de ressources. Une valeur de retour directe de ce type indique une opération d’exécution courte qui est destinée à être synchrone. Dans la section suivante, vous verrez un exemple d’une opération longue et comment interagir avec ces dernières.

### <a name="performing-the-deployment"></a>Effectuer le déploiement

Une fois que le groupe qui contient ses ressources est créé, il est temps d’exécuter le déploiement. Ce code est divisé en sections plus petites pour mettre en évidence les différentes parties de sa logique.


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

        // ...
```

Les fichiers de déploiement sont chargés par `readJSON`, dont détails sont ignorés ici. Cette fonction retourne un `*map[string]interface{}`, le type utilisé pour construire les métadonnées pour l’appel de déploiement de ressources.

```go
        // ...
        
        deploymentsClient := resources.NewDeploymentsClient(config.SubscriptionID)
        deploymentsClient.Authorizer = autorest.NewBearerAuthorizer(token)

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
                log.Fatalf("Failed to create deployment: %v", err)
        }
        //...
```

Ce code suit le même modèle que pour la création du groupe de ressources. Un nouveau client est créé, la possibilité de s’authentifier avec Azure lui est donnée, puis une méthode est appelée. La méthode a le même nom (`CreateOrUpdate`) que la méthode correspondante pour les groupes de ressources. Ce modèle est régulièrement utilisé dans les Kit de développement logiciel (SDK). Les méthodes qui effectuent un travail similaire portent généralement le même nom.

La différence principale réside dans la valeur de retour de la méthode `deploymentsClient.CreateOrUpdate()`. Cette valeur est un objet `Future` qui suit le [modèle de conception de perspective](https://en.wikipedia.org/wiki/Futures_and_promises). Les perspectives représentent une opération longue dans Azure, que vous voudrez parfois interroger lors de l’exécution d’autres tâches.

```go
        //...
        err = deploymentFuture.Future.WaitForCompletion(ctx, deploymentsClient.BaseClient.Client)
        if err != nil {
                log.Fatalf("Error while waiting for deployment creation: %v", err)
        }
        return deploymentFuture.Result(deploymentsClient)
}
```

Pour cet exemple, la meilleure chose à faire est d’attendre que l’opération se termine. L’attente d’une perspective requiert à la fois un [objet de contexte](https://blog.golang.org/context) et le client ayant créé l’objet de perspective. Il existe deux sources d’erreur possibles ici : une erreur côté client lors de la tentative d’appel de la méthode et une réponse d’erreur provenant du serveur. Cette dernière est retournée dans le cadre de l’appel `deploymentFuture.Result()`.

### <a name="obtaining-the-assigned-ip-address"></a>Obtention de l’adresse IP attribuée

Pour toute opération avec la machine virtuelle nouvellement créée, vous aurez besoin de l’adresse IP attribuée. Les adresses IP sont des ressources Azure distinctes et autonomes, liées aux ressources du contrôleur d’interface réseau.

```go
func getLogin() {
        params, err := readJSON(parametersFile)
        if err != nil {
                log.Fatalf("Unable to read parameters. Get login information with `az network public-ip list -g %s", resourceGroupName)
        }

        addressClient := network.NewPublicIPAddressesClient(config.SubscriptionID)
        addressClient.Authorizer = autorest.NewBearerAuthorizer(token)
        ipName := (*params)["publicIPAddresses_QuickstartVM_ip_name"].(map[string]interface{})
        ipAddress, err := addressClient.Get(ctx, resourceGroupName, ipName["value"].(string), "")
        if err != nil {
                log.Fatalf("Unable to get IP information. Try using `az network public-ip list -g %s", resourceGroupName)
        }

        vmUser := (*params)["vm_user"].(map[string]interface{})
        vmPass := (*params)["vm_password"].(map[string]interface{})

        log.Printf("Log in with ssh: %s@%s, password: %s",
                vmUser["value"].(string),
                *ipAddress.PublicIPAddressPropertiesFormat.IPAddress,
                vmPass["value"].(string))
}
```

Cette méthode s’appuie sur les informations stockées dans le fichier de paramètres. Le code peut interroger la machine virtuelle directement pour obtenir sa carte d’interface réseau, interroger la carte d’interface réseau pour obtenir sa ressource IP et interroger directement la ressource IP. C’est une longue chaîne de dépendances et d’opérations à résoudre, ce qui rend le processus coûteux. Étant donné que les informations JSON sont locales, elles peuvent être chargées à la place.

Les valeurs et le mot de passe pour l’utilisateur de la machine virtuelle sont également chargées à partir de JSON.

## <a name="next-steps"></a>étapes suivantes

Dans ce guide de démarrage rapide, vous avez pris un modèle existant et l’avez déployé via Go. Puis vous l’avez connecté à la machine virtuelle nouvellement créée via le protocole SSH pour vous assurer qu’il s’exécute.

Pour en savoir sur l’utilisation des machines virtuelles dans l’environnement Azure avec Go, consultez les [exemples de calcul Azure pour Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) ou les [exemples de gestion de ressources Azure pour Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).
