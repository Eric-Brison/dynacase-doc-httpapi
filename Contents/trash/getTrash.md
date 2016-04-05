# Consultation d'un document supprimé  {#httpapi-ref:52be10c1-9f46-456b-a22f-24909386567f}

## URL {#httpapi-ref:074c3c3f-e25e-418b-9030-a6d77f986731}

Récupération d'un document du trash.

    GET /api/v1/trash/<documentId>

Récupération de la dernière révision du document supprimé ayant pour id `<documentId>`.

L'extension ".json" peut être ajoutée pour expliciter le format de sortie.

Exemple :

    GET /api/v1/trash/1234.json

L'identifiant du document peut être son nom logique, son identifiant numérique.

## Content {#httpapi-ref:4f2315f1-9696-4843-804d-ff8f135193f0}

Le contenu de la requête est vide.

## Structure de retour {#httpapi-ref:1e75875d-514f-477b-84ac-f9e45027aa16}

Le retour est une donnée JSON.

### En cas de réussite : {#httpapi-ref:1a845c54-6c3a-4d61-9996-43a5979b7819}

La partie `data` contient 3 champs :

1.  `document.uri` : uri d'accès à la ressource modifiée
1.  `document.properties` : liste des valeurs des propriétés
1.  `document.attributes` : liste des valeurs des attributs

Les attributs en visibilité "I" ne sont pas retournés. 

Exemple :

    [javascript]
    {"success" :             true,
        "messages" :         [],
        "data" :             {
            "document" : {
                "uri" :        "api\/v1\/documents\/1057.json",
                "properties" : {
                    "title" :     "Aristote le Stagirite",
                    "name" :      null,
                    "icon" :      "resizeimg.php?img=Images%2Fdoc.png&size=32"
                    [...]
                },
                "attributes" : {
                    "my_title" :            {"value" : "[123", "displayValue" : "[123"},
                    "my_level" :          {"value" : 34, "displayValue" : "34"}
                    [...]
                }
            }
        }
    }

### En cas d'échec {#httpapi-ref:0de77a4f-f138-432f-b8a4-c3ec8d5b982c}

Les raisons d'échecs spécifiques à cette requête sont 

|                     Raison                     | Status HTTP | Error Code |
| ---------------------------------------------- | ----------- | ---------- |
| Document non trouvé                            |         404 | API0200    |
| Privilège insuffisant pour accéder au document |         403 | API0201    |
| Document supprimé                              |         404 | API0219    |
| Propriété demandée inexistante                 |         400 | API0202    |
| Attribut demandé inexistant                    |         400 | API0218    |

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

## Résultat partiel {#httpapi-ref:e641eb3e-8540-47bf-b124-ba1be648d39f}

Le document peut être retourné avec plus ou moins d'information.

* GET /api/v1/trash/1234.json?fields=document.properties
* GET /api/v1/trash/1234.json?fields=document.properties.id,document.properties.title,document.attributes
* GET /api/v1/trash/1234.json?fields=document.properties.id,document.properties.title,document.attributes.my_exemple
* GET /api/v1/trash/1234.json?fields=document.properties.id,document.properties.title,document.attributes,family.structure

Par défaut : `fields=document.properties,document.attributes`

|            fields            |                        Signification                         |                      Remarques                      |
| ---------------------------- | ------------------------------------------------------------ | --------------------------------------------------- |
| `document.properties`        | Récupère les propriétés données par défaut                   | "id", "title", "icon", "initid", "name", "revision" |
| `document.properties.<prop>` | Récupère la propriété indiquée                               |                                                     |
| `document.attributes`        | Récupère les valeurs et les valeurs affichable des attributs |                                                     |
| `document.attributes.<id>`   | Récupère la valeur d'un attribut particulier                 |                                                     |
| `document.family.structure`  | Récupère la structure de la famille                          |                                                     |

La liste des propriétés est documentée dans la [documentation de format collection][properties].

[properties]: http://docs.anakeen.com/dynacase/3.2/dynacase-doc-core-reference/website/book/core-ref:74ce9ce4-8e4e-42ee-a0df-415eb6897a81.html#core-ref:9ebcbfd6-d094-45ee-a993-9b221fb4d893
