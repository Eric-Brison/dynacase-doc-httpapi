# Documents {#httpapi-ref:7fe0e66a-c3ce-4315-aff6-fa345eae31da}

Cette collection décrit les [documents][doc_document] de Dynacase.

## URL {#httpapi-ref:0d3d4576-b7d3-4480-9b00-03d6d20704fa}

L'url d'accès est : `api/v1/documents`

## Méthodes {#httpapi-ref:18b84a9c-9226-4367-870c-2f0737151239}

La collection *documents* implémente les éléments suivants :

* Collection :

| Action   | URL                      | Action effectuée                                                       |
| :-     : | :                      : | :                                                                    : |
| `GET`    | `api/v1/documents/`      | [Retourne la liste des documents][get_documents]                       |
| `POST`   | `api/v1/documents/`      | N/A                                                                    |
| `PUT`    | `api/v1/documents/`      | N/A                                                                    |
| `DELETE` | `api/v1/documents/`      | N/A                                                                    |

* Ressource :

| Action   | URL                        | Action effectuée                         |
| :-     : | :                        : | :                                      : |
| `GET`    | `api/v1/documents/<id>`    | [Retourne le document `id`][get_doc]     |
| `POST`   | `api/v1/documents/<id>`    | N/A                                      |
| `PUT`    | `api/v1/documents/<id>`    | [Modifie le document `id`][update_doc]   |
| `DELETE` | `api/v1/documents/<id>`    | [Supprime le document `id`][delete_doc]  |


<!-- links -->
[doc_document]: ../../../dynacase-doc-core-reference/website/book/core-ref:e01bf76d-481b-41fd-ac64-167a68d34c55.html#core-ref:67929e29-abef-437c-88a3-7f43647c60ff
[get_doc]: #httpapi-ref:1d7b939f-d5fc-4b57-b33f-d216913efc22
[update_doc]: #httpapi-ref:db2cb01a-7325-4f78-8cec-ceac9858caf2
[delete_doc]: #httpapi-ref:3358b3bd-bdf6-44ef-b1d7-438f8eb21067
[get_documents]: #httpapi-ref:2ee6dd78-5b5a-4e00-aba5-4cd85c8a1cdc
