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
ms.openlocfilehash: ae460dbf21b13c40f3d564274f8b790afe005aae
ms.sourcegitcommit: af3473779cd7c2978f290fbdc51ee15eb1130840
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/15/2018
---
# <a name="quickstart-deploy-an-azure-virtual-machine-from-a-template-with-the-azure-sdk-for-go"></a><span data-ttu-id="9086c-104">Démarrage rapide : déployer une machine virtuelle à partir d’un modèle avec le kit de développement logiciel Microsoft Azure SDK pour Go</span><span class="sxs-lookup"><span data-stu-id="9086c-104">Quickstart: Deploy an Azure virtual machine from a template with the Azure SDK for Go</span></span>

<span data-ttu-id="9086c-105">Ce démarrage rapide porte sur le déploiement de ressources à partir d’un modèle avec le kit de développement logiciel Microsoft Azure SDK pour Go.</span><span class="sxs-lookup"><span data-stu-id="9086c-105">This quickstart focuses on deploying resources from a template with the Azure SDK for Go.</span></span> <span data-ttu-id="9086c-106">Les modèles sont des instantanés de toutes les ressources contenues dans un [groupe de ressources Azure](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview).</span><span class="sxs-lookup"><span data-stu-id="9086c-106">Templates are snapshots of all of the resources contained within an [Azure resource group](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview).</span></span> <span data-ttu-id="9086c-107">Durant la procédure, vous vous familiariserez avec les fonctionnalités et les conventions du kit de développement logiciel (SDK) lors de l’exécution d’une tâche utile.</span><span class="sxs-lookup"><span data-stu-id="9086c-107">Along the way, you'll become familiar with the functionality and conventions of the SDK while performing a useful task.</span></span>

<span data-ttu-id="9086c-108">À la fin de ce démarrage rapide, vous devrez connecter une machine virtuelle en cours d’exécution avec un nom d’utilisateur et un mot de passe.</span><span class="sxs-lookup"><span data-stu-id="9086c-108">At the end of this quickstart, you have a running VM that you log into with a username and password.</span></span>

[!INCLUDE [quickstarts-free-trial-note](includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](includes/cloud-shell-try-it.md)]

<span data-ttu-id="9086c-109">Si vous utilisez une installation locale de l’interface de ligne de commande Azure, ce démarrage rapide requiert la version d’interface CLI 2.0.24 ou une version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="9086c-109">If you use a local install of the Azure CLI, this quickstart requires CLI version 2.0.24 or later.</span></span> <span data-ttu-id="9086c-110">Exécutez `az --version` pour vous assurer que votre installation d’interface de ligne de commande répond à cette exigence.</span><span class="sxs-lookup"><span data-stu-id="9086c-110">Run `az --version` to make sure your CLI install meets this requirement.</span></span> <span data-ttu-id="9086c-111">Si vous devez installer ou mettre à niveau, consultez [Installation d’Azure CLI 2.0](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="9086c-111">If you need to install or upgrade, see [Install the Azure CLI 2.0](/cli/azure/install-azure-cli).</span></span>

## <a name="install-the-azure-sdk-for-go"></a><span data-ttu-id="9086c-112">Installer le kit de développement logiciel Microsoft Azure SDK pour Go</span><span class="sxs-lookup"><span data-stu-id="9086c-112">Install the Azure SDK for Go</span></span> 

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

## <a name="create-a-service-principal"></a><span data-ttu-id="9086c-113">Créer un principal du service</span><span class="sxs-lookup"><span data-stu-id="9086c-113">Create a service principal</span></span>

<span data-ttu-id="9086c-114">Pour vous connecter en mode non interactif à une application, vous avez besoin d’un principal de service.</span><span class="sxs-lookup"><span data-stu-id="9086c-114">To log in non-interactively with an application, you need a service principal.</span></span> <span data-ttu-id="9086c-115">Les principaux de service font partie du contrôle d’accès basé sur le rôle (RBAC), qui crée une identité d’utilisateur unique.</span><span class="sxs-lookup"><span data-stu-id="9086c-115">Service principals are part of role-based access control (RBAC), which creates a unique user identity.</span></span> <span data-ttu-id="9086c-116">Pour créer un principal de service avec l’interface CLI, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="9086c-116">To create a new service principal with the CLI, run the following command:</span></span>

```azurecli-interactive
az ad sp create-for-rbac --name az-go-vm-quickstart
```

<span data-ttu-id="9086c-117">__Assurez-vous__ d’enregistrer les valeurs `appId`, `password`, et `tenant` dans la sortie.</span><span class="sxs-lookup"><span data-stu-id="9086c-117">__Make sure__ to record the `appId`, `password`, and `tenant` values in the output.</span></span> <span data-ttu-id="9086c-118">Ces valeurs sont utilisées par l’application pour s’authentifier auprès d’Azure.</span><span class="sxs-lookup"><span data-stu-id="9086c-118">These values are used by the application to authenticate with Azure.</span></span>

<span data-ttu-id="9086c-119">Pour plus d’informations sur la création et la gestion de principaux de service avec Azure CLI 2.0, consultez [Créer un principal du service Azure avec Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="9086c-119">For more information on creating and managing service principals with the Azure CLI 2.0, see [Create an Azure service principal with Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli).</span></span>

## <a name="get-the-code"></a><span data-ttu-id="9086c-120">Obtenir le code</span><span class="sxs-lookup"><span data-stu-id="9086c-120">Get the code</span></span>

<span data-ttu-id="9086c-121">Obtenez le code de démarrage rapide et toutes ses dépendances avec `go get`.</span><span class="sxs-lookup"><span data-stu-id="9086c-121">Get the quickstart code and all of its dependencies with `go get`.</span></span>

```bash
go get -u -d github.com/azure-samples/azure-sdk-for-go-samples/quickstart/deploy-vm/...
```

<span data-ttu-id="9086c-122">Ce code se compile, mais pour qu’il s’exécute correctement vous devez fournir plus d’informations sur votre compte Azure et le principal de service créé.</span><span class="sxs-lookup"><span data-stu-id="9086c-122">This code compiles, but doesn't run correctly until you provide it information about your Azure account and the created service principal.</span></span> <span data-ttu-id="9086c-123">Dans `main.go` il y a une variable, `config`, qui contient un struct `authInfo`.</span><span class="sxs-lookup"><span data-stu-id="9086c-123">In `main.go` there is a variable, `config`, which contains an `authInfo` struct.</span></span> <span data-ttu-id="9086c-124">Les valeurs de champ de ce struct doivent être remplacées pour pouvoir effectuer l’authentification correctement.</span><span class="sxs-lookup"><span data-stu-id="9086c-124">This struct needs to have its field values replaced in order to authenticate correctly.</span></span>

```go
    config = authInfo{ // Your application credentials
        TenantID:               "", // Azure account tenantID
        SubscriptionID:         "", // Azure subscription subscriptionID
        ServicePrincipalID:     "", // Service principal appId
        ServicePrincipalSecret: "", // Service principal password/secret
    }
```

* <span data-ttu-id="9086c-125">`SubscriptionID` : votre ID d’abonnement, qui peut être obtenu à partir de la commande d’interface CLI</span><span class="sxs-lookup"><span data-stu-id="9086c-125">`SubscriptionID`: Your subscription ID, which can be obtained from the CLI command</span></span>

  ```azurecli-interactive
  az account show --query id -o tsv
  ```

* <span data-ttu-id="9086c-126">`TenantID` : votre ID client, la `tenant` valeur enregistrée lors de la création du principal de service</span><span class="sxs-lookup"><span data-stu-id="9086c-126">`TenantID`: Your tenant ID, the `tenant` value recorded when creating the service principal</span></span>
* <span data-ttu-id="9086c-127">`ServicePrincipalID` : la valeur `appId` enregistrée lors de la création du principal de service</span><span class="sxs-lookup"><span data-stu-id="9086c-127">`ServicePrincipalID`: The `appId` value recorded when creating the service principal</span></span>
* <span data-ttu-id="9086c-128">`ServicePrincipalSecret` : la valeur `password` enregistrée lors de la création du principal de service</span><span class="sxs-lookup"><span data-stu-id="9086c-128">`ServicePrincipalSecret`: The `password` value recorded when creating the service principal</span></span>

<span data-ttu-id="9086c-129">Vous devez également modifier une valeur dans le fichier `vm-quickstart-params.json`.</span><span class="sxs-lookup"><span data-stu-id="9086c-129">You also need to edit a value in the `vm-quickstart-params.json` file.</span></span>

```json
    "vm_password": {
        "value": "_"
    }
```

* <span data-ttu-id="9086c-130">`vm_password` : le mot de passe du compte d’utilisateur de la machine virtuelle.</span><span class="sxs-lookup"><span data-stu-id="9086c-130">`vm_password`: The password for the VM user account.</span></span> <span data-ttu-id="9086c-131">Il doit comporter 12 à 72 caractères et contenir 3 des caractères suivants :</span><span class="sxs-lookup"><span data-stu-id="9086c-131">It must be 12-72 characters in length and contain 3 of the following characters:</span></span>
  * <span data-ttu-id="9086c-132">une lettre minuscule</span><span class="sxs-lookup"><span data-stu-id="9086c-132">A lowercase letter</span></span>
  * <span data-ttu-id="9086c-133">une lettre majuscule</span><span class="sxs-lookup"><span data-stu-id="9086c-133">An uppercase letter</span></span>
  * <span data-ttu-id="9086c-134">un chiffre</span><span class="sxs-lookup"><span data-stu-id="9086c-134">A number</span></span>
  * <span data-ttu-id="9086c-135">un symbole</span><span class="sxs-lookup"><span data-stu-id="9086c-135">A symbol</span></span>

## <a name="running-the-code"></a><span data-ttu-id="9086c-136">Exécution du code</span><span class="sxs-lookup"><span data-stu-id="9086c-136">Running the code</span></span>

<span data-ttu-id="9086c-137">Exécutez le démarrage rapide avec la commande `go run`.</span><span class="sxs-lookup"><span data-stu-id="9086c-137">Run the quickstart with the `go run` command.</span></span>

```bash
cd $GOPATH/src/github.com/azure-samples/azure-sdk-for-go-samples/quickstart/deploy-vm
go run main.go
```

<span data-ttu-id="9086c-138">En cas de défaillance dans le déploiement, vous obtenez un message indiquant qu’un problème a eu lieu, mais sans détails spécifiques.</span><span class="sxs-lookup"><span data-stu-id="9086c-138">If there is a failure in the deployment, you get a message indicating that there was an issue, but without any specific details.</span></span> <span data-ttu-id="9086c-139">À l’aide de l’interface de ligne de commande Azure, obtenez les détails de l’échec du déploiement avec la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="9086c-139">Using the Azure CLI, get the details of the deployment failure with the following command:</span></span>

```azurecli-interactive
az group deployment show -g GoVMQuickstart -n VMDeployQuickstart
```

<span data-ttu-id="9086c-140">Si le déploiement réussit, un message comportant le nom d’utilisateur, l’adresse IP et le mot de passe pour se connecter à la machine virtuelle nouvellement créée s’affiche.</span><span class="sxs-lookup"><span data-stu-id="9086c-140">If the deployment is successful, you see a message giving the username, IP address, and password for logging into the newly created virtual machine.</span></span> <span data-ttu-id="9086c-141">Connectez-vous avec SSH à cette machine pour vérifier qu’elle est en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="9086c-141">SSH into this machine to confirm that it is up and running.</span></span>

## <a name="cleaning-up"></a><span data-ttu-id="9086c-142">Nettoyage</span><span class="sxs-lookup"><span data-stu-id="9086c-142">Cleaning up</span></span>

<span data-ttu-id="9086c-143">Nettoyez les ressources créées au cours de ce démarrage rapide en supprimant le groupe de ressources à l’aide de l’interface CLI.</span><span class="sxs-lookup"><span data-stu-id="9086c-143">Clean up the resources created during this quickstart by deleting the resource group with the CLI.</span></span>

```azurecli-interactive
az group delete -n GoVMQuickstart
```

## <a name="code-in-depth"></a><span data-ttu-id="9086c-144">Le code en profondeur</span><span class="sxs-lookup"><span data-stu-id="9086c-144">Code in depth</span></span>

<span data-ttu-id="9086c-145">Le code de démarrage rapide est divisé en un bloc de variables et plusieurs petites fonctions, qui sont toutes décrites ici.</span><span class="sxs-lookup"><span data-stu-id="9086c-145">What the quickstart code does is broken down into a block of variables and several small functions, each of which are discussed here.</span></span>

### <a name="variable-assignments-and-structs"></a><span data-ttu-id="9086c-146">Les structs et les attribution de variables</span><span class="sxs-lookup"><span data-stu-id="9086c-146">Variable assignments and structs</span></span>

<span data-ttu-id="9086c-147">Étant donné que le démarrage rapide est autonome, il utilise les variables globales plutôt que les options de ligne de commande ou les variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="9086c-147">Since quickstart is self-contained, it uses global variables rather than command-line options or environment variables.</span></span>

```go
type authInfo struct {
        TenantID               string
        SubscriptionID         string
        ServicePrincipalID     string
        ServicePrincipalSecret string
}
```

<span data-ttu-id="9086c-148">Le struct `authInfo` est déclaré pour encapsuler toutes les informations nécessaires pour l’autorisation avec les services Azure.</span><span class="sxs-lookup"><span data-stu-id="9086c-148">The `authInfo` struct is declared to encapsulate all of the information needed for authorization with Azure services.</span></span>

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

<span data-ttu-id="9086c-149">Les valeurs sont déclarées, ce qui donne les noms des ressources créées.</span><span class="sxs-lookup"><span data-stu-id="9086c-149">Values are declared which give the names of created resources.</span></span> <span data-ttu-id="9086c-150">L’emplacement est également spécifié ici, vous pouvez le modifier pour voir comment les déploiements se comportent dans d’autres centres de données.</span><span class="sxs-lookup"><span data-stu-id="9086c-150">The location is also specified here, which you can change to see how deployments behave in other datacenters.</span></span> <span data-ttu-id="9086c-151">Chaque centre de données ne dispose pas de toutes les ressources requises disponibles.</span><span class="sxs-lookup"><span data-stu-id="9086c-151">Not every datacenter has all of the required resources available.</span></span>

<span data-ttu-id="9086c-152">Les constantes `templateFile` et `parametersFile` pointent vers les fichiers nécessaires au déploiement.</span><span class="sxs-lookup"><span data-stu-id="9086c-152">The `templateFile` and `parametersFile` constants point to the files needed for deployment.</span></span> <span data-ttu-id="9086c-153">Le jeton du principal de service est décrit ultérieurement, et la variable `ctx` est un [contexte Go](https://blog.golang.org/context) pour les opérations de réseau.</span><span class="sxs-lookup"><span data-stu-id="9086c-153">The service principal token is covered later, and the `ctx` variable is a [Go context](https://blog.golang.org/context) for the network operations.</span></span>

### <a name="init-and-authorization"></a><span data-ttu-id="9086c-154">init() et l’autorisation</span><span class="sxs-lookup"><span data-stu-id="9086c-154">init() and authorization</span></span>

<span data-ttu-id="9086c-155">La méthode `init()` pour le code définit l’autorisation.</span><span class="sxs-lookup"><span data-stu-id="9086c-155">The `init()` method for the code sets up authorization.</span></span> <span data-ttu-id="9086c-156">Étant donné que l’autorisation est une condition préalable pour tous les éléments du démarrage rapide, il est judicieux d’en disposer dans le cadre de l’initialisation.</span><span class="sxs-lookup"><span data-stu-id="9086c-156">Since authorization is a precondition for everything in the quickstart, it makes sense to have it as part of initialization.</span></span> 

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

<span data-ttu-id="9086c-157">Ce code effectue deux étapes pour l’autorisation :</span><span class="sxs-lookup"><span data-stu-id="9086c-157">This code completes two steps for authorization:</span></span>

* <span data-ttu-id="9086c-158">Les informations de configuration OAuth pour le `TenantID` sont récupérées en créant une interface avec Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="9086c-158">OAuth configuration information for the `TenantID` is retrieved by interfacing with Azure Active Directory.</span></span> <span data-ttu-id="9086c-159">L’objet [`azure.PublicCloud`](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#PublicCloud) contient des points de terminaison utilisés dans la configuration Azure standard.</span><span class="sxs-lookup"><span data-stu-id="9086c-159">The [`azure.PublicCloud`](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#PublicCloud) object contains endpoints used in the standard Azure configuration.</span></span>
* <span data-ttu-id="9086c-160">La Fonction [`adal.NewServicePrincipalToken()`](https://godoc.org/github.com/Azure/go-autorest/autorest/adal#NewServicePrincipalToken) est appelée.</span><span class="sxs-lookup"><span data-stu-id="9086c-160">The [`adal.NewServicePrincipalToken()`](https://godoc.org/github.com/Azure/go-autorest/autorest/adal#NewServicePrincipalToken) function is called.</span></span> <span data-ttu-id="9086c-161">Cette fonction accepte les informations OAuth, ainsi que la connexion du service principal et le style de gestion Azure qui est utilisé.</span><span class="sxs-lookup"><span data-stu-id="9086c-161">This function takes the OAuth information along with the service principal login, as well as which style of Azure management is being used.</span></span> <span data-ttu-id="9086c-162">Sauf si vous avez des exigences spécifiques et savez ce que vous faites, cette valeur doit toujours être `.ResourceManagerEndpoint`.</span><span class="sxs-lookup"><span data-stu-id="9086c-162">Unless you have specific requirements and know what you're doing, this value should always be `.ResourceManagerEndpoint`.</span></span>

### <a name="flow-of-operations-in-main"></a><span data-ttu-id="9086c-163">Flux des opérations dans main()</span><span class="sxs-lookup"><span data-stu-id="9086c-163">Flow of operations in main()</span></span>

<span data-ttu-id="9086c-164">La fonction `main()` est simple, elle indique uniquement le flux des opérations et exécute la vérification des erreurs.</span><span class="sxs-lookup"><span data-stu-id="9086c-164">The `main()` function is simple, only indicating the flow of operations and performing error-checking.</span></span>

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

<span data-ttu-id="9086c-165">Les étapes que le code parcourt sont, dans l’ordre :</span><span class="sxs-lookup"><span data-stu-id="9086c-165">The steps that the code runs through are, in order:</span></span>

* <span data-ttu-id="9086c-166">Créer le groupe de ressources à déployer sur (`createGroup()`)</span><span class="sxs-lookup"><span data-stu-id="9086c-166">Create the resource group to deploy to (`createGroup()`)</span></span>
* <span data-ttu-id="9086c-167">Créer le déploiement au sein de ce groupe (`createDeployment()`)</span><span class="sxs-lookup"><span data-stu-id="9086c-167">Create the deployment within this group (`createDeployment()`)</span></span>
* <span data-ttu-id="9086c-168">Obtenir et afficher des informations de connexion pour la machine virtuelle déployée (`getLogin()`)</span><span class="sxs-lookup"><span data-stu-id="9086c-168">Obtain and display login information for the deployed VM (`getLogin()`)</span></span>

### <a name="creating-the-resource-group"></a><span data-ttu-id="9086c-169">Création du groupe de ressources</span><span class="sxs-lookup"><span data-stu-id="9086c-169">Creating the resource group</span></span>

<span data-ttu-id="9086c-170">La fonction `createGroup()` crée le groupe de ressources.</span><span class="sxs-lookup"><span data-stu-id="9086c-170">The `createGroup()` function creates the resource group.</span></span> <span data-ttu-id="9086c-171">Examiner le flux des appels et les arguments illustre la façon dont les interactions du service sont structurées dans le kit de développement logiciel (SDK).</span><span class="sxs-lookup"><span data-stu-id="9086c-171">Looking at the call flow and arguments demonstrates the way that service interactions are structured in the SDK.</span></span>

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

<span data-ttu-id="9086c-172">Le flux général d’interaction avec un service Azure est :</span><span class="sxs-lookup"><span data-stu-id="9086c-172">The general flow of interacting with an Azure service is:</span></span>

* <span data-ttu-id="9086c-173">Créez le client à l’aide de la méthode `service.NewXClient()`, où `X` est le type de ressource du `service` avec lequel vous souhaitez interagir.</span><span class="sxs-lookup"><span data-stu-id="9086c-173">Create the client using the `service.NewXClient()` method, where `X` is the resource type of the `service` that you want to interact with.</span></span> <span data-ttu-id="9086c-174">Cette fonction prend toujours un ID d’abonnement.</span><span class="sxs-lookup"><span data-stu-id="9086c-174">This function always takes a subscription ID.</span></span>
* <span data-ttu-id="9086c-175">Définissez la méthode d’autorisation pour le client, en lui permettant d’interagir avec l’API distante.</span><span class="sxs-lookup"><span data-stu-id="9086c-175">Set the authorization method for the client, allowing it to interact with the remote API.</span></span>
* <span data-ttu-id="9086c-176">Faites correspondre l’appel de méthode sur le client avec l’API distante.</span><span class="sxs-lookup"><span data-stu-id="9086c-176">Make the method call on the client corresponding to the remote API.</span></span> <span data-ttu-id="9086c-177">Les méthodes de client du service prennent généralement le nom de la ressource et un objet de métadonnées.</span><span class="sxs-lookup"><span data-stu-id="9086c-177">Service client methods usually take the name of the resource and a metadata object.</span></span>

<span data-ttu-id="9086c-178">La fonction [`to.StringPtr()`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) est utilisée ici pour effectuer une conversion de type.</span><span class="sxs-lookup"><span data-stu-id="9086c-178">The [`to.StringPtr()`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) function is used to perform a type conversion here.</span></span> <span data-ttu-id="9086c-179">Les structs de paramètres pour les méthodes du kit de développement logiciel (SDK) acceptent presque exclusivement des pointeurs, ces méthodes sont fournies pour faciliter les conversions de type.</span><span class="sxs-lookup"><span data-stu-id="9086c-179">The parameters structs for methods of the SDK almost exclusively take pointers, so these methods are provided to make the type conversions easy.</span></span> <span data-ttu-id="9086c-180">Consultez la documentation relative au module [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) pour la liste complète et le comportement des convertisseurs de commodité.</span><span class="sxs-lookup"><span data-stu-id="9086c-180">See the documentation for the [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) module for the complete list and behavior of convenience converters.</span></span>

<span data-ttu-id="9086c-181">L’opération `groupsClient.CreateOrUpdate()` retourne un pointeur vers un struct de données qui représente le groupe de ressources.</span><span class="sxs-lookup"><span data-stu-id="9086c-181">The `groupsClient.CreateOrUpdate()` operation returns a pointer to a data struct representing the resource group.</span></span> <span data-ttu-id="9086c-182">Une valeur de retour directe de ce type indique une opération d’exécution courte qui est destinée à être synchrone.</span><span class="sxs-lookup"><span data-stu-id="9086c-182">A direct return value of this kind indicates a short-running operation that is meant to be synchronous.</span></span> <span data-ttu-id="9086c-183">Dans la section suivante, vous verrez un exemple d’une opération longue et comment interagir avec ces dernières.</span><span class="sxs-lookup"><span data-stu-id="9086c-183">In the next section, you'll see an example of a long-running operation and how to interact with them.</span></span>

### <a name="performing-the-deployment"></a><span data-ttu-id="9086c-184">Effectuer le déploiement</span><span class="sxs-lookup"><span data-stu-id="9086c-184">Performing the deployment</span></span>

<span data-ttu-id="9086c-185">Une fois que le groupe qui contient ses ressources est créé, il est temps d’exécuter le déploiement.</span><span class="sxs-lookup"><span data-stu-id="9086c-185">Once the group to contain its resources is created, it's time to run the deployment.</span></span> <span data-ttu-id="9086c-186">Ce code est divisé en sections plus petites pour mettre en évidence les différentes parties de sa logique.</span><span class="sxs-lookup"><span data-stu-id="9086c-186">This code is broken up into smaller sections to emphasize different parts of its logic.</span></span>


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

<span data-ttu-id="9086c-187">Les fichiers de déploiement sont chargés par `readJSON`, dont détails sont ignorés ici.</span><span class="sxs-lookup"><span data-stu-id="9086c-187">The deployment files are loaded by `readJSON`, the details of which are skipped here.</span></span> <span data-ttu-id="9086c-188">Cette fonction retourne un `*map[string]interface{}`, le type utilisé pour construire les métadonnées pour l’appel de déploiement de ressources.</span><span class="sxs-lookup"><span data-stu-id="9086c-188">This function returns a `*map[string]interface{}`, the type used in constructing the metadata for the resource deployment call.</span></span>

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

<span data-ttu-id="9086c-189">Ce code suit le même modèle que pour la création du groupe de ressources.</span><span class="sxs-lookup"><span data-stu-id="9086c-189">This code follows the same pattern as with creating the resource group.</span></span> <span data-ttu-id="9086c-190">Un nouveau client est créé, la possibilité de s’authentifier avec Azure lui est donnée, puis une méthode est appelée.</span><span class="sxs-lookup"><span data-stu-id="9086c-190">A new client is created, given the ability to authenticate with Azure, and then a method is called.</span></span> <span data-ttu-id="9086c-191">La méthode a le même nom (`CreateOrUpdate`) que la méthode correspondante pour les groupes de ressources.</span><span class="sxs-lookup"><span data-stu-id="9086c-191">The method even has the same name (`CreateOrUpdate`) as the corresponding method for resource groups.</span></span> <span data-ttu-id="9086c-192">Ce modèle est régulièrement utilisé dans les Kit de développement logiciel (SDK).</span><span class="sxs-lookup"><span data-stu-id="9086c-192">This pattern is seen again and again in the SDK.</span></span> <span data-ttu-id="9086c-193">Les méthodes qui effectuent un travail similaire portent généralement le même nom.</span><span class="sxs-lookup"><span data-stu-id="9086c-193">Methods that perform similar work normally have the same name.</span></span>

<span data-ttu-id="9086c-194">La différence principale réside dans la valeur de retour de la méthode `deploymentsClient.CreateOrUpdate()`.</span><span class="sxs-lookup"><span data-stu-id="9086c-194">The biggest difference comes in the return value of the `deploymentsClient.CreateOrUpdate()` method.</span></span> <span data-ttu-id="9086c-195">Cette valeur est un objet `Future` qui suit le [modèle de conception de perspective](https://en.wikipedia.org/wiki/Futures_and_promises).</span><span class="sxs-lookup"><span data-stu-id="9086c-195">This value is a `Future` object, which follows the [future design pattern](https://en.wikipedia.org/wiki/Futures_and_promises).</span></span> <span data-ttu-id="9086c-196">Les perspectives représentent une opération longue dans Azure, que vous voudrez parfois interroger lors de l’exécution d’autres tâches.</span><span class="sxs-lookup"><span data-stu-id="9086c-196">Futures represent a long-running operation in Azure that you may want to occasionally poll while performing other work.</span></span>

```go
        //...
        err = deploymentFuture.Future.WaitForCompletion(ctx, deploymentsClient.BaseClient.Client)
        if err != nil {
                log.Fatalf("Error while waiting for deployment creation: %v", err)
        }
        return deploymentFuture.Result(deploymentsClient)
}
```

<span data-ttu-id="9086c-197">Pour cet exemple, la meilleure chose à faire est d’attendre que l’opération se termine.</span><span class="sxs-lookup"><span data-stu-id="9086c-197">For this example, the best thing to do is to wait for the operation to complete.</span></span> <span data-ttu-id="9086c-198">L’attente d’une perspective requiert à la fois un [objet de contexte](https://blog.golang.org/context) et le client ayant créé l’objet de perspective.</span><span class="sxs-lookup"><span data-stu-id="9086c-198">Waiting on a future requires both a [context object](https://blog.golang.org/context) and the client that created the Future object.</span></span> <span data-ttu-id="9086c-199">Il existe deux sources d’erreur possibles ici : une erreur côté client lors de la tentative d’appel de la méthode et une réponse d’erreur provenant du serveur.</span><span class="sxs-lookup"><span data-stu-id="9086c-199">There are two possible error sources here: An error caused on the client side when trying to invoke the method, and an error response from the server.</span></span> <span data-ttu-id="9086c-200">Cette dernière est retournée dans le cadre de l’appel `deploymentFuture.Result()`.</span><span class="sxs-lookup"><span data-stu-id="9086c-200">The latter is returned as part of the `deploymentFuture.Result()` call.</span></span>

### <a name="obtaining-the-assigned-ip-address"></a><span data-ttu-id="9086c-201">Obtention de l’adresse IP attribuée</span><span class="sxs-lookup"><span data-stu-id="9086c-201">Obtaining the assigned IP address</span></span>

<span data-ttu-id="9086c-202">Pour toute opération avec la machine virtuelle nouvellement créée, vous aurez besoin de l’adresse IP attribuée.</span><span class="sxs-lookup"><span data-stu-id="9086c-202">To do anything with the newly created VM, you need the assigned IP address.</span></span> <span data-ttu-id="9086c-203">Les adresses IP sont des ressources Azure distinctes et autonomes, liées aux ressources du contrôleur d’interface réseau.</span><span class="sxs-lookup"><span data-stu-id="9086c-203">IP addresses are their own separate Azure resource, bound to Network Interface Controller (NIC) resources.</span></span>

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

<span data-ttu-id="9086c-204">Cette méthode s’appuie sur les informations stockées dans le fichier de paramètres.</span><span class="sxs-lookup"><span data-stu-id="9086c-204">This method relies on information that is stored in the parameters file.</span></span> <span data-ttu-id="9086c-205">Le code peut interroger la machine virtuelle directement pour obtenir sa carte d’interface réseau, interroger la carte d’interface réseau pour obtenir sa ressource IP et interroger directement la ressource IP.</span><span class="sxs-lookup"><span data-stu-id="9086c-205">The code could query the VM directly to get its NIC, query the NIC to get its IP resource, and then query the IP resource directly.</span></span> <span data-ttu-id="9086c-206">C’est une longue chaîne de dépendances et d’opérations à résoudre, ce qui rend le processus coûteux.</span><span class="sxs-lookup"><span data-stu-id="9086c-206">That's a long chain of dependencies and operations to resolve, making it expensive.</span></span> <span data-ttu-id="9086c-207">Étant donné que les informations JSON sont locales, elles peuvent être chargées à la place.</span><span class="sxs-lookup"><span data-stu-id="9086c-207">Since the JSON information is local, it can be loaded instead.</span></span>

<span data-ttu-id="9086c-208">Les valeurs et le mot de passe pour l’utilisateur de la machine virtuelle sont également chargées à partir de JSON.</span><span class="sxs-lookup"><span data-stu-id="9086c-208">The values for the VM user and password are likewise loaded from the JSON.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9086c-209">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="9086c-209">Next steps</span></span>

<span data-ttu-id="9086c-210">Dans ce guide de démarrage rapide, vous avez pris un modèle existant et l’avez déployé via Go.</span><span class="sxs-lookup"><span data-stu-id="9086c-210">In this quickstart, you took an existing template and deployed it through Go.</span></span> <span data-ttu-id="9086c-211">Puis vous l’avez connecté à la machine virtuelle nouvellement créée via le protocole SSH pour vous assurer qu’il s’exécute.</span><span class="sxs-lookup"><span data-stu-id="9086c-211">Then you connected to the newly created VM via SSH to ensure that it's running.</span></span>

<span data-ttu-id="9086c-212">Pour en savoir sur l’utilisation des machines virtuelles dans l’environnement Azure avec Go, consultez les [exemples de calcul Azure pour Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) ou les [exemples de gestion de ressources Azure pour Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).</span><span class="sxs-lookup"><span data-stu-id="9086c-212">To continue learning about working with virtual machines in the Azure environment with Go, take a look at the [Azure compute samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) or [Azure resource management samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).</span></span>
