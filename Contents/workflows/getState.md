# Récupérer les caractéristiques d'un état {#httpapi-ref:89142988-9b2b-42f6-af33-f68749c7af35}
## URL   {#httpapi-ref:2dc8c6b1-cf39-42d6-9953-ff5f93916870}

    GET /api/v1/documents/<documentId>/workflows/states/<stateId>

Récupération de l'état `stateId` du document `documentId`.

Exemple :

    GET /api/v1/documents/my_document/workflows/states/my_published


## Content   {#httpapi-ref:0199cabd-7a8c-4cca-88e0-fa369a96ad4f}

Le contenu de la requête est vide.

## Structure de retour   {#httpapi-ref:0405238d-6674-4bd7-b23a-50bf45c1cfcb}

Le retour est une donnée JSON.

### En cas de réussite :   {#httpapi-ref:25894f05-e83d-4188-8270-dce76fe1c8e1}

La partie `data` contient :

1.  `uri` : URI préférentielle d'accès à la ressource;
1.  `state` : Caractéristique de l'étape
    1.  `id` : identifiant de l'état,
    1.  `isCurrentState` : indique si cet état est l'étape courante,
    1.  `label` : intitulé de l'état (localisé en fonction de la langue de l'utilisateur) 
    1.  `activity` : intitulé de l'activité (localisé en fonction de la langue de l'utilisateur) 
    1.  `displayValue` : intitulé calculé en fonction des valeurs de "activity" et "label"
    1.  `color` : code couleur (#RRGGBB) associé à l'état
    1.  `transition` : transition qui emmène à cet étape ("null" si pas de transition possible)
        1.  `uri` : uri de la transition
        1.  `label` : libellé de la transition

Exemple :

    [javascript]
    {
        "success": true,
        "messages": [],
        "data": {
            "uri": "./api/v1/documents/61120/workflows/states/my_transmited",
            "state": {
                "id": "my_transmited",
                "isCurrentState": false,
                "label": "Transmis",
                "activity": "Vérification de l'adoption",
                "displayValue": "Vérification de l'adoption",
                "color": "#A8E5FF",
                "transition": {
                    "uri": "./api/v1/documents/61120/workflows/transitions/my_Ttransmited",
                    "label": "Transmettre le dossier"
                }
            }
        }
    }


### En cas d'échec   {#httpapi-ref:b76fa0f7-2877-4984-abb5-999739cdc4eb}

Les raisons d'échecs spécifiques à cette requête sont 


|            Raison           |       Status HTTP        | Error Code |
| --------------------------- | ------------------------ | ---------- |
| État inconnu                | 404 State not found      | CRUD0228   |
| Pas de cycle de vie associé | 404 No workflow detected | CRUD0227   |

Exemple : 


    [javascript]
     {
        "success": false,
        "messages": [
            {
                "type": "error",
                "contentText": "State \"foo\" is not available for  workflow \"My workflow\" (1090)",
                "code": "CRUD0228"
            }
        ],
        "data": null,
        "exceptionMessage": "State \"foo\" is not available for  workflow \"My workflow\" (1090)"
    }

