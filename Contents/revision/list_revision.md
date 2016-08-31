# Consultation de la liste des révisions d'un document {#httpapi-ref:2dd5afbe-1d3d-4830-8241-c93077d88430}

## URL {#httpapi-ref:4665a788-c584-4805-b638-3b40c33c281d}

    GET /api/v1/documents/<documentId>/revisions/

Récupération de la liste des révisions du document `<documentId>`.

L'identifiant du document peut être son nom logique, son identifiant numérique.

## Content {#httpapi-ref:91384551-3ae9-4d97-8c70-0c81ab64bdfa}

Le contenu de la requête est vide.

## Structure de retour {#httpapi-ref:d9f3ca39-11e2-4131-966a-034352f34ca7}

Le retour est une donnée JSON.

### En cas de réussite : {#httpapi-ref:e5b9222e-c8ef-433b-91fc-45efc59575a3}

La partie `data` contient :

1.  `requestParameters` : contient un résumé des paramètres de la requête en cours (pagination et orderBy),
1.  `uri` : URI d'accès de la collection,
1.  `revisions` : un tableau de révision (sous la même forme que les documents unitaires)

Chaque révision est un objet contenant les entrées suivantes :

1.  `properties` : liste des propriétés de la révision,
1.  `attributes` : liste des attributs du document révisé (facultatif),
1.  `uri` : URI d'accès du document.

Exemple :

    [javascript]
    {
        "success": true,
        "messages": [],
        "data": {
            "requestParameters": {
                "slice": 10,
                "offset": 0,
                "length": 2,
                "orderBy": "revision desc, id desc"
            },
            "uri": "http://www.example.net/api/v1/documents/1427/revisions/",
            "revisions": [
                {
                    "properties": {
                        "id": 13511,
                        "title": "Éléonore d'aquitaine",
                        "icon": "api/v1/images/assets/sizes/24x24c/article.png",
                        "initid": 1427,
                        "name": null,
                        "status": "alive",
                        "revision": 1
                    },
                    "attributes": {
                        "an_nom": {
                            "value": "Éléonore d'aquitaine",
                            "displayValue": "Éléonore d'aquitaine"
                        },...
                    },
                    "uri": "http://www.example.net/api/v1/documents/1427/revisions/1.json"
                },
                {
                    "properties": {
                        "id": 1427,
                        "title": "Éléonore",
                        "icon": "api/v1/images/assets/sizes/24x24c/article.png",
                        "initid": 1427,
                        "name": null,
                        "status": "fixed",
                        "revision": 0
                    },
                    "attributes": {
                        "an_nom": {
                            "value": "Éléonore",
                            "displayValue": "Éléonore"
                        },...
                    },
                    "uri": "http://www.example.net/api/v1/documents/1427/revisions/0.json"
                }
            ]
        }
    }

### En cas d'échec {#httpapi-ref:5bfa051d-e3a4-4263-8589-f392a1257e2d}

Les raisons d'échecs spécifiques à cette requête sont 

|                     Raison                     | Status HTTP | Error Code |
| ---------------------------------------------- | ----------- | ---------- |
| Document non trouvé                            |         404 | API0200    |

Exemple : 

Cas d'erreur de document non trouvé

    [javascript]
    {"success" :             false,
        "messages" :         [
            {
                "type" :        "error",
                "contentText" : "Document \"1200\" not found",
                "contentHtml" : "",
                "code" :        "API0200",
                "uri" :         "",
                "data" :        null
            }
        ],
        "data" :             null,
        "exceptionMessage" : "Document \"1200\" not found"
    }

## Cache {#httpapi-ref:8d9d9dee-4e67-4881-aa4c-b490be94505e}

Dans le cadre du [cache][cache], le `Etag` est calculé à l'aide des éléments suivants :

* identifiant de la dernière révision du document,
* date de dernière modification du document,
* identifiant de l'utilisateur,
* identifiant des droits portés sur le document (vecteur de droits),
* langue sélectionnée.

L'ensemble de ces éléments sont concaténés et ensuite le [sha1][sha1] de cette
concaténation consitue le `Etag`.


## Autres URL d'accès {#httpapi-ref:95660f6b-2551-44bd-a925-169e99332a7a}

Vous pouvez aussi accéder à cette ressources via :

    GET /api/v1/families/<famName>/documents/<documentId>/revisions/

Récupération de la liste des révisions de la dernière révision d'un document de
la famille `<famName>` ayant l'identifiant `<documentId>`.

<span class="flag inline nota-bene"></span> La différence entre les collection
`families` et `documents` est que pour la collection
`/api/v1/families/<famName>/documents/<documentId>/revisions/`
l'identifiant doit être dans la famille indiquée pour être retourné sinon une
erreur 404 (ressource non trouvée) est retournée.

<span class="flag inline nota-bene"></span> Les révisions d'un document
"supprimé" pevent être récupérés via l'url

    GET /api/v1/trash/<documentId>/revisions/

[trash]: #httpapi-ref:52be10c1-9f46-456b-a22f-24909386567
[cache]: #httpapi-ref:804f8d68-acfa-4a35-bb41-27b2a27c14dc
[sha1]: https://fr.wikipedia.org/wiki/SHA-1
