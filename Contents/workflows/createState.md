# Changer d'état {#httpapi-ref:f3d33034-af23-48c6-b535-e609266e5bc5}
## URL   {#httpapi-ref:813c71bc-ea7c-409f-8aa6-bcf30d4562f5}

    POST /api/v1/documents/<documentId>/workflows/states/<stateid>

Le document `documentId` passe dans le nouvel état donné `stateId`. La
transition liée à ce changement d'état est exécutée.

Note : Seul l'utilisateur "admin", peut changer l'état d'un document
lorsqu'aucune transition n'est valide.

Exemple :

    POST /api/v1/documents/<documentId>/workflows/states/<stateid>


## Content   {#httpapi-ref:3c3b8a7f-5a04-49ea-865d-52843c4cfa83}

Le contenu est une structure JSON qui comprend les informations suivantes :

1.  `comment` : Texte à insérer dans l'historique
1.  `parameters` : Liste des valeurs des paramètres indexés par leur identifiant

Exemple :

    [javascript]
    {
        "comment" : "Mon commentaire de transition"
        "parameters": {
            "my_referencedate": "2015-06-23",
            "my_validation": "yes"
        }
    }

## Structure de retour   {#httpapi-ref:ab04da0d-9c5a-4153-9754-98b76bce43a0}

Le retour est une donnée JSON.

### En cas de réussite :   {#httpapi-ref:ff5a6973-46de-4c99-b3fa-a77f986d8b21}

La partie `data` contient :

1.  `uri` : URI préférentielle d'accès à la ressource;
1.  `state` : Nouvel état du document
    1.  `id` : identifiant de l'état,
    1.  `isCurrentState` : indique si cet état est l'étape courante,
    1.  `label` : intitulé de l'état (localisé en fonction de la langue de l'utilisateur) 
    1.  `activity` : intitulé de l'activité (localisé en fonction de la langue de l'utilisateur) 
    1.  `displayValue` : intitulé calculé en fonction des valeurs de "activity" et "label"
    1.  `color` : code couleur (#RRGGBB) associé à l'état

Exemple :

    [javascript]
    {
        "success": true,
        "messages": [
            {
                "type": "warning",
                "contentText": "Avertissement : Pas de modèle de courriel",
                "code": "WORKFLOW_TRANSITION"
            },
            {
                "type": "notice",
                "contentText": "12:00 - Bulle changement d'état vers Transmis"
            }
        ],
        "data": {
            "state": {
                "id": "my_transmited",
                "isCurrentState": true,
                "label": "Transmis",
                "activity": "Vérification de l'adoption",
                "displayValue": "Vérification de l'adoption",
                "color": "#A8E5FF"
            }
        }
    }


### En cas d'échec   {#httpapi-ref:558861bc-14e5-4849-816b-cdbfe9a892ce}

Les raisons d'échecs spécifiques à cette requête sont 


|                 Raison                 |       Status HTTP        | Error Code |
| -------------------------------------- | ------------------------ | ---------- |
| Échec de la transition (droit, m0, m1) | 403 Forbidden            | CRUD0230   |
| Pas de cycle de vie associé            | 404 No workflow detected | CRUD0227   |
|                                        |                          |            |

Exemple : 

    [javascript]
    {
        "success": false,
        "messages": [
            {
                "type": "error",
                "contentText": "Erreur : Pas d'adresse pour le contrôleur",
                "code": "CRUD0230"
            }
        ],
        "data": null,
        "exceptionMessage": "Cannot use transition \"Erreur : Pas d'adresse pour le contrôleur\" "
    }

