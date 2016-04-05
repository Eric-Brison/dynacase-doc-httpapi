# Supprimer un tag utilisateur {#httpapi-ref:433d5c93-a890-4da2-b0a9-3e4089c14d3d}
## URL  {#httpapi-ref:dd0a4047-77cd-48ba-9bc2-48aa491593ce}

    DELETE /api/v1/documents/<documentId>/usertags/<tagid>

Suppression du tag `tagid` du document `documentId`

Exemple :

    DELETE /api/v1/documents/my_document/usertags/my_custom

Le droit de modifier le document n'est pas requis pour supprimer un tag. Seul le
droit de consulter le document est requis.

## Content  {#httpapi-ref:ec90e20c-dffc-4a0d-8b3a-0344fcc4f33e}

Le contenu de la requête est vide.

## Structure de retour  {#httpapi-ref:392d07b8-38d4-44f8-8e39-4396eb4ba662}

Le retour est une donnée JSON.

### En cas de réussite :  {#httpapi-ref:5f728fd6-e65f-4b3d-90d4-5a931d75e6a6}

La partie `data` est vide.

Exemple :

    [javascript]
    {
        "success": true,
        "messages": [],
        "data": null
    }


### En cas d'échec  {#httpapi-ref:91cee956-e200-47a6-b4ff-f78d0df4c954}
Les raisons d'échecs spécifiques à cette requête sont 

|                     Raison                     | Status HTTP | Error Code |
| ---------------------------------------------- | ----------- | ---------- |
| Privilège insuffisant pour accéder au document |         403 | CRUD0201   |
| Document supprimé                              |         404 | CRUD0108   |
| Tag demandé inexistant                         |         400 | CRUD0223   |
|                                                |             |            |

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

