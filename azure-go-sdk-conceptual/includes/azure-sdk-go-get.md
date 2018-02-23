Le kit de développement logiciel Microsoft Azure SDK pour Go est compatible avec les versions de Go 1.8 et ultérieures. Pour les environnements utilisant des [profils Azure Stack](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-version-profiles), la version 1.9 de Go est la configuration minimale requise. Si Go n’est pas disponible sur votre système, suivez [les instructions d’installation Go](https://golang.org/doc/install).

Vous pouvez obtenir le kit de développement logiciel (SDK) Azure pour Go et ses dépendances via `go get`.

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> Assurez-vous de mettre `Azure` en majuscules dans l’URL. Procéder autrement peut entraîner des problèmes d’importation liés à la casse lorsque vous travaillez avec le kit de développement logiciel (SDK). Vous devez également mettre `Azure` en majuscules dans vos instructions d’importation.

