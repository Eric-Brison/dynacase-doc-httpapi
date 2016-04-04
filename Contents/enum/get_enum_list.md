# Liste des attributs énumérés {#httpapi-ref:69fba1cf-5754-4189-ac07-c16f348e7fda}

## URL  {#httpapi-ref:9a7f786a-d1da-49a6-ab5b-87ddaa460f4d}

    GET /api/v1/families/<famName>/enumerates/

Récupére la liste des énumérés de la famille `<famname>`.

## Content  {#httpapi-ref:7b3b19a8-646c-4082-b8a0-a333c5a75893}

Le contenu de la requête est vide.

## Structure de retour  {#httpapi-ref:4b63cc4a-fc3a-4bff-825f-252a300dceb4}

Le retour est une donnée JSON.

### En cas de réussite :  {#httpapi-ref:9a44c2df-f3d4-4dc1-a2f8-c6eeb75b1863}

    [javascript]
    {
        "success": true,
        "messages": [],
        "data": {
            "uri": "http://www.example.net/api/v1/families/DIR/enumerates/",
            "enumerates": [
                {
                    "attributeId": "gui_isrss",
                    "label": "Utilisable comme flux RSS",
                    "uri": "http://www.example.net/api/v1/families/DIR/enumerates/gui_isrss.json"
                },
                {
                    "attributeId": "gui_sysrss",
                    "label": "Flux RSS système",
                    "uri": "http://www.example.net/api/v1/families/DIR/enumerates/gui_sysrss.json"
                },
                {
                    "attributeId": "fld_allbut",
                    "label": "tout ou rien",
                    "uri": "http://www.example.net/api/v1/families/DIR/enumerates/fld_allbut.json"
                },
                {
                    "attributeId": "fld_subfam",
                    "label": "restriction sous famille",
                    "uri": "http://www.example.net/api/v1/families/DIR/enumerates/fld_subfam.json"
                }
            ]
        }
    }

L'api retourne une liste d'énumérés (`enumerates`) chacun des éléments contient :
 
* l'URI permettant d'accéder à la liste des valeurs,
* le label de l'attribut énuméré dans la langue de l'utilisateur appelant,
* le nom logique de l'attribut énuméré.


### En cas d'échec  {#httpapi-ref:7539ed03-7491-4ccb-acec-51c2a332a18e}

Les raisons d'échecs spécifiques à cette requête sont 

|                     Raison                     | Status HTTP | Error Code |
| ---------------------------------------------- | ----------- | ---------- |
| Famille non trouvée                            |         404 | API0220    |

Exemple : 

    [javascript]
    {
        "success": false,
        "messages": [
            {
                "type": "error",
                "contentText": "Document \"NOT_A_FAMILY\" not found",
                "code": "API0200"
            },
            {
                "type": "message",
                "contentText": "You can consult http://www.example.net/index.php?app=HTTPAPI_V1 to have info on the API",
                "contentHtml": "You can consult <a href=\"http://www.example.net/index.php?app=HTTPAPI_V1\">the REST page</a> to have info on the API"
            }
        ],
        "data": null,
        "exceptionMessage": "Document \"NOT_A_FAMILY\" not found"
    }



## Filtres  {#httpapi-ref:d77fb9b8-aa8e-4a25-bc46-c2df14c74ceb}

### Slice {#httpapi-ref:1245cc5f-ce50-4f05-98d7-7ad7b8b5b4d0}

Cette option indique le nombre de valeur maximum à retourner.

Exemple :

    GET api/v1/families/<famName>/enumerates/?slice=10

### Offset {#httpapi-ref:0a2255e6-86b0-43cf-b72d-a9e4e286bb03}

Index à partir duquel, les énumérés sont retournées.

Exemple: 

    GET api/v1/families/<famName>/enumerates/?slice=2&offset=7
