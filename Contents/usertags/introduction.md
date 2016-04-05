# Document : Tags utilisateurs {#httpapi-ref:a401a687-cf26-49bf-b454-abd9a118ded4}

Cette ressource décrit les tags utilisateur des documents.  Un tag est un mot-
clef auquel une valeur est éventuellement associée. Ce tag est associé à
un document pour un utilisateur particulier. 

## URL  {#httpapi-ref:4107fe84-3c08-4890-8f58-1115256a2b25}

L'url d'accès est : `/api/v1/documents/<documentId>/usertags/`

## Méthodes  {#httpapi-ref:9cef4175-3161-4bfb-af98-46e83787d6ff}

La sous-collection usertags implémente les éléments suivants :

* Collection :

| Action   | URL                                          | Action effectuée                                      |
| :-     : | :                                          : | :                                                   : |
| `GET`    | `/api/v1/documents/<documentId>/usertags/`   | [Liste des tags utilisateurs][get_usertag_list]       |
| `POST`   | `/api/v1/documents/<documentId>/usertags/`   | N/A                                                   |
| `PUT`    | `/api/v1/documents/<documentId>/usertags/`   | N/A                                                   |
| `DELETE` | `/api/v1/documents/<documentId>/usertags/`   | N/A                                                   |

* Ressource :

| Action   | URL                                                     | Action effectuée                                              |
| :-     : | :                                                     : | :                                                           : |
| `GET`    | `/api/v1/documents/<documentId>/usertags/<tagid>`       | [Retourne la définition du tag donnée][get_usertag]           |
| `POST`   | `/api/v1/documents/<documentId>/usertags/<tagid>`       | [Ajoute un tag][post_usertag]                                 |
| `PUT`    | `/api/v1/documents/<documentId>/usertags/<tagid>`       | [Modifie un tag existant][put_usertag]                        |
| `DELETE` | `/api/v1/documents/<documentId>/usertags/<tagid>`       | [Supprime un tag][delete_usertag]                             |
|          |                                                         |                                                               |


<!-- links -->
[get_usertag_list]:     #httpapi-ref:4d01fe57-4cd9-4989-adb8-f088762e4236
[get_usertag]:          #httpapi-ref:1ecfb964-e7ff-4adc-b874-f6887b800fc5
[post_usertag]:         #httpapi-ref:8f415def-6741-49a9-b0dd-fb7824c648be
[put_usertag]:          #httpapi-ref:bcca7517-ecff-4e8a-8339-e8defa71bc4c
[delete_usertag]:       #httpapi-ref:433d5c93-a890-4da2-b0a9-3e4089c14d3d