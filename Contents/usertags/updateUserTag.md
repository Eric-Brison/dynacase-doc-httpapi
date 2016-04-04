# Modifier un tag utilisateur {#httpapi-ref:bcca7517-ecff-4e8a-8339-e8defa71bc4c}
## URL  {#httpapi-ref:d0abc03c-d650-4c7e-8e35-ce5de1795aed}

    PUT /api/v1/documents/<documentId>/usertags/<tagid>

Modification du tag `tagid` du document `documentId` pour l'utilisateur
connecté.

Exemple :

    PUT /api/v1/documents/my_document/usertags/my_custom

<span class="flag inline nota-bene"></span> Si le tag n'existe déjà, il est créé.

## Content  {#httpapi-ref:edf3bccd-44da-4589-8d1a-ac6597f10fb1}


Le contenu contient la valeur du tag.  
Si il est vide la valeur est égale à la chaîne vide.

Si le contenu est une structure JSON, la valeur retournée est une structure.

## Structure de retour  {#httpapi-ref:bf7517bd-d1bd-499f-9beb-0e8a401396de}

Le retour est une donnée JSON.

### En cas de réussite :  {#httpapi-ref:bc1a8bcb-b5cb-4605-a479-629af8598263}

La partie `data` contient 2 champs :

1.  `uri` : URI préférentielle d'accès à la ressource;
1.  `userTag` : liste des valeurs du tag;

Exemple :

    [javascript]
    {
        "success": true,
        "messages": [],
        "data": {
            "uri": "http://www.example.net/api/v1/documents/34757/usertags/test",
            "userTag": {
                "id": "test",
                "date": "2015-01-08 14:58:27",
                "value": {
                    "first":"Interesting",
                    "second" : 123.56
                }
            }
        }
    }


### En cas d'échec  {#httpapi-ref:58d87ac3-a366-4df9-9618-7c7b8d6b5a68}

Les raisons d'échecs spécifiques à cette requête sont 

|                     Raison                     | Status HTTP | Error Code |
| ---------------------------------------------- | ----------- | ---------- |
| Privilège insuffisant pour accéder au document |         403 | CRUD0201   |
| Document supprimé                              |         404 | CRUD0108   |

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

