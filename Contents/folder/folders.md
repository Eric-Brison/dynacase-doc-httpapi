# Liste des dossiers {#httpapi-ref:b3f83d12-4ea7-44e2-8509-1145d05003d6}

## URL canonique {#httpapi-ref:4a76de69-324a-472b-afd3-ced155682c29}

    GET /api/v1/folders/

Récupération de la liste des dossiers accessibles.

## Content {#httpapi-ref:3d8f3937-b9af-47da-9b66-e57a518537d9}

Le contenu de la requête est vide.

## Structure de retour {#httpapi-ref:511f865c-a04d-44e2-b9ec-b670a85ea92f}

Le retour est une donnée JSON.

### En cas de réussite : {#httpapi-ref:b4f272ae-c84f-4097-91d5-497a29d86f8d}

La partie `data` contient :

1.  `requestParameters` : contient un résumé des paramètres de la requête en cours (pagination et orderBy),
1.  `uri` : URI d'accès de la collection,
1.  `documents` : un tableau de document (sous la même forme que les documents unitaires)

Chaque document est un objet contenant les entrées suivantes :

1.  `properties` : liste des propriétés du dossier,
1.  `uri` : URI du contenu du dossier

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
            "uri": "/api/v1/folders/",
            "properties": {
                "title": "The folders"
            },
            "documents": [
                {
                    "properties": {
                        "id": 1011,
                        "title": "Administrateurs",
                        "icon": "resizeimg.php?img=Images%2Figroup.png&size=24",
                        "initid": 1011,
                        "name": "GADMIN",
                        "revision": 0
                    },
                    "uri": "/api/v1/folders/1011/documents/"
                }
            ], ...
        }
    }


### En cas d'échec {#httpapi-ref:2dd9eb61-e8d3-4579-aa68-609de685d877}

Les raisons d'échecs spécifiques à cette requête sont 

|                     Raison                     | Status HTTP | Error Code |
| ---------------------------------------------- | ----------- | ---------- |
| Sens de l'orderBy inconnu                      |         400 | CRUD0501   |
| Attribut ou propriété d'orderBy invalide       |         400 | CRUD0502   |

## Résultat partiel {#httpapi-ref:5e59e2cc-87ba-4bcd-9c06-44db0f0f7065}

### Pagination et tri {#httpapi-ref:cb3f11ad-db7f-45bb-943b-2835c01878e7}

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

<span class="flag inline nota-bene"></span> Les paramètres appliqués sont résumés dans le retour de la collection 
`requestParameter`.

### Informations retournées {#httpapi-ref:6a10473b-3fce-4289-843d-8762d5c852e0}

Les documents peuvent être retournés avec plus ou moins d'information.

* GET /folders/?fields=document.properties
* GET /folders/?fields=document.properties.id,document.properties.title

Par défaut : `fields=document.properties`

|           fields                   |                        Signification                         |                                                           Remarques                                                           |
| ---------------------------------- | ------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------- |
| `document.properties`              | Récupère l'ensemble des propriétés par défaut                | "id", "title", "icon", "initid", "name"                                                                                       |
| `document.properties.all`          | Récupère toutes les propriétés                               |                                                                                                                               |
| `document.properties.<prop>`       | Récupère la propriété indiquée                               |                                                                                                                               |

La liste des propriétés est documentée dans la [documentation de format collection][properties].

## Cache {#httpapi-ref:0fb54612-5946-4d8f-9d6c-492a08fc30e7}

La collection n'a pas de cache.

[properties]: http://docs.anakeen.com/dynacase/3.2/dynacase-doc-core-reference/website/book/core-ref:74ce9ce4-8e4e-42ee-a0df-415eb6897a81.html#core-ref:9ebcbfd6-d094-45ee-a993-9b221fb4d893