# Document : Révision {#httpapi-ref:67f3dee4-5dbb-490b-8dc3-778160f0a47e}

Cette sous-collection décrit les [révisions][doc_revision] des documents.

## URL {#httpapi-ref:ba5ca85c-ab4b-40d1-91b6-3db0d986bf46}

L'url d'accès est : `api/v1/documents/<documentId>/revisions/`

## Méthodes  {#httpapi-ref:ed4f6df0-6b9d-4ba0-a22a-723916291ca3}

La sous-collection `revisions` implémente les éléments suivants :

* Collection :

| Action   | URL                                        | Action effectuée                             |
| :-     : | :                      :                   | :                                            |
| `GET`    | `api/v1/documents/<documentId>/revisions/` | [Liste des urls de révisions][list_revision] |
| `POST`   | `api/v1/documents/<documentId>/revisions/` | N/A                                          |
| `PUT`    | `api/v1/documents/<documentId>/revisions/` | N/A                                          |
| `DELETE` | `api/v1/documents/<documentId>/revisions/` | N/A                                          |

* Ressource :

| Action   | URL                                                        | Action effectuée                      |
| :-     : | :                      :                                   | :                                     |
| `GET`    | `api/v1/documents/<documentId>/revisions/<revisionNumber>` | [Récupère une révision][get_revision] |
| `POST`   | `api/v1/documents/<documentId>/revisions/<revisionNumber>` | N/A                                   |
| `PUT`    | `api/v1/documents/<documentId>/revisions/<revisionNumber>` | N/A                                   |
| `DELETE` | `api/v1/documents/<documentId>/revisions/<revisionNumber>` | N/A                                   |


<!-- links -->

[doc_revision]: ../../../dynacase-doc-core-reference/website/book/core-ref:1cdff481-42e0-4caf-baba-d2348d760ca5.html "Manuel de référence Dynacase Core"
[get_revision]: #httpapi-ref:eb7b6954-0945-4f02-8e10-16e69729c529
[list_revision]: #httpapi-ref:2dd5afbe-1d3d-4830-8241-c93077d88430
