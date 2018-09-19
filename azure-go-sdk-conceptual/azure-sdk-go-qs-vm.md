---
title: Déployer une machine virtuelle Azure à partir de Go
description: Déployez une machine virtuelle à l’aide du kit de développement logiciel Azure SDK pour Go.
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 09/05/2018
ms.topic: quickstart
ms.technology: azure-sdk-go
ms.service: virtual-machines
ms.devlang: go
ms.openlocfilehash: a7970be0857fd414d776241b033af0c23457790c
ms.sourcegitcommit: 8b9e10b960150dc08f046ab840d6a5627410db29
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44059133"
---
# <a name="quickstart-deploy-an-azure-virtual-machine-from-a-template-with-the-azure-sdk-for-go"></a><span data-ttu-id="8788c-103">Démarrage rapide : déployer une machine virtuelle à partir d’un modèle avec le kit de développement logiciel Microsoft Azure SDK pour Go</span><span class="sxs-lookup"><span data-stu-id="8788c-103">Quickstart: Deploy an Azure virtual machine from a template with the Azure SDK for Go</span></span>

<span data-ttu-id="8788c-104">Ce démarrage rapide vous montre comment déployer des ressources à partir d’un modèle Azure Resource Manager, via Azure SDK pour Go.</span><span class="sxs-lookup"><span data-stu-id="8788c-104">This quickstart shows you how to deploy resources from an Azure Resource Manager template, using the Azure SDK for Go.</span></span> <span data-ttu-id="8788c-105">Les modèles sont des instantanés de toutes les ressources contenues dans un [groupe de ressources Azure](/azure/azure-resource-manager/resource-group-overview).</span><span class="sxs-lookup"><span data-stu-id="8788c-105">Templates are snapshots of all of the resources within an [Azure resource group](/azure/azure-resource-manager/resource-group-overview).</span></span> <span data-ttu-id="8788c-106">Durant la procédure, vous allez vous familiariser avec les fonctionnalités et les conventions du Kit de développement logiciel (SDK).</span><span class="sxs-lookup"><span data-stu-id="8788c-106">Along the way, you'll become familiar with the functionality and conventions of the SDK.</span></span>

<span data-ttu-id="8788c-107">À la fin de ce démarrage rapide, vous devrez connecter une machine virtuelle en cours d’exécution avec un nom d’utilisateur et un mot de passe.</span><span class="sxs-lookup"><span data-stu-id="8788c-107">At the end of this quickstart, you have a running VM that you log into with a username and password.</span></span>

> [!NOTE]
> <span data-ttu-id="8788c-108">Pour voir la création d’une machine virtuelle dans Go sans utiliser de modèle Resource Manager, il existe un [exemple impératif](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm.go) qui montre comment générer et configurer toutes les ressources de machine virtuelle avec le Kit de développement logiciel (SDK).</span><span class="sxs-lookup"><span data-stu-id="8788c-108">To see the creation of a VM in Go without the use of a Resource Manager template, there is an [imperative sample](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm.go) demonstrating how to build and configure all VM resources with the SDK.</span></span> <span data-ttu-id="8788c-109">Le fait d’utiliser un modèle dans cet exemple permet de se concentrer sur les conventions du Kit de développement logiciel (SDK), sans entrer trop dans les détails sur l’architecture de service Azure.</span><span class="sxs-lookup"><span data-stu-id="8788c-109">Using a template in this sample allows a focus on SDK conventions without getting into too many details about Azure service architecture.</span></span>

[!INCLUDE [quickstarts-free-trial-note](includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](includes/cloud-shell-try-it.md)]

<span data-ttu-id="8788c-110">Si vous utilisez une installation locale de l’interface de ligne de commande Azure, ce démarrage rapide requiert la version d’interface CLI __2.0.28__ ou une version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="8788c-110">If you use a local install of the Azure CLI, this quickstart requires CLI version __2.0.28__ or later.</span></span> <span data-ttu-id="8788c-111">Exécutez `az --version` pour vous assurer que votre installation d’interface de ligne de commande répond à cette exigence.</span><span class="sxs-lookup"><span data-stu-id="8788c-111">Run `az --version` to make sure your CLI install meets this requirement.</span></span> <span data-ttu-id="8788c-112">Si vous devez effectuer une installation ou une mise à niveau, consultez [Installer Azure CLI](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="8788c-112">If you need to install or upgrade, see [Install the Azure CLI](/cli/azure/install-azure-cli).</span></span>

## <a name="install-the-azure-sdk-for-go"></a><span data-ttu-id="8788c-113">Installer le kit de développement logiciel Microsoft Azure SDK pour Go</span><span class="sxs-lookup"><span data-stu-id="8788c-113">Install the Azure SDK for Go</span></span>

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

## <a name="create-a-service-principal"></a><span data-ttu-id="8788c-114">Créer un principal du service</span><span class="sxs-lookup"><span data-stu-id="8788c-114">Create a service principal</span></span>

<span data-ttu-id="8788c-115">Pour vous connecter en mode non interactif sur Azure avec une application, vous avez besoin d’un principal de service.</span><span class="sxs-lookup"><span data-stu-id="8788c-115">To sign in non-interactively to Azure with an application, you need a service principal.</span></span> <span data-ttu-id="8788c-116">Les principaux de service font partie du contrôle d’accès basé sur le rôle (RBAC), qui crée une identité d’utilisateur unique.</span><span class="sxs-lookup"><span data-stu-id="8788c-116">Service principals are part of role-based access control (RBAC), which creates a unique user identity.</span></span> <span data-ttu-id="8788c-117">Pour créer un principal de service avec l’interface CLI, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="8788c-117">To create a new service principal with the CLI, run the following command:</span></span>

```azurecli-interactive
az ad sp create-for-rbac --sdk-auth > quickstart.auth
```

<span data-ttu-id="8788c-118">Définissez la variable d’environnement `AZURE_AUTH_LOCATION` pour qu’elle devienne le chemin d’accès complet à ce fichier.</span><span class="sxs-lookup"><span data-stu-id="8788c-118">Set the environment variable `AZURE_AUTH_LOCATION` to be the full path to this file.</span></span> <span data-ttu-id="8788c-119">Ensuite, le kit de développement logiciel (SDK) localise et lit les informations d’identification directement à partir de ce fichier, sans que vous ayez à apporter des modifications ou à enregistrer des informations depuis le principal de service.</span><span class="sxs-lookup"><span data-stu-id="8788c-119">Then the SDK locates and reads the credentials directly from this file, without you having to make any changes or record information from the service principal.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="8788c-120">Obtenir le code</span><span class="sxs-lookup"><span data-stu-id="8788c-120">Get the code</span></span>

<span data-ttu-id="8788c-121">Obtenez le code de démarrage rapide et toutes ses dépendances avec `go get`.</span><span class="sxs-lookup"><span data-stu-id="8788c-121">Get the quickstart code and all of its dependencies with `go get`.</span></span>

```bash
go get -u -d github.com/azure-samples/azure-sdk-for-go-samples/quickstarts/deploy-vm/...
```

<span data-ttu-id="8788c-122">Vous n’avez pas besoin d’apporter des modifications au code source si la variable `AZURE_AUTH_LOCATION` est correctement définie.</span><span class="sxs-lookup"><span data-stu-id="8788c-122">You don't need to make any source code modifications if the `AZURE_AUTH_LOCATION` variable is properly set.</span></span> <span data-ttu-id="8788c-123">Lorsque le programme s’exécute, il charge toutes les informations d’authentification nécessaires à partir de là.</span><span class="sxs-lookup"><span data-stu-id="8788c-123">When the program runs, it loads all the necessary authentication information from there.</span></span>

## <a name="running-the-code"></a><span data-ttu-id="8788c-124">Exécution du code</span><span class="sxs-lookup"><span data-stu-id="8788c-124">Running the code</span></span>

<span data-ttu-id="8788c-125">Exécutez le démarrage rapide avec la commande `go run`.</span><span class="sxs-lookup"><span data-stu-id="8788c-125">Run the quickstart with the `go run` command.</span></span>

```bash
cd $GOPATH/src/github.com/azure-samples/azure-sdk-for-go-samples/quickstarts/deploy-vm
go run main.go
```

<span data-ttu-id="8788c-126">Si le déploiement réussit, un message comportant le nom d’utilisateur, l’adresse IP et le mot de passe pour se connecter à la machine virtuelle nouvellement créée s’affiche.</span><span class="sxs-lookup"><span data-stu-id="8788c-126">If the deployment is successful, you see a message giving the username, IP address, and password for logging into the newly created virtual machine.</span></span> <span data-ttu-id="8788c-127">Connectez-vous avec SSH à cette machine pour vérifier qu’elle est en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="8788c-127">SSH into this machine to see if it's up and running.</span></span> 

## <a name="cleaning-up"></a><span data-ttu-id="8788c-128">Nettoyage</span><span class="sxs-lookup"><span data-stu-id="8788c-128">Cleaning up</span></span>

<span data-ttu-id="8788c-129">Nettoyez les ressources créées au cours de ce démarrage rapide en supprimant le groupe de ressources à l’aide de l’interface CLI.</span><span class="sxs-lookup"><span data-stu-id="8788c-129">Clean up the resources created during this quickstart by deleting the resource group with the CLI.</span></span>

```azurecli-interactive
az group delete -n GoVMQuickstart
```

<span data-ttu-id="8788c-130">Supprimez également le principal de service qui a été créé.</span><span class="sxs-lookup"><span data-stu-id="8788c-130">Also delete the service principal that was created.</span></span> <span data-ttu-id="8788c-131">Dans le fichier `quickstart.auth`, il existe une clé JSON pour `clientId`.</span><span class="sxs-lookup"><span data-stu-id="8788c-131">In the `quickstart.auth` file, there's a JSON key for `clientId`.</span></span> <span data-ttu-id="8788c-132">Copiez cette valeur dans la variable d’environnement `CLIENT_ID_VALUE` et exécutez la commande Azure CLI suivante :</span><span class="sxs-lookup"><span data-stu-id="8788c-132">Copy this value to the `CLIENT_ID_VALUE` environment variable and run the following Azure CLI command:</span></span>

```azurecli-interactive
az ad sp delete --id ${CLIENT_ID_VALUE}
```

<span data-ttu-id="8788c-133">Emplacement où vous indiquez la valeur de `CLIENT_ID_VALUE` depuis `quickstart.auth`.</span><span class="sxs-lookup"><span data-stu-id="8788c-133">Where you supply the value for `CLIENT_ID_VALUE` from `quickstart.auth`.</span></span>

> [!WARNING]
> <span data-ttu-id="8788c-134">L’échec de suppression du principal de service de cette application le laisse inactif dans votre locataire Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="8788c-134">Failing to delete the service principal for this application leaves it active in your Azure Active Directory tenant.</span></span>
> <span data-ttu-id="8788c-135">Lors de la génération du nom et du mot de passe en tant qu’UUID pour le principal de service, vérifiez que vous respectez les pratiques de sécurité appropriées en supprimant l’ensemble des applications Azure Active Directory et principaux de service non utilisés.</span><span class="sxs-lookup"><span data-stu-id="8788c-135">While both the name and password for the service principal are generated as UUIDs, make sure that you follow good security practices by deleting any unused service principals and Azure Active Directory Applications.</span></span>

## <a name="code-in-depth"></a><span data-ttu-id="8788c-136">Le code en profondeur</span><span class="sxs-lookup"><span data-stu-id="8788c-136">Code in depth</span></span>

<span data-ttu-id="8788c-137">Le code de démarrage rapide est divisé en un bloc de variables et plusieurs petites fonctions, qui sont toutes décrites ici.</span><span class="sxs-lookup"><span data-stu-id="8788c-137">What the quickstart code does is broken down into a block of variables and several small functions, each of which are discussed here.</span></span>

### <a name="variables-constants-and-types"></a><span data-ttu-id="8788c-138">Variables, constantes et types</span><span class="sxs-lookup"><span data-stu-id="8788c-138">Variables, constants, and types</span></span>

<span data-ttu-id="8788c-139">Étant donné que le démarrage rapide est autonome, il utilise des variables et des constantes globales.</span><span class="sxs-lookup"><span data-stu-id="8788c-139">Since quickstart is self-contained, it uses global constants and variables.</span></span>

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

<span data-ttu-id="8788c-140">Les valeurs sont déclarées, ce qui donne les noms des ressources créées.</span><span class="sxs-lookup"><span data-stu-id="8788c-140">Values are declared which give the names of created resources.</span></span> <span data-ttu-id="8788c-141">L’emplacement est également spécifié ici, vous pouvez le modifier pour voir comment les déploiements se comportent dans d’autres centres de données.</span><span class="sxs-lookup"><span data-stu-id="8788c-141">The location is also specified here, which you can change to see how deployments behave in other datacenters.</span></span> <span data-ttu-id="8788c-142">Chaque centre de données ne dispose pas de toutes les ressources requises disponibles.</span><span class="sxs-lookup"><span data-stu-id="8788c-142">Not every datacenter has all of the required resources available.</span></span>

<span data-ttu-id="8788c-143">Le type `clientInfo` contient les informations chargées à partir du fichier d’authentification pour configurer les clients dans le Kit de développement logiciel (SDK) et définir le mot de passe de la machine virtuelle.</span><span class="sxs-lookup"><span data-stu-id="8788c-143">The `clientInfo` type holds the information loaded from the authentication file to set up clients in the SDK and set the VM password.</span></span>

<span data-ttu-id="8788c-144">Les constantes `templateFile` et `parametersFile` pointent vers les fichiers nécessaires au déploiement.</span><span class="sxs-lookup"><span data-stu-id="8788c-144">The `templateFile` and `parametersFile` constants point to the files needed for deployment.</span></span> <span data-ttu-id="8788c-145">Le `authorizer` sera configuré par le kit de développement logiciel (SDK) Go pour l’authentification, et la variable `ctx` est un [contexte Go](https://blog.golang.org/context) pour les opérations réseau.</span><span class="sxs-lookup"><span data-stu-id="8788c-145">The `authorizer` will be configured by the Go SDK for authentication, and the `ctx` variable is a [Go context](https://blog.golang.org/context) for the network operations.</span></span>

### <a name="authentication-and-initialization"></a><span data-ttu-id="8788c-146">Initialisation et authentification</span><span class="sxs-lookup"><span data-stu-id="8788c-146">Authentication and initialization</span></span>

<span data-ttu-id="8788c-147">La fonction `init` configure l’authentification.</span><span class="sxs-lookup"><span data-stu-id="8788c-147">The `init` function sets up authentication.</span></span> <span data-ttu-id="8788c-148">Étant donné que l’authentification est une condition préalable pour tous les éléments du démarrage rapide, il est judicieux d’en disposer dans le cadre de l’initialisation.</span><span class="sxs-lookup"><span data-stu-id="8788c-148">Since authentication is a precondition for everything in the quickstart, it makes sense to have it as part of initialization.</span></span> <span data-ttu-id="8788c-149">Elle charge également des informations permettant de configurer les clients et la machine virtuelle à partir du fichier d’authentification.</span><span class="sxs-lookup"><span data-stu-id="8788c-149">It also loads some information needed from the authentication file to configure clients and the VM.</span></span>

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

<span data-ttu-id="8788c-150">Tout d’abord, [auth.NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile) est appelée pour charger les informations d’authentification à partir du fichier situé dans `AZURE_AUTH_LOCATION`.</span><span class="sxs-lookup"><span data-stu-id="8788c-150">First, [auth.NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile) is called to load the authentication information from the file located at `AZURE_AUTH_LOCATION`.</span></span> <span data-ttu-id="8788c-151">Ensuite, ce fichier est chargé manuellement par la fonction `readJSON` (omise ici) pour extraire les deux valeurs nécessaires pour exécuter le reste du programme : l’ID d’abonnement du client, et le secret du principal du service, qui est également utilisé pour le mot de passe de la machine virtuelle.</span><span class="sxs-lookup"><span data-stu-id="8788c-151">Next, this file is loaded manually by the `readJSON` function (omitted here) to pull the two values needed to run the rest of the program: The subscription ID of the client, and the service principal's secret, which is also used for the VM's password.</span></span>

> [!WARNING]
> <span data-ttu-id="8788c-152">Pour simplifier le démarrage rapide, le mot de passe du principal de service est réutilisé.</span><span class="sxs-lookup"><span data-stu-id="8788c-152">To keep the quickstart simple, the service principal password is reused.</span></span> <span data-ttu-id="8788c-153">En production, veillez à ne __jamais__ réutiliser un mot de passe qui permet d’accéder à vos ressources Azure.</span><span class="sxs-lookup"><span data-stu-id="8788c-153">In production, take care to __never__ reuse a password which gives access to your Azure resources.</span></span>

### <a name="flow-of-operations-in-main"></a><span data-ttu-id="8788c-154">Flux des opérations dans main()</span><span class="sxs-lookup"><span data-stu-id="8788c-154">Flow of operations in main()</span></span>

<span data-ttu-id="8788c-155">La fonction `main` est simple, elle indique uniquement le flux des opérations et exécute la vérification des erreurs.</span><span class="sxs-lookup"><span data-stu-id="8788c-155">The `main` function is simple, only indicating the flow of operations and performing error-checking.</span></span>

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

<span data-ttu-id="8788c-156">Les étapes que le code parcourt sont, dans l’ordre :</span><span class="sxs-lookup"><span data-stu-id="8788c-156">The steps that the code runs through are, in order:</span></span>

* <span data-ttu-id="8788c-157">Créer le groupe de ressources à déployer sur (`createGroup`)</span><span class="sxs-lookup"><span data-stu-id="8788c-157">Create the resource group to deploy to (`createGroup`)</span></span>
* <span data-ttu-id="8788c-158">Créer le déploiement au sein de ce groupe (`createDeployment`)</span><span class="sxs-lookup"><span data-stu-id="8788c-158">Create the deployment within this group (`createDeployment`)</span></span>
* <span data-ttu-id="8788c-159">Obtenir et afficher des informations de connexion pour la machine virtuelle déployée (`getLogin`)</span><span class="sxs-lookup"><span data-stu-id="8788c-159">Get and display login information for the deployed VM (`getLogin`)</span></span>

### <a name="create-the-resource-group"></a><span data-ttu-id="8788c-160">Créer le groupe de ressources</span><span class="sxs-lookup"><span data-stu-id="8788c-160">Create the resource group</span></span>

<span data-ttu-id="8788c-161">La fonction `createGroup` crée le groupe de ressources.</span><span class="sxs-lookup"><span data-stu-id="8788c-161">The `createGroup` function creates the resource group.</span></span> <span data-ttu-id="8788c-162">Examiner le flux des appels et les arguments illustre la façon dont les interactions du service sont structurées dans le kit de développement logiciel (SDK).</span><span class="sxs-lookup"><span data-stu-id="8788c-162">Looking at the call flow and arguments demonstrates the way that service interactions are structured in the SDK.</span></span>

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

<span data-ttu-id="8788c-163">Le flux général d’interaction avec un service Azure est :</span><span class="sxs-lookup"><span data-stu-id="8788c-163">The general flow of interacting with an Azure service is:</span></span>

* <span data-ttu-id="8788c-164">Créez le client à l’aide de la méthode `service.New*Client()`, où `*` est le type de ressource du `service` avec lequel vous souhaitez interagir.</span><span class="sxs-lookup"><span data-stu-id="8788c-164">Create the client using the `service.New*Client()` method, where `*` is the resource type of the `service` that you want to interact with.</span></span> <span data-ttu-id="8788c-165">Cette fonction prend toujours un ID d’abonnement.</span><span class="sxs-lookup"><span data-stu-id="8788c-165">This function always takes a subscription ID.</span></span>
* <span data-ttu-id="8788c-166">Définissez la méthode d’autorisation pour le client, en lui permettant d’interagir avec l’API distante.</span><span class="sxs-lookup"><span data-stu-id="8788c-166">Set the authorization method for the client, allowing it to interact with the remote API.</span></span>
* <span data-ttu-id="8788c-167">Faites correspondre l’appel de méthode sur le client avec l’API distante.</span><span class="sxs-lookup"><span data-stu-id="8788c-167">Make the method call on the client corresponding to the remote API.</span></span> <span data-ttu-id="8788c-168">Les méthodes de client du service prennent généralement le nom de la ressource et un objet de métadonnées.</span><span class="sxs-lookup"><span data-stu-id="8788c-168">Service client methods usually take the name of the resource and a metadata object.</span></span>

<span data-ttu-id="8788c-169">La fonction [`to.StringPtr`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) est utilisée ici pour effectuer une conversion de type.</span><span class="sxs-lookup"><span data-stu-id="8788c-169">The [`to.StringPtr`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) function is used to perform a type conversion here.</span></span> <span data-ttu-id="8788c-170">Les paramètres pour les méthodes du kit de développement logiciel (SDK) acceptent presque exclusivement des pointeurs, des méthodes pratiques sont donc fournies pour faciliter les conversions de type.</span><span class="sxs-lookup"><span data-stu-id="8788c-170">The parameters for SDK methods almost exclusively take pointers, so convenience methods are provided to make the type conversions easy.</span></span> <span data-ttu-id="8788c-171">Consultez la documentation relative au module [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) pour la liste complète des convertisseurs de commodité et de leur comportement.</span><span class="sxs-lookup"><span data-stu-id="8788c-171">See the documentation for the [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) module for the complete list of convenience converters and their behavior.</span></span>

<span data-ttu-id="8788c-172">La méthode `groupsClient.CreateOrUpdate` retourne un pointeur vers un type de données qui représente le groupe de ressources.</span><span class="sxs-lookup"><span data-stu-id="8788c-172">The `groupsClient.CreateOrUpdate` method returns a pointer to a data type representing the resource group.</span></span> <span data-ttu-id="8788c-173">Une valeur de retour directe de ce type indique une opération d’exécution courte qui est destinée à être synchrone.</span><span class="sxs-lookup"><span data-stu-id="8788c-173">A direct return value of this kind indicates a short-running operation that is meant to be synchronous.</span></span> <span data-ttu-id="8788c-174">Dans la section suivante, vous verrez un exemple d’une opération longue et comment interagir avec elle.</span><span class="sxs-lookup"><span data-stu-id="8788c-174">In the next section, you'll see an example of a long-running operation and how to interact with it.</span></span>

### <a name="perform-the-deployment"></a><span data-ttu-id="8788c-175">Effectuer le déploiement</span><span class="sxs-lookup"><span data-stu-id="8788c-175">Perform the deployment</span></span>

<span data-ttu-id="8788c-176">Une fois que le groupe de ressources est créé, il est temps d’exécuter le déploiement.</span><span class="sxs-lookup"><span data-stu-id="8788c-176">Once the resource group is created, it's time to run the deployment.</span></span> <span data-ttu-id="8788c-177">Ce code est divisé en sections plus petites pour mettre en évidence les différentes parties de sa logique.</span><span class="sxs-lookup"><span data-stu-id="8788c-177">This code is broken up into smaller sections to emphasize different parts of its logic.</span></span>

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

<span data-ttu-id="8788c-178">Les fichiers de déploiement sont chargés par `readJSON`, dont détails sont ignorés ici.</span><span class="sxs-lookup"><span data-stu-id="8788c-178">The deployment files are loaded by `readJSON`, the details of which are skipped here.</span></span> <span data-ttu-id="8788c-179">Cette fonction retourne un `*map[string]interface{}`, le type utilisé pour construire les métadonnées pour l’appel de déploiement de ressources.</span><span class="sxs-lookup"><span data-stu-id="8788c-179">This function returns a `*map[string]interface{}`, the type used in constructing the metadata for the resource deployment call.</span></span> <span data-ttu-id="8788c-180">Le mot de passe de la machine virtuelle est également défini manuellement sur les paramètres de déploiement.</span><span class="sxs-lookup"><span data-stu-id="8788c-180">The VM's password is also set manually on the deployment parameters.</span></span>

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

<span data-ttu-id="8788c-181">Ce code suit le même modèle que pour la création du groupe de ressources.</span><span class="sxs-lookup"><span data-stu-id="8788c-181">This code follows the same pattern as creating the resource group.</span></span> <span data-ttu-id="8788c-182">Un nouveau client est créé, la possibilité de s’authentifier avec Azure lui est donnée, puis une méthode est appelée.</span><span class="sxs-lookup"><span data-stu-id="8788c-182">A new client is created, given the ability to authenticate with Azure, and then a method is called.</span></span>
<span data-ttu-id="8788c-183">La méthode a le même nom (`CreateOrUpdate`) que la méthode correspondante pour les groupes de ressources.</span><span class="sxs-lookup"><span data-stu-id="8788c-183">The method even has the same name (`CreateOrUpdate`) as the corresponding method for resource groups.</span></span> <span data-ttu-id="8788c-184">Ce modèle est visible dans le Kit de développement logiciel (SDK).</span><span class="sxs-lookup"><span data-stu-id="8788c-184">This pattern is seen throughout the SDK.</span></span>
<span data-ttu-id="8788c-185">Les méthodes qui effectuent un travail similaire portent généralement le même nom.</span><span class="sxs-lookup"><span data-stu-id="8788c-185">Methods that perform similar work normally have the same name.</span></span>

<span data-ttu-id="8788c-186">La différence principale réside dans la valeur de retour de la méthode `deploymentsClient.CreateOrUpdate`.</span><span class="sxs-lookup"><span data-stu-id="8788c-186">The biggest difference comes in the return value of the `deploymentsClient.CreateOrUpdate` method.</span></span> <span data-ttu-id="8788c-187">Cette valeur est du type [Perspective](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#Future), qui suit le [modèle de conception de perspective](https://en.wikipedia.org/wiki/Futures_and_promises).</span><span class="sxs-lookup"><span data-stu-id="8788c-187">This value is of the [Future](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#Future) type, which follows the [future design pattern](https://en.wikipedia.org/wiki/Futures_and_promises).</span></span> <span data-ttu-id="8788c-188">Les perspectives représentent une opération longue dans Azure que vous pouvez interroger, annuler ou bloquer dès leur achèvement.</span><span class="sxs-lookup"><span data-stu-id="8788c-188">Futures represent a long-running operation in Azure that you can poll, cancel, or block on their completion.</span></span>

```go
        //...
    err = deploymentFuture.Future.WaitForCompletion(ctx, deploymentsClient.BaseClient.Client)
    if err != nil {
        return
    }
    return deploymentFuture.Result(deploymentsClient)
}
```

<span data-ttu-id="8788c-189">Pour cet exemple, la meilleure chose à faire est d’attendre que l’opération se termine.</span><span class="sxs-lookup"><span data-stu-id="8788c-189">For this example, the best thing to do is to wait for the operation to complete.</span></span> <span data-ttu-id="8788c-190">L’attente d’une perspective requiert à la fois un [objet de contexte](https://blog.golang.org/context) et le client ayant créé le `Future`.</span><span class="sxs-lookup"><span data-stu-id="8788c-190">Waiting on a future requires both a [context object](https://blog.golang.org/context) and the client that created the `Future`.</span></span> <span data-ttu-id="8788c-191">Il existe deux sources d’erreur possibles ici : une erreur côté client lors de la tentative d’appel de la méthode et une réponse d’erreur provenant du serveur.</span><span class="sxs-lookup"><span data-stu-id="8788c-191">There are two possible error sources here: An error caused on the client side when trying to invoke the method, and an error response from the server.</span></span> <span data-ttu-id="8788c-192">Cette dernière est retournée dans le cadre de l’appel `deploymentFuture.Result`.</span><span class="sxs-lookup"><span data-stu-id="8788c-192">The latter is returned as part of the `deploymentFuture.Result` call.</span></span>

### <a name="get-the-assigned-ip-address"></a><span data-ttu-id="8788c-193">Obtenir l’adresse IP attribuée</span><span class="sxs-lookup"><span data-stu-id="8788c-193">Get the assigned IP address</span></span>

<span data-ttu-id="8788c-194">Pour toute opération avec la machine virtuelle nouvellement créée, vous aurez besoin de l’adresse IP attribuée.</span><span class="sxs-lookup"><span data-stu-id="8788c-194">To do anything with the newly created VM, you need the assigned IP address.</span></span> <span data-ttu-id="8788c-195">Les adresses IP sont des ressources Azure distinctes et autonomes, liées aux ressources du contrôleur d’interface réseau.</span><span class="sxs-lookup"><span data-stu-id="8788c-195">IP addresses are their own separate Azure resource, bound to Network Interface Controller (NIC) resources.</span></span>

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

<span data-ttu-id="8788c-196">Cette méthode s’appuie sur les informations stockées dans le fichier de paramètres.</span><span class="sxs-lookup"><span data-stu-id="8788c-196">This method relies on information that is stored in the parameters file.</span></span> <span data-ttu-id="8788c-197">Le code peut interroger la machine virtuelle directement pour obtenir sa carte d’interface réseau, interroger la carte d’interface réseau pour obtenir sa ressource IP et interroger directement la ressource IP.</span><span class="sxs-lookup"><span data-stu-id="8788c-197">The code could query the VM directly to get its NIC, query the NIC to get its IP resource, and then query the IP resource directly.</span></span> <span data-ttu-id="8788c-198">C’est une longue chaîne de dépendances et d’opérations à résoudre, ce qui rend le processus coûteux.</span><span class="sxs-lookup"><span data-stu-id="8788c-198">That's a long chain of dependencies and operations to resolve, making it expensive.</span></span> <span data-ttu-id="8788c-199">Étant donné que les informations JSON sont locales, elles peuvent être chargées à la place.</span><span class="sxs-lookup"><span data-stu-id="8788c-199">Since the JSON information is local, it can be loaded instead.</span></span>

<span data-ttu-id="8788c-200">La valeur de l’utilisateur de la machine virtuelle est également chargée à partir de JSON.</span><span class="sxs-lookup"><span data-stu-id="8788c-200">The value for the VM user is also loaded from the JSON.</span></span> <span data-ttu-id="8788c-201">Le mot de passe de la machine virtuelle a été chargé précédemment à partir du fichier d’authentification.</span><span class="sxs-lookup"><span data-stu-id="8788c-201">The VM password was loaded earlier from the authentication file.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8788c-202">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="8788c-202">Next steps</span></span>

<span data-ttu-id="8788c-203">Dans ce guide de démarrage rapide, vous avez pris un modèle existant et l’avez déployé via Go.</span><span class="sxs-lookup"><span data-stu-id="8788c-203">In this quickstart, you took an existing template and deployed it through Go.</span></span> <span data-ttu-id="8788c-204">Puis vous l’avez connecté à la machine virtuelle nouvellement créée via le protocole SSH.</span><span class="sxs-lookup"><span data-stu-id="8788c-204">Then you connected to the newly created VM via SSH.</span></span>

<span data-ttu-id="8788c-205">Pour en savoir sur l’utilisation des machines virtuelles dans l’environnement Azure avec Go, consultez les [exemples de calcul Azure pour Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) ou les [exemples de gestion de ressources Azure pour Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).</span><span class="sxs-lookup"><span data-stu-id="8788c-205">To continue learning about working with virtual machines in the Azure environment with Go, take a look at the [Azure compute samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) or [Azure resource management samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).</span></span>

<span data-ttu-id="8788c-206">Pour en savoir plus sur les méthodes d’authentification disponibles dans le Kit de développement logiciel (SDK), et sur les types d’authentification qu’elles prennent en charge, consultez [Authentification avec le Kit de développement logiciel (SDK) Azure pour Go](azure-sdk-go-authorization.md).</span><span class="sxs-lookup"><span data-stu-id="8788c-206">To learn more about the available authentication methods in the SDK, and which authentication types they support, see [Authentication with the Azure SDK for Go](azure-sdk-go-authorization.md).</span></span>
