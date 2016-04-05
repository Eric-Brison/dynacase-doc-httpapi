# Recherche {#httpapi-ref:d65c99b2-7f57-478a-9f76-bcc33aa6bea8}

Cette collection décrit les recherches de Dynacase.

## URL {#httpapi-ref:bcc4ab0b-a9d3-43c4-adc2-2ceb7ab2689e}

L'url d'accès est : `/api/v1/searches`

## Méthodes {#httpapi-ref:b23f5636-f2e4-4b5d-99d1-0ca90a03ca9c}

La collection searches implémente les éléments suivants :

* Collection : Il n'y a pas d'accès possible directement auprès de la collection :

| Action   | URL                         | Action effectuée                                          |
| :-     : | :                          :| :                                                       : |
| `GET`    | `/api/v1/searches/`         | [Liste des recherches][searches_collection]               |
| `POST`   | `/api/v1/searches/`         | N/A                                                       |
| `PUT`    | `/api/v1/searches/`         | N/A                                                       |
| `DELETE` | `/api/v1/searches/`         | N/A                                                       |

<span class="flag inline nota-bene"></span> Une recherche étant un document Dynacase, la création d'une nouvelle
recherche passe par l'utilisation de l'entrée `families/SEARCH/documents/`. De plus, il existe différents [type de recherches][core_search].

* Ressource :

| Action   | URL                                | Action effectuée                                 |
| :-     : | :                            :     | :                                   :            |
| `GET`    | `/api/v1/searches/<id>/documents/` | [Contenu de la recherche `id`][searches_content] |
| `POST`   | `/api/v1/searches/<id>/documents/` | N/A                                              |
| `PUT`    | `/api/v1/searches/<id>/documents/` | N/A                                              |
| `DELETE` | `/api/v1/searches/<id>/documents/` | N/A                                              |


<!-- links -->
[core_search]: ../../../dynacase-doc-core-reference/website/book//core-ref:bda916b0-e564-40fd-b195-c62bbab7b8be.html
[searches_collection]: #httpapi-ref:9b8f4a2b-3f56-4b21-a7b7-bb299b4ac7b3
[searches_content]: #httpapi-ref:b7fb15e0-6e51-4ace-8a5f-aec7d565e24d
