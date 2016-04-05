# Récupérer la liste des transitions {#httpapi-ref:a91dc2b7-3248-452a-b51e-3f660d7d3cf2}
## URL   {#httpapi-ref:665eef6e-57e7-4fe8-bcf2-60181798ea09}


    GET /api/v1/documents/<documentId>/workflows/transitions/

Récupération de toutes les transitions du document `documentId`.


Exemple :

    GET /api/v1/documents/61120/workflows/transitions/



## Content   {#httpapi-ref:f8e43000-eb22-4335-bb72-209dd77efc5a}

Le contenu de la requête est vide.

## Structure de retour   {#httpapi-ref:66df5b76-2deb-40e3-b33b-7ef62f8c0bbd}

Le retour est une donnée JSON.

### En cas de réussite :   {#httpapi-ref:60ed408f-3e8c-4f01-8e1e-de04f54b8a71}

La partie `data` contient :

1.  `uri` : URI préférentielle d'accès à la ressource;
1.  `transitions` : Liste de transition
    1.  `uri` : identifiant de l'état de départ
    1.  `label` : libellé de la transition (localisé en fonction de la langue de l'utilisateur)
    1.  `valid` : indique si la transition existe depuis l'étape courante  
    Cela ne vérifie pas si l'utilisateur courant peut passer la transition

Exemple :

    [javascript]
    {
        "success": true,
        "messages": [],
        "data": {
            "uri": "./api/v1/documents/61120/workflows/transitions/",
            "transitions": [
                {
                    "uri": "./api/v1/documents/61120/workflows/transitions/my_Ttransmited",
                    "label": "Transmettre le dossier",
                    "valid": true
                },
                {
                    "uri": "./api/v1/documents/61120/workflows/transitions/my_Taccepted",
                    "label": "Accepter le dossier",
                    "valid": false
                },
                {
                    "uri": "./api/v1/documents/61120/workflows/transitions/my_Trefused",
                    "label": "Refuser le dossier",
                    "valid": false
                },
                {
                    "uri": "./api/v1/documents/61120/workflows/transitions/my_Trealised",
                    "label": "Fin de traitement",
                    "valid": false
                },
                {
                    "uri": "./api/v1/documents/61120/workflows/transitions/my_Tretry",
                    "label": "À corriger",
                    "valid": false
                }
            ]
        }
    }


### En cas d'échec   {#httpapi-ref:4fe84ef0-fc99-44f3-8650-5d8f07ea1e08}

Les raisons d'échecs spécifiques à cette requête sont 

|            Raison           |       Status HTTP        | Error Code |
| --------------------------- | ------------------------ | ---------- |
| Pas de cycle de vie associé | 404 No workflow detected | CRUD0227   |

Exemple : 


    [javascript]
    {
        "success": false,
        "messages": [
            {
                "type": "error",
                "contentText": "No associated workflow for document \"9567\"",
                "code": "CRUD0227"
            }
        ],
        "data": null,
        "exceptionMessage": "No associated workflow for document \"9567\""
    }