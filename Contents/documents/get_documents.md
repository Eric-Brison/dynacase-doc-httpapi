# Consultation de la liste des documents {#httpapi-ref:2ee6dd78-5b5a-4e00-aba5-4cd85c8a1cdc}

## URL canonique {#httpapi-ref:7ff62475-453d-4a3f-a667-3c25d68322cb}

    GET /api/v1/documents/

Récupération de la liste des documents présents sur la plateforme.

## Content {#httpapi-ref:1ed73b68-6252-45b4-884f-cbe54fa23dbc}

Le contenu de la requête est vide.

## Structure de retour {#httpapi-ref:11e1501d-594c-42fb-a076-c375233b48a0}

Le retour est une donnée JSON.

### En cas de réussite : {#httpapi-ref:8852cb99-39d1-4ac5-91e9-82627ef0408f}

La partie `data` contient :

1.  `requestParameters` : contient un résumé des paramètres de la requête en cours (pagination et orderBy),
1.  `uri` : URI d'accès de la collection,
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
            "uri": "/api/v1/documents/",
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
                    "uri": "/api/v1/documents/1003.json"
                },
                [...]
            ]
        }
    }

<span class="flag inline nota-bene"></span> Les valeurs retournées 
correspondent aux valeurs de la vue de consultation
par défaut.

### En cas d'échec {#httpapi-ref:467331a9-cb91-4301-ab66-3f960f6f5e6e}

Les raisons d'échecs spécifiques à cette requête sont 

|                     Raison                     | Status HTTP | Error Code |
| ---------------------------------------------- | ----------- | ---------- |
| Sens de l'orderBy inconnu                      |         400 | CRUD0501   |
| Attribut ou propriété d'orderBy invalide       |         400 | CRUD0502   |

## Résultat partiel {#httpapi-ref:bb65587e-8b65-4473-b3ba-11678d3b25f5}

### Pagination et tri {#httpapi-ref:678398f5-d979-482e-ab16-b30c02fc1bf5}

La liste des documents est paginée et ordonnée.

Les mots clefs GET sont les suivants :

* **orderBy** : `<attribut|propriété>:<asc:desc>`
  * indique dans quel sens la collection doit être triée, on peut mettre plusieurs valeurs séparées par des ,
  * valeur par défaut : `title:asc`,
* **slice** : 
  * il indique le nombre maximum de documents à retourner, sa valeur est un entier ou le mot clef `all`,
  * sa valeur par défaut est celle du paramètre applicatif `COLLECTION_DEFAULT_SLICE` de l'application `HTTPAPI_V1` (10),
* **offset** :
  * indique de passer ce nombre de document avant de renvoyer les documents restants.
  * valeur par défaut : 0

<span class="flag inline nota-bene"></span> Les paramètres appliqués sont résumés 
dans le retour de la collection 
`requestParameter`.

Exemple : 

* GET `/documents/?orderBy=title:asc,id:desc&slice=100&offset=20`

### Informations retournées {#httpapi-ref:9ca4cfe7-3692-4b86-8ae0-b0a1c6a11487}

Les documents peuvent être retournés avec plus ou moins d'information.

* GET /documents/?fields=document.properties
* GET /documents/?fields=document.properties.id,document.properties.title

Par défaut : `fields=document.properties`

|               fields               |                 Signification                 |                               Remarques                               |
| ---------------------------------- | --------------------------------------------- | --------------------------------------------------------------------- |
| `document.properties`              | Récupère l'ensemble des propriétés par défaut | "id", "title", "icon", "initid", "name"                               |
| `document.properties.all`          | Récupère toutes les propriétés                |                                                                       |
| `document.properties.<prop>`       | Récupère la propriété indiquée                |                                                                       |
| `document.attributes.my_attribute` | Ajoute l'attribut my_attribute                | si l'attribut n'existe pas dans un des documents il est retourné vide |
| `requestParameters.slice`          | Nombre maximum de document demandé            |                                                                       |
| `requestParameters.offset`         | Nombre de premiers document à ignorer         |                                                                       |
| `requestParameters.orderBy`        | Tri demandé                                   |                                                                       |
| `requestParameters.length`         | Nombre de document effectivement retourné     |                                                                       |

La liste des propriétés est documentée dans la [documentation de format collection][properties].

## Cache {#httpapi-ref:804900bb-7935-4264-bdfe-1d747eba5f39}

La collection n'a pas de cache.

[properties]: http://docs.anakeen.com/dynacase/3.2/dynacase-doc-core-reference/website/book/core-ref:74ce9ce4-8e4e-42ee-a0df-415eb6897a81.html#core-ref:9ebcbfd6-d094-45ee-a993-9b221fb4d893