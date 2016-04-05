# Document : Supprimé  {#httpapi-ref:d7788d16-384b-4d5f-816a-4fe2b4deeeed}

Cette collection décrit les [documents][doc_document] de Dynacase supprimés. 

## URL  {#httpapi-ref:ddd114c7-5c75-4edc-8a72-8d83c30064f3}

L'url d'accès est : `/api/v1/trash`

## Méthodes  {#httpapi-ref:4a56e248-efc9-4d5c-a27a-994c55ba425e}

La collection trash implémente les éléments suivants :

* Collection : Il n'y a pas d'accès possible directement auprès de la collection :

| Action   | URL                     | Action effectuée                                          |
| :-     : | :                      :| :                                                       : |
| `GET`    | `/api/v1/trash/`         | [Liste des documents supprimés][get_trash]                |
| `POST`   | `/api/v1/trash/`         | N/A                                                       |
| `PUT`    | `/api/v1/trash/`         | N/A                                                       |
| `DELETE` | `/api/v1/trash/`         | N/A                                                       |

* Ressource :

| Action   | URL                       | Action effectuée                            |
| :-     : | :                        :| :                                   :       |
| `GET`    | `/api/v1/trash/<id>`      | [Retourne le document `id`][trash_doc]      |
| `POST`   | `/api/v1/trash/<id>`      | N/A                                         |
| `PUT`    | `/api/v1/trash/<id>`      | [Restauration du document `id`][restore_doc]|
| `DELETE` | `/api/v1/trash/<id>`      | N/A                                         |


<!-- links -->
[doc_document]: http://docs.anakeen.com/dynacase/3.2/dynacase-doc-core-reference/website/book/core-ref:e01bf76d-481b-41fd-ac64-167a68d34c55.html#core-ref:67929e29-abef-437c-88a3-7f43647c60ff
[trash_doc]: #httpapi-ref:52be10c1-9f46-456b-a22f-24909386567f
[get_trash]: #httpapi-ref:4052b9db-d36c-4535-809f-1fad107e8270
[restore_doc]: #httpapi-ref:21652c32-5695-4cc0-9b71-f4a2b5f33125