# Liste des recherches {#httpapi-ref:9b8f4a2b-3f56-4b21-a7b7-bb299b4ac7b3}

## URL canonique {#httpapi-ref:b7c4d707-d0db-40a0-a429-cd06fd3a23b2}

    GET /api/v1/searches/

Récupération de la liste des recherches accessibles.

## Content {#httpapi-ref:9cb7063c-5a36-41ee-8e9c-8d4a85b37f9d}

Le contenu de la requête est vide.

## Structure de retour {#httpapi-ref:99315351-3ddd-4a82-a3a8-af2e0937bce7}

Le retour est une donnée JSON.

### En cas de réussite : {#httpapi-ref:87cb2837-629e-40cb-8b8e-0e8a89014df8}

La partie `data` contient :

1.  `requestParameters` : contient un résumé des paramètres de la requête en cours (pagination et orderBy),
1.  `uri` : URI d'accès de la collection,
1.  `documents` : un tableau de recherche (sous la même forme que les documents unitaires)

Chaque document est un objet contenant les entrées suivantes :

1.  `properties` : liste des propriétés de la recherche,
1.  `uri` : URI de contenu de la recherche.

Exemple :

    [javascript]
    {
        "success": true,
        "messages": [],
        "data": {
            "requestParameters": {
                "slice": 10,
                "offset": 0,
                "length": 9,
                "orderBy": "title asc, id desc"
            },
            "uri": "/api/v1/searches/",
            "documents": [
                {
                    "properties": {
                        "id": 1894,
                        "title": "Les articles intéressants",
                        "icon": "api/v1/images/assets/sizes/24x24c/search.gif",
                        "initid": 1894,
                        "name": null,
                        "revision": 0
                    },
                    "uri": "/api/v1/searches/1894/documents/"
                }[...]
            ]
        }
    }


### En cas d'échec {#httpapi-ref:996ba68e-77f5-41cf-81b7-98aa8a1ec316}

Les raisons d'échecs spécifiques à cette requête sont 

|                     Raison                     | Status HTTP | Error Code |
| ---------------------------------------------- | ----------- | ---------- |
| Sens de l'orderBy inconnu                      |         400 | CRUD0501   |
| Attribut ou propriété d'orderBy invalide       |         400 | CRUD0502   |

## Résultat partiel {#httpapi-ref:0d57bb09-b1d3-4ba8-a5af-aff76a542ccf}

### Pagination et tri {#httpapi-ref:b85c060a-15c2-4bd1-a99f-ce3b7841de60}

La liste des documents peut être paginée et ordonnée.

Les mots clefs GET sont les suivants :

* **orderBy** : `<attribut|propriété>:<asc:desc>`
  * il indique dans quel sens la collection doit être triée,
  * valeur par défaut : `title:asc`,
* **slice** : 
  * il indique le nombre maximum de documents à retourner,
  * sa valeur par défaut est celle du paramètre applicatif `COLLECTION_DEFAULT_SLICE` de l'application `HTTPAPI_V1` (10),
* **offset** :
  * il indique le nombre d'éléments à exclure avant de retourner la collection.
  * valeur par défaut : 0

<span class="flag inline nota-bene"></span> 
Les paramètres appliqués sont résumés dans le retour de la collection 
`requestParameter`.

### Informations retournées {#httpapi-ref:170d8527-8224-404d-af50-ed3cd0f6bd55}

Les documents peuvent être retournés avec plus ou moins d'information.

* GET /searches/?fields=document.properties
* GET /searches/?fields=document.properties.id,document.properties.title

Par défaut : `fields=document.properties`

|            fields            |                 Signification                 |                      Remarques                      |
| ---------------------------- | --------------------------------------------- | --------------------------------------------------- |
| `document.properties`        | Récupère l'ensemble des propriétés par défaut | "id", "title", "icon", "initid", "name", "revision" |
| `document.properties.all`    | Récupère toutes les propriétés                |                                                     |
| `document.properties.<prop>` | Récupère la propriété indiquée                |                                                     |

La liste des propriétés est documentée dans la [documentation de format collection][properties].

## Cache {#httpapi-ref:ea45c330-8541-41e1-9167-9fa4b4990b26}

La collection n'a pas de cache.

[properties]: http://docs.anakeen.com/dynacase/3.2/dynacase-doc-core-reference/website/book/core-ref:74ce9ce4-8e4e-42ee-a0df-415eb6897a81.html#core-ref:9ebcbfd6-d094-45ee-a993-9b221fb4d893