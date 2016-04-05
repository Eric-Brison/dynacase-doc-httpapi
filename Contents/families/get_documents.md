# Consultation des documents de la familles {#httpapi-ref:f21d3f3f-82ea-48a9-bb9e-ba986bae9b62}

## URL canonique {#httpapi-ref:2967f00f-d472-44fc-8bb8-e5cb1187efe9}

    GET /api/v1/families/<famName>/documents/

Récupération de la liste des documents de la famille `families` présents sur la plateforme.

## Content {#httpapi-ref:1478e0ce-931d-45c6-b62a-c85a6d676391}

Le contenu de la requête est vide.

## Structure de retour {#httpapi-ref:0e5220e6-0b3a-4c81-8947-bff9ebbb8d61}

Le retour est une donnée JSON.

### En cas de réussite : {#httpapi-ref:ddec0249-b31b-4004-909a-0347370ad3ce}

La partie `data` contient :

1.  `requestParameters` : contient un résumé des paramètres de la requête en cours (pagination et orderBy),
1.  `uri` : URI d'accès de la collection,
1.  `properties` : titre de la recherche (composée à partie du titre de la famille)
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
                "length": 2,
                "orderBy": "title asc, id desc"
            },
            "uri": "/api/v1/families/TEXT/documents/",
            "properties": {
                "title": "Texte Documents"
            },
            "documents": [
                {
                    "properties": {
                        "id": 1054,
                        "title": "La vie des fourmis",
                        "icon": "resizeimg.php?img=Images%2Ftext.gif&size=24",
                        "initid": 1054,
                        "name": null,
                        "revision": 0
                    },
                    "uri": "/api/v1/documents/1054.json"
                },
                {
                    "properties": {
                        "id": 1053,
                        "title": "Les grands philosophes",
                        "icon": "resizeimg.php?img=Images%2Ftext.gif&size=24",
                        "initid": 1053,
                        "name": null,
                        "revision": 0
                    },
                    "uri": "/api/v1/documents/1053.json"
                }
            ]
        }
    }

<span class="flag inline nota-bene"></span> Les valeurs retournées correspondent aux valeurs de la vue de consultation
par défaut.

### En cas d'échec {#httpapi-ref:27f3e1f4-282e-40a7-bf3c-7d9c8f3284fb}

Les raisons d'échecs spécifiques à cette requête sont 

|                     Raison                     | Status HTTP | Error Code |
| ---------------------------------------------- | ----------- | ---------- |
| Sens de l'orderBy inconnu                      |         400 | CRUD0501   |
| Attribut ou propriété d'orderBy invalide       |         400 | CRUD0502   |

## Résultat partiel {#httpapi-ref:aa550b99-3d9b-4ce9-a5a7-e5e6c794a8f1}

### Pagination et tri {#httpapi-ref:c1f59c59-0edc-4bae-9149-ccba71f39a50}

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

* GET `families/<famName>/documents/?orderBy=title:asc,id:desc&slice=100&offset=20`

### Informations retournées {#httpapi-ref:8bcaf080-db65-46d2-b1b9-9c1f7ba82f6f}

Les documents peuvent être retournés avec plus ou moins d'information.

* GET `families/<famName>/documents/?fields=document.properties`
* GET `families/<famName>/documents/?fields=document.properties.id,document.properties.title`

Par défaut : `fields=document.properties`

|               fields               |               Signification                |                               Remarques                               |
| ---------------------------------- | ------------------------------------------ | --------------------------------------------------------------------- |
| `document.properties`              | Récupère les propriétés données par défaut | "id", "title", "icon", "initid", "name", "revision"                   |
| `document.properties.all`          | Récupère toutes les propriétés             |                                                                       |
| `document.properties.<prop>`       | Récupère la propriété indiquée             |                                                                       |
| `document.attributes.my_attribute` | Ajoute l'attribut my_attribute             | si l'attribut n'existe pas dans un des documents il est retourné vide |
| `document.attributes`              | Ajoute tous les attributs                  |                                                                       |

La liste des propriétés est documentée dans la [documentation de format collection][properties].

## Cache {#httpapi-ref:6128563b-6ad9-4bf0-941d-df876c017e4a}

La collection n'a pas de cache.

[properties]: http://docs.anakeen.com/dynacase/3.2/dynacase-doc-core-reference/website/book/core-ref:74ce9ce4-8e4e-42ee-a0df-415eb6897a81.html#core-ref:9ebcbfd6-d094-45ee-a993-9b221fb4d893