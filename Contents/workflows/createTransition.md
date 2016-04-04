# Passer une transition {#httpapi-ref:697b7714-d986-4ae5-8020-a5602cfbe7d5}
## URL   {#httpapi-ref:3ebdeef9-fb96-4c3f-9860-77a418f37aee}

    POST /api/v1/documents/<documentId>/workflows/transitions/<transitionId>

Le document `documentId` passe la transition donné `transitionId`. La
transition demandée doit exister à partir de l'état courant.

Exemple :

    POST /api/v1/documents/<documentId>/workflows/transitions/my_transmission


## Content   {#httpapi-ref:986b642a-091a-43a3-8cd9-85bf4fa1a7d9}

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


## Structure de retour   {#httpapi-ref:d4411750-3515-409e-bc3c-f872a68e37fb}

Le retour est une donnée JSON.

### En cas de réussite :   {#httpapi-ref:7e773824-c340-4e33-a154-15e6901aa362}

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
                "contentText": "Avertissement : no mail template",
                "code": "WORKFLOW_TRANSITION"
            },
            {
                "type": "notice",
                "contentText": "13:48 - Bulle changement d'état vers Transmis"
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



### En cas d'échec   {#httpapi-ref:07de3437-cb10-4c2a-8047-31edeeeea2fd}

Les raisons d'échecs spécifiques à cette requête sont 


|                            Raison                            |       Status HTTP        | Error Code |
| ------------------------------------------------------------ | ------------------------ | ---------- |
| Échec de la transition (droit, m0, m1)                       | 403 Forbidden            | CRUD0230   |
| Pas de cycle de vie associé                                  | 404 No workflow detected | CRUD0227   |
| Transition invalide (non applicable depuis l'étape courante) | 404 Invalid transition   | CRUD0235   |
|                                                              |                          |            |

Exemple : 

    [javascript]
      {
        "success": false,
        "messages": [
            {
                "type": "error",
                "contentText": "Destination state is not defined for workflow \"Défaut demande adoption\" (1090)",
                "code": "CRUD0235"
            },
            {
                "type": "message",
                "contentText": "You can consult ?app=HTTPAPI_V1 to have info on the API",
                "contentHtml": "You can consult <a href=\"?app=HTTPAPI_V1\">the REST page</a> to have info on the API"
            }
        ],
        "data": null,
        "exceptionMessage": "Destination state is not defined for workflow \"Défaut demande adoption\" (1090)"
    }

