# Consultation du contenu d'un dossier {#httpapi-ref:f3e3869b-4dcf-40ee-9733-3f4b57e2386f}

## URL canonique {#httpapi-ref:0dd9c8af-80a2-4fd4-8859-1d2f6362aff9}

    GET /api/v1/folders/<folderId>/documents/

Récupération de la liste des documents contenu par le dossier `<folderId>`.

## Content {#httpapi-ref:a6f9271b-4262-4165-be35-83bc91c6d274}

Le contenu de la requête est vide.

## Structure de retour {#httpapi-ref:d98179aa-a11f-4028-9e0f-a43859d3220f}

Le retour est une donnée JSON.

### En cas de réussite : {#httpapi-ref:136feb3e-5345-4a9a-bc48-2bcc9de08395}

La partie `data` contient :

1.  `requestParameters` : contient un résumé des paramètres de la requête en cours (pagination et orderBy),
1.  `uri` : URI d'accès de la collection,
1.  `documents` : un tableau de documents (sous la même forme que les documents unitaires)

Chaque document est un objet contenant les entrées suivantes :

1.  `properties` : liste des propriétés du document,
1.  `attributes` : liste des attributs du document (facultatif),
1.  `uri` : URI d'accès au document

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
            "uri": "/api/v1/folders/9987/documents/",
            "properties": {
                "title": "Documents importants",
                "uri": "http://www.example.net/api/v1/documents/9987.json"
            },
            "documents": [
                {
                    "properties": {
                        "id": 1003,
                        "title": "Réchauffement climatique",
                        "icon": "api/v1/images/assets/sizes/24x24c/article.png",
                        "initid": 1003,
                        "name": null,
                        "revision": 0
                    },
                    "uri": "http://www.example.net/api/v1/documents/1003.json"
                },
                [...]
            ]
        }
    }

<span class="flag inline nota-bene"></span> 
Les valeurs retournées correspondent aux valeurs de la vue de consultation
par défaut.

### En cas d'échec {#httpapi-ref:c25358e8-0d31-43ce-aec8-acc3b9456e14}

Les raisons d'échecs spécifiques à cette requête sont 

|                          Raison                         | Status HTTP | Error Code |
| ------------------------------------------------------- | ----------- | ---------- |
| Sens de l'orderBy inconnu                               |         400 | CRUD0501   |
| Attribut ou propriété d'orderBy invalide                |         400 | CRUD0502   |
| L'identifiant du dossier ne correspond pas à un dossier |         400 | CRUD0504   |

## Résultat partiel {#httpapi-ref:9a860053-1b51-4371-8632-ab7f0ff7f2c6}

### Pagination et tri {#httpapi-ref:9bc36793-b627-445e-b2ed-66b30af89a45}

La liste des documents peut être paginée et ordonnée.

Les mots clefs GET sont les suivants :

* **orderBy** : `<attribut|propriété>:<asc:desc>`
  * indique dans quel sens la collection doit être triée, on peut mettre plusieurs valeurs séparées par des virgules
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

* GET `/folders/<folderId>/?orderBy=title:asc,id:desc&slice=100&offset=20`

### Informations retournées {#httpapi-ref:bd501807-6f2c-4626-b741-a88a9144f796}

Les documents peuvent être retournés avec plus ou moins d'information.

* GET `/folders/<folderId>/documents/?fields=document.properties`
* GET `/folders/<folderId>/documents/?fields=document.properties.id,document.properties.title`
* GET `/folders/<folderId>/documents/?fields=document.properties.id,document.properties.title`
* GET `/folders/<folderId>/documents/?fields=document.attributes`
* GET `/folders/<folderId>/documents/?fields=document.attributes.my_attribute`

Par défaut : `fields=document.properties`

|               fields               |                           Signification                           |                                 Remarques                                  |
| ---------------------------------- | ----------------------------------------------------------------- | -------------------------------------------------------------------------- |
| `document.properties`              | Récupère l'ensemble des propriétés par défaut                     | "id", "title", "icon", "initid", "name", "revision"                        |
| `document.properties.all`          | Récupère toutes les propriétés                                    |                                                                            |
| `document.properties.<prop>`       | Récupère la propriété indiquée                                    |                                                                            |
| `document.attributes`              | Ajoute tous les attributs de la famille référencée par le dossier | s'il n'y a pas de famille de référence alors aucun attribut n'est retourné |
| `document.attributes.my_attribute` | Ajoute l'attribut my_attribute                                    | si l'attribut n'existe pas dans un des documents il est retourné vide      |

La liste des propriétés est documentée dans la [documentation de format collection][properties].

## Cache {#httpapi-ref:8c76352a-8242-4e40-abb5-632c18730f6f}

La collection n'a pas de cache.

[properties]: http://docs.anakeen.com/dynacase/3.2/dynacase-doc-core-reference/website/book/core-ref:74ce9ce4-8e4e-42ee-a0df-415eb6897a81.html#core-ref:9ebcbfd6-d094-45ee-a993-9b221fb4d893