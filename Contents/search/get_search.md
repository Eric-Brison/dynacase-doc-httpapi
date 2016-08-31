# Consultation du contenu d'une recherche {#httpapi-ref:b7fb15e0-6e51-4ace-8a5f-aec7d565e24d}

## URL canonique {#httpapi-ref:2e662769-63e0-42b5-acf4-fb26e039a775}

    GET /api/v1/searches/<searchId>/documents/

Récupération de la liste des documents trouvés par la recherche `<searchId>`.

## Content {#httpapi-ref:6ca763df-432e-4d8f-a14f-68559cc6f705}

Le contenu de la requête est vide.

## Structure de retour {#httpapi-ref:beb23765-7d41-48c8-92f3-f4b6769e8a3e}

Le retour est une donnée JSON.

### En cas de réussite : {#httpapi-ref:9efd746f-b036-4e02-8c02-a1c49084ff53}

La partie `data` contient :

1.  `requestParameters` : contient un résumé des paramètres de la requête en cours (pagination et orderBy),
1.  `uri` : URI d'accès de la collection,
1.  `properties`: contient le titre de la recherche, et l'uri d'accès au document "recherche"
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
                "length": 1,
                "orderBy": "title asc, id desc"
            },
            "uri": "http://www.example.net/api/v1/searches/5236/documents/",
            "properties": {
                "title": "Articles intéressants",
                "uri": "http://localhost/tmp32/api/v1/document/5236.json"
            },
            "documents": [
                {
                    "properties": {
                        "id": 26653,
                        "title": "La culture des perles",
                        "icon": "api/v1/images/assets/sizes/24x24c/article.png",
                        "initid": 1425,
                        "name": null,
                        "revision": 1
                    },
                    "uri": "http://www.example.net/api/v1/documents/1425.json"
                }
            ]
        }
    }


<span class="flag inline nota-bene"></span> Les valeurs retournées correspondent aux valeurs de la vue de consultation
par défaut.

### En cas d'échec {#httpapi-ref:7e9e5f6b-068d-445c-8bbd-6d64d8314587}

Les raisons d'échecs spécifiques à cette requête sont 

|                              Raison                             | Status HTTP | Error Code |
| --------------------------------------------------------------- | ----------- | ---------- |
| Sens de l'orderBy inconnu                                       |         400 | CRUD0501   |
| Attribut ou propriété d'orderBy invalide                        |         400 | CRUD0502   |
| L'identifiant de la recherche ne correspond pas à une recherche |         400 | CRUD0503   |

## Résultat partiel {#httpapi-ref:75a948b3-b1bb-43ec-bb3b-2e4a77671ef2}

### Pagination et tri {#httpapi-ref:67bf8b79-c5a5-4b82-b923-a1c5ed78c69c}

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

* GET `/searches/<my_search>/?orderBy=title:asc,id:desc&slice=100&offset=20`

### Informations retournées {#httpapi-ref:d9d12fa3-bbea-49cb-bc86-0c730f0d73eb}

Les documents peuvent être retournés avec plus ou moins d'information.

* GET `/searches/<my_search>/documents/?fields=document.properties`
* GET `/searches/<my_search>/documents/?fields=document.properties.id,document.properties.title`
* GET `/searches/<my_search>/documents/?fields=document.properties.id,document.properties.title`
* GET `/searches/<my_search>/documents/?fields=document.attributes`
* GET `/searches/<my_search>/documents/?fields=document.attributes.my_attribute`

Par défaut : `fields=document.properties`

|               fields               |                            Signification                            |                                 Remarques                                  |
| ---------------------------------- | ------------------------------------------------------------------- | -------------------------------------------------------------------------- |
| `document.properties`              | Récupère l'ensemble des propriétés par défaut                       | "id", "title", "icon", "initid", "name", "revision"                        |
| `document.properties.all`          | Récupère toutes les propriétés                                      |                                                                            |
| `document.properties.<prop>`       | Récupère la propriété indiquée                                      |                                                                            |
| `document.attributes`              | Ajoute tous les attributs de la famille référencée par la recherche | s'il n'y a pas de famille de référence alors aucun attribut n'est retourné |
| `document.attributes.my_attribute` | Ajoute l'attribut my_attribute                                      | si l'attribut n'existe pas dans un des documents il est retourné vide      |

La liste des propriétés est documentée dans la [documentation de format collection][properties].

## Cache {#httpapi-ref:716b4215-feec-4325-b54d-8969144cf224}

La collection n'a pas de cache.

[properties]: http://docs.anakeen.com/dynacase/3.2/dynacase-doc-core-reference/website/book/core-ref:74ce9ce4-8e4e-42ee-a0df-415eb6897a81.html#core-ref:9ebcbfd6-d094-45ee-a993-9b221fb4d893