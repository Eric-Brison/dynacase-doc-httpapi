# Restauration d'un document  {#httpapi-ref:21652c32-5695-4cc0-9b71-f4a2b5f33125}

## URL {#httpapi-ref:7b6ec527-672b-47da-843c-c592cf6b410a}

    PUT /api/v1/trash/<id>

Restauration du document `<documentId>`. 

L'extension ".json" peut être ajoutée pour expliciter le format de sortie.

Exemple :

    PUT /api/v1/trash/1234.json


**Attention** : L'identifiant du document peut être son nom logique, son identifiant numérique.
L'identifiant numérique peut référencer n'importe quelle révision du document. 
Dans tous les cas, la modification porte sur la dernière révision du document.

## Content {#httpapi-ref:954e5a3f-f24c-4799-beb1-45343151a324}

### Format JSON {#httpapi-ref:018e095c-a2a2-4d7c-bf30-f410da265df9}

La demande de restauration doit être formulée de la manière suivante.

Le type de la requête est `application/json`.

    [javascript]
    {
        "document": {
            "properties": {
                "status": "alive"
            }
        }
    }

Note : Toute donnée additionnelle sera ignorée.

## Structure de retour {#httpapi-ref:297bd5ed-0508-4d69-94cc-b33407f900d0}

Le retour est une donnée JSON.

### En cas de réussite :  {#httpapi-ref:024c2ea6-e204-4241-90cc-0159a4bcb042}

La partie `data` contient 3 champs :

1.  `document.uri` : uri d'accès à la ressource modifiée
1.  `document.properties` : liste des valeurs des propriétés
1.  `document.attributes` : liste des valeurs des attributs

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

### En cas d'échec {#httpapi-ref:c2140b81-f2a8-45a8-92d1-00040c08fc6c}

Les raisons d'échecs spécifiques à cette requête sont 

|                          Raison                         | Status HTTP | Error Code |
| ------------------------------------------------------- | ----------- | ---------- |
| Impossible de lire la demande de restauration           |         500 | CRUD0208   |
| Demande de restauration mal formulée                    |         500 | CRUD0236   |
| Document non trouvé                                     |         404 | CRUD0200   |
| Document trouvé mais pas dans la poubelle               |         404 | CRUD0236   |
| Impossible de restaurer le document                     |         500 | CRUD0505   |


Exemple : 

Cas de demande de restauration mal formulée

    [javascript]
    {
        "success": false,
        "messages": [
            {
                "type": "error",
                "contentText": "The restoration must be initialized with {\"document\" : { \"properties\" : { \"status\" : \"alive\" } } }",
                "code": "CRUD0236"
            }
        ],
        "data": null,
        "exceptionMessage": "The restoration must be initialized with {\"document\" : { \"properties\" : { \"status\" : \"alive\" } } }"
    }


