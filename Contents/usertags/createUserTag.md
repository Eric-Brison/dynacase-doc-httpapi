# Créer un tag utilisateur {#httpapi-ref:8f415def-6741-49a9-b0dd-fb7824c648be}
## URL  {#httpapi-ref:413d298b-f21f-43bb-9e73-96b52047c353}

    POST /api/v1/documents/<documentId>/usertags/<tagid>

Création du tag `tagid` pour document `documentId`.
Le tag est associé à l'utilisateur connecté.

Exemple :

    POST /api/v1/documents/my_document/usertags/my_custom

Le droit de modifier le document n'est pas requis pour ajouter un tag. Seul le
droit de consulter le document est requis.

## Content  {#httpapi-ref:60fae96d-6011-4f30-942c-484ad5ebd4d7}

Le contenu contient la valeur du tag.
Si elle est vide la valeur sera égale à la chaîne vide.

Si le contenu est une structure json, la valeur retournée sera une structure.

## Structure de retour  {#httpapi-ref:56a1414f-5574-4544-8c04-4a353f424f8c}

Le retour est une donnée JSON.

### En cas de réussite :  {#httpapi-ref:f7389131-7c55-4476-b566-2a510306b5be}

La partie `data` contient 2 champs :

1.  `uri` : URI préférentielle d'accès à la ressource;
1.  `userTag` : liste des valeurs des propriétés;

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
                "value": "Hello"
            }
        }
    }




### En cas d'échec  {#httpapi-ref:5dccf953-8adb-4cff-b3d4-e90c4b08673d}

Les raisons d'échecs spécifiques à cette requête sont 

|                     Raison                     | Status HTTP | Error Code |
| ---------------------------------------------- | ----------- | ---------- |
| Privilège insuffisant pour accéder au document |         403 | CRUD0201   |
| Document supprimé                              |         404 | CRUD0108   |
| Tag demandé existant                           |         400 | CRUD0225   |

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

