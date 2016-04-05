# Famille {#httpapi-ref:359e9f87-73d0-4c23-b6ef-dcae9b4019b4}

Cette collection décrit les [familles][doc_family] de Dynacase.

## URL {#httpapi-ref:91be7285-5982-4ae7-b2ec-a94de22e173d}

L'url d'accès est : `/api/v1/families`

## Méthodes {#httpapi-ref:de906276-e0d0-47ce-a8ff-1b282a7a89bf}

La collection familles implémente les méthodes suivantes :

* Collection : Il n'y a pas d'accès possible directement auprès de la collection famille :

| Action   | URL                     | Action effectuée   |
| :-     : | :                      :| :                : |
| `GET`    | `/api/v1/families/`     | N/A                |
| `POST`   | `/api/v1/families/`     | N/A                |
| `PUT`    | `/api/v1/families/`     | N/A                |
| `DELETE` | `/api/v1/families/`     | N/A                |

* Ressource :

| Action   | URL                         | Action effectuée                                          |
| :-     : | :                          :| :                                                 :       |
| `GET`    | `/api/v1/families/<famid>`  | [Retourne les propriétés de la famille famid][get_family] |
| `POST`   | `/api/v1/families/<famid>`  | N/A                                                       |
| `PUT`    | `/api/v1/families/<famid>`  | N/A                                                       |
| `DELETE` | `/api/v1/families/<famid>`  | N/A                                                       |

* Sous-ressource /documents/ :

| Action   | URL                                    | Action effectuée                                          |
| :-     : | :                                     :| :                                                 :       |
| `GET`    | `/api/v1/families/<famid>/documents/`  | [Liste des documents de la famille famid][fam_list_document]|
| `POST`   | `/api/v1/families/<famid>/documents/`  | [Créé un document de la famille famid][create_document]   |
| `PUT`    | `/api/v1/families/<famid>/documents/`  | N/A                                                       |
| `DELETE` | `/api/v1/families/<famid>/documents/`  | N/A                                                       |

* Sous-ressource /documents/ :

| Action   | URL                                        | Action effectuée                                          |
| :-     : | :                                         :| :                                                       : |
| `GET`    | `/api/v1/families/<famid>/documents/<id>`  | [Retourne le document `id`][get_doc]                      |
| `POST`   | `/api/v1/families/<famid>/documents/<id>`  | N/A                                                       |
| `PUT`    | `/api/v1/families/<famid>/documents/<id>`  | [Met à jour le document `id`][update_doc]                 |
| `DELETE` | `/api/v1/families/<famid>/documents/<id>`  | [Supprime le document `id`][delete_doc]                   |


<!-- links -->

[doc_family]: ../../../dynacase-doc-core-reference/website/book/core-ref:e01bf76d-481b-41fd-ac64-167a68d34c55.html#core-ref:e263d44b-8357-4450-87bf-11cef8bafb24
[get_family]: #httpapi-ref:6b195156-0cda-47c8-9a9a-04ec13562c9a
[create_document]: #httpapi-ref:e769b476-0033-407c-b453-4e8466e09975
[get_doc]: #httpapi-ref:1d7b939f-d5fc-4b57-b33f-d216913efc22
[update_doc]: #httpapi-ref:db2cb01a-7325-4f78-8cec-ceac9858caf2
[delete_doc]: #httpapi-ref:3358b3bd-bdf6-44ef-b1d7-438f8eb21067
[fam_list_document]: #httpapi-ref:f21d3f3f-82ea-48a9-bb9e-ba986bae9b62
