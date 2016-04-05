# Consultation de la liste des documents supprimés {#httpapi-ref:4052b9db-d36c-4535-809f-1fad107e8270}

## URL canonique {#httpapi-ref:d24f9500-e99c-4d6b-bb52-3565de25977f}

    GET /api/v1/trash/

Récupération de la liste des documents supprimés de la plateforme.

## Content {#httpapi-ref:811d1927-0661-4321-b59d-ba1f90b899c6}

Le contenu de la requête est vide.

## Structure de retour {#httpapi-ref:d21504de-f705-4af9-a3f6-b545448ae79f}

Le retour est une donnée JSON.

### En cas de réussite : {#httpapi-ref:7286851d-1e3e-4280-a2de-56422f6ff270}

La partie `data` contient :

1.  `requestParameters` : contient un résumé des paramètres de la requête en cours (pagination et orderBy),
1.  `uri` : URI d'accès de la collection,
1.  `properties` : Le titre de la recherche "The trash" (traduit)
1.  `documents` : un tableau de document (sous la même forme que les documents unitaires)

Chaque document est un objet contenant les entrées suivantes :

1.  `properties` : liste des propriétés du document,
1.  `attributes` : liste des attributs du document (facultatif),
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
                "length": 10,
                "orderBy": "title asc, id desc"
            },
            "uri": "/api/v1/trash/",
            "properties": {
                "title": "The trash"
            },
            "documents": [
                {
                    "properties": {
                        "id": 1003,
                        "title": "accord",
                        "icon": "resizeimg.php?img=Images%2Fwask.png&size=24",
                        "initid": 1003,
                        "name": "WASK",
                        "revision": 0
                    },
                    "uri": "/api/v1/trash/1003.json"
                },
                [...]
            ]
        }
    }

<span class="flag inline nota-bene"></span> Les valeurs retournées correspondent aux valeurs de la vue de consultation
par défaut.

### En cas d'échec {#httpapi-ref:4983cb3f-3bea-4f42-ba00-6e313ac5ddf8}

Les raisons d'échecs spécifiques à cette requête sont 

|                     Raison                     | Status HTTP | Error Code |
| ---------------------------------------------- | ----------- | ---------- |
| Sens de l'orderBy inconnu                      |         400 | CRUD0501   |
| Attribut ou propriété d'orderBy invalide       |         400 | CRUD0502   |

## Résultat partiel {#httpapi-ref:d0ea75fc-4700-45b5-b525-19829f50d9cc}

### Pagination et tri {#httpapi-ref:7dd8b328-7372-4d4d-875c-b4b11e02e6ae}

La liste des documents peut être paginée et ordonnée.

Les mots clefs GET sont les suivants :

* **orderBy** : `<attribut|propriété>:<asc:desc>`
  * indique dans quel sens la collection doit être triée, on peut mettre plusieurs valeurs séparées par des ,
  * valeur par défaut : `title:asc`,
* **slice** : 
  * il indique le nombre maximum de documents à retourner, sa valeur est un entier ou le mot clef `all`,
  * sa valeur par défaut est celle du paramètre applicatif `COLLECTION_DEFAULT_SLICE` de l'application `HTTPAPI_V1` (10),
* **offset** :
  * indique le nombre d'éléments à exclure avant de retourner la collection.
  * valeur par défaut : 0

<span class="flag inline nota-bene"></span> Les paramètres appliqués sont résumés dans le retour de la collection 
`requestParameter`.

Exemple : 

* GET `/trash/?orderBy=title:asc,id:desc&slice=100&offset=20`

### Informations retournées {#httpapi-ref:e8f5e238-092b-403b-b37b-9aabe8e2637f}

Les documents peuvent être retournés avec plus ou moins d'information.

* GET /trash/?fields=document.properties
* GET /trash/?fields=document.properties.id,document.properties.title

Par défaut : `fields=document.properties`

|           fields                   |                        Signification                         |                                                           Remarques                                                           |
| ---------------------------------- | ------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------- |
| `document.properties`              | Récupère l'ensemble des propriétés par défaut                | "id", "title", "icon", "initid", "name"                                                                                       |
| `document.properties.all`          | Récupère toutes les propriétés                               |                                                                                                                               |
| `document.properties.<prop>`       | Récupère la propriété indiquée                               |                                                                                                                               |
| `document.attributes.my_attribute` | Ajoute l'attribut my_attribute                               |  si l'attribut n'existe pas dans un des documents il est retourné vide                                                        |

La liste des propriétés est documentée dans la [documentation de format collection][properties].

## Cache {#httpapi-ref:478d16f8-cf1f-4195-9a3e-02d1d64889c6}

La collection n'a pas de cache.

[properties]: http://docs.anakeen.com/dynacase/3.2/dynacase-doc-core-reference/website/book/core-ref:74ce9ce4-8e4e-42ee-a0df-415eb6897a81.html#core-ref:9ebcbfd6-d094-45ee-a993-9b221fb4d893