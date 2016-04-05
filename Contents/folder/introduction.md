# Dossier {#httpapi-ref:5dda257a-b6b2-4cef-ac04-6d42d59154a9}

Cette collection décrit les dossiers de Dynacase. 

Un dossier est un document permettant de stocker un ensemble de documents.

## URL {#httpapi-ref:8b2a3860-c482-4f0e-8d15-04f495b67e72}

L'url d'accès est : `/api/v1/folders`

## Méthodes {#httpapi-ref:ff2321ab-3b4e-4837-84aa-5c67f4f4c8aa}

La collection searches implémente les éléments suivants :

* Collection : Il n'y a pas d'accès possible directement auprès de la collection :

| Action   | URL                          | Action effectuée                                          |
| :-     : | :                          : | :                                                       : |
| `GET`    | `/api/v1/folders/`           | [Liste des dossiers][folders_collection]                  |
| `POST`   | `/api/v1/folders/`           | N/A                                                       |
| `PUT`    | `/api/v1/folders/`           | N/A                                                       |
| `DELETE` | `/api/v1/folders/`           | N/A                                                       |

<span class="flag inline nota-bene"></span> Une recherche étant un document Dynacase, la création d'une nouvelle 
dossier passe par l'utilisation de l'entrée `families/DIR/documents/`.

* Ressource :

| Action   | URL                               | Action effectuée                           |
| :-     : | :                             :   | :                                  :       |
| `GET`    | `/api/v1/folders/<id>/documents/` | [Contenu du dossier `id`][folders_content] |
| `POST`   | `/api/v1/folders/<id>/documents/` | N/A                                        |
| `PUT`    | `/api/v1/folders/<id>/documents/` | N/A                                        |
| `DELETE` | `/api/v1/folders/<id>/documents/` | N/A                                        |


<!-- links -->

[folders_collection]: #httpapi-ref:b3f83d12-4ea7-44e2-8509-1145d05003d6
[folders_content]: #httpapi-ref:f3e3869b-4dcf-40ee-9733-3f4b57e2386f
