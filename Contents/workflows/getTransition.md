# Récupérer les caractéristiques d'une transition {#httpapi-ref:d370f800-2a17-4589-90ae-d505b5f71c71}
## URL   {#httpapi-ref:5011229b-0968-4f86-9cf0-c04eeb3e96e7}


    GET /api/v1/documents/<documentId>/workflows/transitions/<transitionId>

Récupération de la transition `transitionId` du document `documentId`.

Exemple :

    GET /api/v1/documents/my_document/workflows/transitions/my_publication


## Content   {#httpapi-ref:0f4daee2-762f-4604-b1d9-924e5a1678d5}

Le contenu de la requête est vide.

## Structure de retour   {#httpapi-ref:3b799e8c-60fb-47ac-8f06-66b23524fec9}

Le retour est une donnée JSON.

### En cas de réussite :   {#httpapi-ref:a5e9587c-b0b3-4cde-ad4d-fdb76e91092b}

La partie `data` contient :

1.  `uri` : URI préférentielle d'accès à la ressource;
1.  `transition` : Caractéristique de la transition
    1.  `id` : identifiant de la transition
    1.  `beginState` : étape de départ de la transition
        1.  `id` : identifiant de l'état de départ
        1.  `isCurrentState` : indique si c'est l'état courant du document
        1.  `label` : libellé de l'état (localisé en fonction de la langue de l'utilisateur)
        1.  `activity` : libellé de l'activité (localisé en fonction de la langue de l'utilisateur)
        1.  `displayValue` : intitulé calculé en fonction des valeurs de "activity" et "label"
        1.  `color` : code couleur (#RRGGBB) associé à l'état
    1.  `endState` : étape d'arrivée de la transition
        1.  `id` : identifiant de l'état d'arrivée
        1.  `isCurrentState` : indique si c'est l'état courant du document
        1.  `label` : libellé de l'état (localisé en fonction de la langue de l'utilisateur)
        1.  `activity` : libellé de l'activité (localisé en fonction de la langue de l'utilisateur)
        1.  `displayValue` : intitulé calculé en fonction des valeurs de "activity" et "label"
        1.  `color` : code couleur (#RRGGBB) associé à l'état
    1.  `label` : libellé de la transition (localisé en fonction de la langue de l'utilisateur)
    1.  `askComment` : (bool) indique si un commentaire peut être associé à la transition
    1.  `askAttributes` : liste des paramètres de transition
        1.  `id` : identifiant du paramètre (provenant de la famille du cycle de vie)
        1.  `visibility` : visibilité du paramètre
        1.  `label` : libellé du paramètre
        1.  `type` : type du paramètre
        1.  `logicalOrder` : ordre (non affecté)
        1.  `multiple` : indique si le type est multivalué
        1.  `options` : options de l'attribut
        1.  `needed` : indique si la valeur est obligatoire

Exemple :

    [javascript]
    {
        "success": true,
        "messages": [],
        "data": {
            "uri": "./api/v1/documents/61120/workflows/transitions/my_Ttransmited",
            "transition": {
                "id": "my_Ttransmited",
                "beginState": {
                    "id": "my_initialised",
                    "isCurrentState": true,
                    "label": "Initialisé",
                    "activity": "Rédaction de la demande",
                    "displayValue": "Rédaction de la demande",
                    "color": "#FFE991"
                },
                "endState": {
                    "id": "my_transmited",
                    "isCurrentState": false,
                    "label": "Transmis",
                    "activity": "Vérification de l'adoption",
                    "displayValue": "Vérification de l'adoption",
                    "color": "#A8E5FF"
                },
                "label": "Transmettre le dossier",
                "askComment": false,
                "askAttributes": [
                    {
                        "id": "wan_date",
                        "visibility": "W",
                        "label": "date de début",
                        "type": "date",
                        "logicalOrder": 0,
                        "multiple": false,
                        "options": [],
                        "needed": false
                    },
                    {
                        "id": "wad_mailsecure",
                        "visibility": "W",
                        "label": "Espèce protégée",
                        "type": "docid",
                        "logicalOrder": 0,
                        "multiple": false,
                        "options": [],
                        "needed": false
                    },
                    {
                        "id": "wad_file",
                        "visibility": "W",
                        "label": "Fichier",
                        "type": "file",
                        "logicalOrder": 0,
                        "multiple": false,
                        "options": [],
                        "needed": false
                    }
                ]
            }
        }
    }


### En cas d'échec   {#httpapi-ref:fe2f90b9-f71b-4a64-93c3-7f8bc1a24b6f}
Les raisons d'échecs spécifiques à cette requête sont 


|            Raison           |       Status HTTP        | Error Code |
| --------------------------- | ------------------------ | ---------- |
| Transition inconnue         | 404 Transition not found | CRUD0229   |
| Pas de cycle de vie associé | 404 No workflow detected | CRUD0227   |

Exemple : 

    [javascript]
    {
        "success": false,
        "messages": [
            {
                "type": "error",
                "contentText": "transition \"foo\" is not available for  workflow \"My workflow\" (1090)",
                "code": "CRUD0229"
            }
        ],
        "data": null,
        "exceptionMessage": "transition \"foo\" is not available for  workflow \"My workflow\" (1090)"
    }

