# Fichiers {#httpapi-ref:f0cb126e-f2df-4f3a-ae73-a08a982241df}

Cette collection décrit l'accès aux fichiers contenu dans les documents. 

## URL  {#httpapi-ref:841de6ab-8f78-4aa2-803f-2bb4fc91697c}

L'url d'accès est : `api/v1/documents/<documentId>/files/<attrid>/<index>`

## Méthodes  {#httpapi-ref:3babfba9-ad57-4b5f-b845-103d17474c0d}

La collection files implémente les éléments suivants :

* Collection :

| Action   | URL                                            | Action effectuée                                                               |
| :-     : | :                                :             | :                                                :                             |
| `GET`    | `api/v1/documents/<documentId>/files/<attrid>` | [Télécharge un fichier d'un attribut de type `file` ou `image`][download_file] |
| `POST`   | `api/v1/temporaryFiles/`                       | N/A                                                                            |
| `PUT`    | `api/v1/temporaryFiles/`                       | N/A                                                                            |
| `DELETE` | `api/v1/temporaryFiles/`                       | N/A                                                                            |



<!-- links -->
[download_file]: #httpapi-ref:674fcf43-a53c-41e0-828d-ab700ff85c77
