# Consultation des messages d'historique {#httpapi-ref:9ae75dac-8eb5-4fa9-a608-7313b90fe33c}

## URL  {#httpapi-ref:24535176-2898-4d2b-8d54-57caf7efc7f1}

    GET /api/v1/documents/<documentId>/history/

Récupération de l'historique du document ayant
l'identifiant `<documentId>`.

## Content  {#httpapi-ref:a3c6ab8a-a46f-4afc-8f9d-7066d7d141c1}

Le contenu de la requête est vide.

## Structure de retour  {#httpapi-ref:e8d2e341-2cbe-4009-ad97-8a0b07245b09}

Le retour est une donnée JSON.

### En cas de réussite :  {#httpapi-ref:0c60cc1d-4fe7-45fa-b655-980c6c673167}
    
La partie `data` contient les éléments suivants :


|        Clef        |               Description                |                                              Exemple                                               |
| ------------------ | ---------------------------------------- | -------------------------------------------------------------------------------------------------- |
| history            | Liste des caractéristiques des révisions | [{documentId:13514, uri:api/v1/documents/7120/revisions/3, title:Document de test, fixed:false,…}] |
| requestParameters: | Filtre utilisé                           | {slice:-1, offset:0, revision:3}                                                                   |
|                    |                                          |                                                                                                    |

Chaque élément de l'historique `history` contient la structure suivante :

*  Un champ `properties` qui contient :

|        Clef       |                           Description                            |               Exemple               |
| ----------------- | ---------------------------------------------------------------- | ----------------------------------- |
| id                | Identifiant numérique du document                                | 13514                               |
| revision:         | Numéro de la révision                                            | 3                                   |
| revisionDate:     | Date de la révision                                              | "2014-10-16 13:58:56"               |
| state.reference : | État de la révision                                              | "zoo_realised"                      |
| state.color:      | Couleur de l'état                                                | "#8870FF"                           |
| state.stateLabel: | Libelle de l'état                                                | "Réalisé"                           |
| state.activity:   | Libelle de l'activité                                            | "Contrôle des vis-à-vis"            |
| status:           | Indique si le status de la révision "fixed" , "alive", "deleted" | "fixed"                             |
| title:            | Titre de la révision                                             | "Alberta"                           |
| uri:              | Uri d'accès à la révision                                        | "api/v1/documents/7120/revisions/3" |
| version:          | Version de la révision                                           | null                                |


*  Un champ `messages` qui contient une liste de messages :

Chaque message contient la structure suivante :

|   Clef   |                       Description                        |                 Exemple                  |
| -------- | -------------------------------------------------------- | ---------------------------------------- |
| code     | Code du message                                          | "MODIFY"                                 |
| comment: | Message textuel                                          | "modification Pays, Propriétaire actuel" |
| date:    | Date d'enregistrement du message                         | "2014-10-16 13:58:56"                    |
| level:   | Niveau du message                                        | 4                                        |
| uid:     | Identifiant de l'utilisateur ayant enregistré le message | "2014-10-16 13:58:56"                    |
| uname:   | Nom de l'utilisateur ayant enregistré le message         | "Jean Martin"                            |
|          |                                                          |                                          |

Les niveaux (`level`) correspondent aux criticités suivantes :

| Criticité |            Description             |
| --------- | ---------------------------------- |
| "notice"  | Message d'information mineur       |
| "info"    | Message d'information (par défaut) |
| "message" | Message important                  |
| "warning" | Avertissement                      |
| "error"   | Erreur                             |
|           |                                    |

Exemple :


    GET api/v1/document/7120/history/

    [javascript]
    {
        "success": true,
        "messages": [],
        "data": {
            "uri": "http://www.example.net/api/v1/documents/7120/history/",
            "requestParameters": {
                "slice": -1,
                "offset": 0,
                "revision": -1
            },
            "history": [
                {
                    "uri": "http://www.example.net/api/v1/documents/7120/revisions/2.json",
                    "properties": {
                        "id": 7120,
                        "title": "Test d'un alligator",
                        "status": "alive",
                        "revision": 2,
                        "owner": {
                            "id": "1",
                            "title": "O'reilly Master"
                        },
                        "state": {
                            "reference": "zoo_transmited",
                            "stateLabel": "Transmis",
                            "activity": "Vérification d'aptitude",
                            "color": "#A8E5FF"
                        },
                        "version": "Deuxième",
                        "revisionDate": "2014-12-01 17:00:37"
                    },
                    "messages": [
                        {
                            "uid": 1,
                            "uname": "O'reilly Master",
                            "date": "2014-12-01 17:00:37",
                            "level": "info",
                            "code": null,
                            "comment": "no mail template"
                        }
                    ]
                },
                {
                    "uri": "http://www.example.net/api/v1/documents/7120/revisions/1.json",
                    "properties": {
                        "id": 7120,
                        "title": "Test primaire",
                        "status": "fixed",
                        "revision": 1,
                        "owner": {
                            "id": "1",
                            "title": "O'reilly Master"
                        },
                        "state": {
                            "reference": "zoo_accepted",
                            "stateLabel": "Accepté",
                            "activity": "Préparation de l'accueil",
                            "color": "#66FF7A"
                        },
                        "version": null,
                        "revisionDate": "2014-10-16 13:57:16"
                    },
                    "messages": [
                        {
                            "uid": 1,
                            "uname": "O'reilly Master",
                            "date": "2014-10-16 13:57:16",
                            "level": "message",
                            "code": "REVISION",
                            "comment": "changement d'état de Transmis vers Accepté"
                        },
                        {
                            "uid": 1,
                            "uname": "O'reilly Master",
                            "date": "2014-10-16 13:57:04",
                            "level": "info",
                            "code": null,
                            "comment": "no mail template"
                        }
                    ]
                },
                {
                    "uri": "http://www.example.net/api/v1/documents/7120/revisions/0.json",
                    "properties": {
                        "id": 7120,
                        "title": "Test primaire",
                        "status": "fixed",
                        "revision": 0,
                        "owner": {
                            "id": "1",
                            "title": "O'reilly Master"
                        },
                        "state": {
                            "reference": "zoo_transmited",
                            "stateLabel": "Transmis",
                            "activity": "Vérification de l'adoption",
                            "color": "#A8E5FF"
                        },
                        "version": null,
                        "revisionDate": "2014-10-16 13:57:04"
                    },
                    "messages": [
                        {
                            "uid": 1,
                            "uname": "O'reilly Master",
                            "date": "2014-10-16 13:57:04",
                            "level": "message",
                            "code": "REVISION",
                            "comment": "changement d'état de Initialisé vers Transmis"
                        },...
                    ]
                }
            ]
        }
    }

### En cas d'échec  {#httpapi-ref:b32007bd-500e-46ca-8734-85af9be2ccc0}

Les raisons d'échecs spécifiques à cette requête sont 

|                     Raison                     | Status HTTP | Error Code |
| ---------------------------------------------- | ----------- | ---------- |
| Famille non trouvée                            |         404 | API0220    |
| Document non trouvé                            |         404 | API0200    |

Exemple : 

Cas d'erreur en cas de document non trouvé

    [javascript]
    {
        "success": false,
        "messages": [
            {
                "type": "error",
                "contentText": "Document \"3802\" not found",
                "contentHtml": "",
                "code": "API0200",
                "uri": "",
                "data": null
            },
            {
                "type": "message",
                "contentText": "You can consult http://dynacase.dev:8081/index.php?app=HTTPAPI_V1 to have info on the API",
                "contentHtml": "You can consult <a href=\"http://dynacase.dev:8081/index.php?app=HTTPAPI_V1\">the REST page</a> to have info on the API",
                "code": "",
                "uri": "",
                "data": null
            }
        ],
        "data": null,
        "exceptionMessage": "Document \"3802\" not found"
    }


## Filtres  {#httpapi-ref:a882873e-dcb1-4866-a7dc-2de45a15aa00}

### Slice {#httpapi-ref:83a97e3c-cd8b-4fdf-8332-c35f54674cde}

Cette option indique le nombre de révision maximum à retourner. 

Les révisions sont ordonnées par numéro décroissant, de la plus récente à la
plus ancienne.

Exemple : les 10 dernières révisions

    GET api/v1/document/1234/history/?slice=10

### Offset {#httpapi-ref:cf027228-a908-419e-8e21-13c246281ecc}

Index à partir duquel, les révisions sont retournées.

Exemple: Les révisions de 7 à 10.

    GET api/v1/document/1234/history/?slice=2&offset=7

### Révision {#httpapi-ref:64f4d573-1b08-4bc7-aad0-d7183d488d42}

Cette option permet de ne retourner l'historique que d'une révision précise.

Exemple : Retourner la révision 3 

    GET api/v1/document/1234/history/?revision=3

La première révision porte le n°0.

## Autres URL d'accès {#httpapi-ref:8501b952-4505-4442-b266-c33414020bec}

Vous pouvez aussi accéder à cette ressources via :

    GET /api/v1/families/<famName>/documents/<documentId>/history/

Récupération de l'historique de la dernière révision du document de la famille
`<famName>` ayant l'identifiant `<documentId>`.

<span class="flag inline nota-bene"></span> La différence entre les collection
`families` et `documents` est que pour la collection
`/api/v1/families/<famName>/documents/<documentId>/history/` l'identifiant doit
être dans la famille indiquée pour être retourné sinon une erreur 404 (ressource
non trouvée) est retournée.

<span class="flag inline nota-bene"></span> L'historique d'un document 
"supprimé" peut être récupéré via l'url 

    GET /api/v1/trash/<documentId>/history/
