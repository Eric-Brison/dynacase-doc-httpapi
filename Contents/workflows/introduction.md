# Document : Cycle de vie {#httpapi-ref:96956bd0-4151-48df-9007-c007f4572767}

Cette collection décrit les états et transitions du [cycle de vie][workflow]
associé à un document.

## URL  {#httpapi-ref:4bc76553-f81d-4838-b113-3b355216cbf9}

L'url d'accès est : `api/v1/documents/<docid>/workflows/`

## Méthodes  {#httpapi-ref:17732d6d-58df-40bb-a37d-8cb2baa06e15}

La collection *documents* implémente les éléments suivants :

* Collection :

| Action   | URL                                               | Action effectuée                                                              |
| :-     : | :                      :                          | :                                                                    :        |
| `GET`    | `api/v1/documents/<docid>/workflows/states/`      | [Retourne la liste des états][get_states] pour le document `docid`            |
| `POST`   |                                                   | N/A                                                                           |
| `PUT`    |                                                   | N/A                                                                           |
| `DELETE` |                                                   | N/A                                                                           |
| `GET`    | `api/v1/documents/<docid>/workflows/transitions/` | [Retourne la liste des transitions][get_transitions] pour le document `docid` |
| `POST`   |                                                   | N/A                                                                           |
| `PUT`    |                                                   | N/A                                                                           |
| `DELETE` |                                                   | N/A                                                                           |

* Ressource :

| Action   | URL                                                          | Action effectuée                                                                |
| :-     : | :                        :                                   | :                                      :                                        |
| `GET`    | `api/v1/documents/<id>/workflows/states/<stateId>`           | [Retourne les caractéristiques de l'état `stateId`][get_state]                  |
| `POST`   |                                                              | [Change l'état du document `docid`][create_state]                               |
| `PUT`    |                                                              | N/A                                                                             |
| `DELETE` |                                                              | N/A                                                                             |
|          |                                                              |                                                                                 |
| `GET`    | `api/v1/documents/<id>/workflows/transitions/<transitionId>` | [Retourne les caractéristiques de la transition `transitionId`][get_transition] |
| `POST`   |                                                              | [Change la transition du document `docid`][create_transition]                   |
| `PUT`    |                                                              | N/A                                                                             |
| `DELETE` |                                                              | N/A                                                                             |


<!-- links -->
[workflow]:         ../../../dynacase-doc-core-reference/website/book/core-ref:55a53d99-0c24-48d8-8cb9-1caa171f2e9a.html "Définition des Workflows"
[get_states]:       #httpapi-ref:af743cfe-c089-4706-a5fb-a131f68020d2
[get_transitions]:  #httpapi-ref:a91dc2b7-3248-452a-b51e-3f660d7d3cf2
[get_state]:        #httpapi-ref:89142988-9b2b-42f6-af33-f68749c7af35
[create_state]:     #httpapi-ref:f3d33034-af23-48c6-b535-e609266e5bc5
[get_transition]:   #httpapi-ref:d370f800-2a17-4589-90ae-d505b5f71c71
[create_transition]:#httpapi-ref:697b7714-d986-4ae5-8020-a5602cfbe7d5
