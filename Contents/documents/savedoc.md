# Modification d'un document  {#httpapi-ref:db2cb01a-7325-4f78-8cec-ceac9858caf2}

## URL {#httpapi-ref:bf50f601-f8ba-432d-91b1-e39f53fcf977}

    PUT /api/v1/documents/<id>

Modification de la dernière révision document ayant l'identifiant `<documentId>`. 

L'extension ".json" peut être ajoutée pour expliciter le format de sortie.

Exemple :

    PUT /api/v1/documents/1234.json


**Attention** : L'identifiant du document peut être son nom logique, son identifiant numérique.
L'identifiant numérique peut référencer n'importe quelle révision du document. 
Dans tous les cas, la modification porte sur la dernière révision du document.

## Content {#httpapi-ref:16f7728f-6635-4503-b762-99229fd48558}

### Format JSON {#httpapi-ref:ecb81339-565d-4c78-860f-a4b1b64f8984}

Le contenu de la requête doit contenir une donnée JSON avec la liste des attributs modifiés.

    [javascript]
    {
        "document": {
            "attributes": {
                "attrid": {
                    "value": "<value>"
                }
            }
        }
    }

Le type de la requête est `application/json`.

Exemple :

    [javascript]
    {
        "document": {
            "attributes": {
                "my_title": {
                    "value": "Hello world"
                }
            }
        }
    }


Note : Toute donnée additionnelle sera ignorée.

### Format urlEncoded {#httpapi-ref:215c250f-7ddb-429b-b692-7083c5bc8bc1}

Le contenu de la requête contient la liste des valeurs d'attributs à enregistrer.
Chaque variable (PUT) est le nom de l'attribut (casse insensible).

Le type de la requête est `application/x-www-form-urlencoded`.

Note : Ce format peut être utilisé directement depuis un formulaire HTML.

Cette forme permet aussi d'enregistrer des fichiers dans le document.

## Structure de retour {#httpapi-ref:4919fce0-5933-4701-9d0c-d8f61fa089f3}

Le retour est une donnée JSON.

### En cas de réussite : {#httpapi-ref:579e3219-f71f-4c20-a0a7-38a0c938dd49}

La partie `data` contient 3 champs :

1.  `document.uri` : uri d'accès à la ressource modifiée
1.  `document.properties` : liste des valeurs des propriétés
1.  `document.attributes` : liste des valeurs des attributs
1.  `changes` : liste des valeurs modifiées

Exemple :

    [javascript]
    {"success" :             true,
        "messages" :         [],
        "data" :             {
            "document" : {
                "uri" :        "api\/v1\/documents\/1057.json",
                "properties" : {
                    "title" :     "Hello World",
                    [...]
                },
                "attributes" : {
                    "my_title" :  {"value" : "Hello World", "displayValue" : "Hello World"},
                    [...]
                }
            }
        },
        "exceptionMessage" : ""
    }

La liste des propriétés est documentée dans la [documentation de format collection][properties].

### En cas d'échec {#httpapi-ref:dd6d10d7-7654-446f-80af-34c51b424291}

Les raisons d'échecs spécifiques à cette requête sont 

|                          Raison                         | Status HTTP | Error Code |
| ------------------------------------------------------- | ----------- | ---------- |
| Privilège insuffisant pour modifier le document         |         403 | API0201    |
| Tentative de modification échouée                       |         500 | API0212    |
| Impossible de modifier l'attribut                       |         500 | API0211    |
| Le document est une famille                             |         403 | API0109    |

NB : On ne peut pas modifier les documents de familles.

Exemple : 

Cas d'erreur de privilège

    [javascript]
    {"success" :             false,
        "messages" :         [
            {
                "type" :        "error",
                "contentText" : "Update forbidden",
                "contentHtml" : "",
                "code" :        "API0201",
                "uri" :         "",
                "data" :        null
            }
        ],
        "data" :             null,
        "exceptionMessage" : "Document \"1057\" access deny : Pas de privil\u00e8ge edit pour le document Hello world [1057]"
    }

## Autres URL d'accès {#httpapi-ref:8c761d0c-1578-443f-8572-2c3b34894330}

Vous pouvez aussi accéder à cette ressources via :

    PUT /api/v1/families/<famName>/documents/<documentId>

Modification de la dernière révision du document de la famille `<famName>` ayant
l'identifiant `<documentId>`.

<span class="flag inline nota-bene"></span> La différence entre les collection
`families` et `documents` est que pour la collection
`families/<famName>/documents/` l'identifiant doit être dans la famille indiquée
pour être retourné sinon une erreur 404 (ressource non trouvée) est retournée.


[properties]: http://docs.anakeen.com/dynacase/3.2/dynacase-doc-core-reference/website/book/core-ref:74ce9ce4-8e4e-42ee-a0df-415eb6897a81.html#core-ref:9ebcbfd6-d094-45ee-a993-9b221fb4d893