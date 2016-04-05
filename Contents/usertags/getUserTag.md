# Récupérer un tag utilisateur {#httpapi-ref:1ecfb964-e7ff-4adc-b874-f6887b800fc5}
## URL  {#httpapi-ref:f96d663a-7366-43ea-bae6-be915748d428}

    GET /api/v1/documents/<documentId>/usertags/<tagid>

Récupération du tag `tagid` du document `documentId` de l'utilisateur connecté.

Exemple :

    GET /api/v1/documents/my_document/usertags/my_custom


## Content  {#httpapi-ref:46755c22-9dab-45e6-b59d-b8b7103111c6}

Le contenu de la requête est vide.

## Structure de retour  {#httpapi-ref:6c4456cd-d004-4b35-8f51-9d11e480e375}

Le retour est une donnée JSON.

### En cas de réussite :  {#httpapi-ref:972158ec-40bc-42dd-8d59-8efa33a25b8b}

La partie `data` contient 2 champs :

1.  `uri` : URI préférentielle d'accès à la ressource;
1.  `userTag` : Propriétés du tag
    1.  `id` : identifiant du tag (les identifiants sont sensible à la casse),
    1.  `date` : date de pose du tag,
    1.  `value` : valeur du tag. La valeur peut avoir 3 formes :
        1. String : Pour les données chaîne (ex : `Hello world`)
        1. Numerique : Pour les données numériques (ex : `123.34`)
        1. Objet : Pour les données compatibles JSON (ex : `{"one": 12, "two":"Hello"}`)

Exemple :

    [javascript]
    {
        "success": true,
        "messages": [],
        "data": {
            "uri": "http://www.example.net/api/v1/documents/34757/usertags/my_custom",
            "userTag": {
                "id": "my_custom",
                "date": "2014-12-24 09:17:24",
                "value": {
                    "my_first": 1123,
                    "my_second": "Hello world"
                }
            }
        }
    }


### En cas d'échec  {#httpapi-ref:0c5478e7-294e-411f-b70e-57ce91408c3c}
Les raisons d'échecs spécifiques à cette requête sont 


|                     Raison                     | Status HTTP | Error Code |
| ---------------------------------------------- | ----------- | ---------- |
| Privilège insuffisant pour accéder au document |         403 | CRUD0201   |
| Document supprimé                              |         404 | CRUD0108   |
| Document non existant                          |         404 | CRUD0200   |
| Tag demandé inexistant                         |         404 | CRUD0223   |

Exemple : 

Cas d'erreur de privilège

    [javascript]
    {"success" :             false,
        "messages" :         [
            {
                "type" :        "error",
                "contentText" : "Document \"1051\" access deny : Pas de privil\u00e8ge view pour le document famille [1051]",
                "contentHtml" : "",
                "code" :        "API0201",
                "uri" :         "",
                "data" :        null
            }
        ],
        "data" :             null,
        "exceptionMessage" : "Document \"1051\" access deny : Pas de privil\u00e8ge view pour le document famille [1051]"
    }

