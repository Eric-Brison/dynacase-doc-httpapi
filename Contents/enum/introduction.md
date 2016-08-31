# Famille : Énuméré {#httpapi-ref:dbffbc8d-dc7a-4f9f-88f2-c3b8bedaef99}

Cette collection décrit les [énumérés][doc_enum] de Dynacase.

## URL {#httpapi-ref:c017bd55-0699-47c3-ae65-edd85413b3b3}

L'url d'accès est : `api/v1/families/<famName>/enumerates/`

## Méthodes {#httpapi-ref:f55c6681-b62a-45c1-ac61-ffd552eda60c}

La sous-collection enumerates implémente les éléments suivants :

* Collection :

| Action   | URL                                                | Action effectuée                                                   |
| :-     : | :                                                : | :                                                                : |
| `GET`    | `api/v1/families/<famName>/enumerates/`            | [Liste des attributs énumérés][get_enum_list]                      |
| `POST`   | `api/v1/families/<famName>/enumerates/`            | N/A                                                                |
| `PUT`    | `api/v1/families/<famName>/enumerates/`            | N/A                                                                |
| `DELETE` | `api/v1/families/<famName>/enumerates/`            | N/A                                                                |

* Ressource :

| Action   | URL                                                     | Action effectuée                                              |
| :-     : | :                                                     : | :                                                           : |
| `GET`    | `api/v1/families/<famName>/enumerates/<attrid>`         | [Retourne la définition de l'énuméré attrid][get_enum]        |
| `POST`   | `api/v1/families/<famName>/enumerates/<attrid>`         | N/A                                                           |
| `PUT`    | `api/v1/families/<famName>/enumerates/<attrid>`         | N/A                                                           |
| `DELETE` | `api/v1/families/<famName>/enumerates/<attrid>`         | N/A                                                           |


<!-- links -->
[doc_enum]: ../../../dynacase-doc-core-reference/website/book//core-ref:eef3e3ec-2d50-41bd-98e1-cc978f0a5178.html#core-ref:eef3e3ec-2d50-41bd-98e1-cc978f0a5178
[get_enum]: #httpapi-ref:bb13e401-1859-4c73-b299-70b801ed7eb0
[get_enum_list]: #httpapi-ref:69fba1cf-5754-4189-ac07-c16f348e7fda
