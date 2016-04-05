# Liste des tags utilisateur {#httpapi-ref:4d01fe57-4cd9-4989-adb8-f088762e4236}
## URL  {#httpapi-ref:a88fa744-48a3-4992-8358-234d90a0f17f}

    GET /api/v1/documents/<documentId>/usertags/

Récupération des tags de l'utilisateur connecté pour le document `documentId`.

Exemple :

    GET /api/v1/documents/my_document/usertags/


## Content  {#httpapi-ref:a76c5cd9-78cc-4eac-8d35-b21a129ff00d}

Le contenu de la requête est vide.

## Structure de retour  {#httpapi-ref:4c15c888-e309-41f9-8ad2-fb6d74561165}

Le retour est une donnée JSON.

### En cas de réussite :  {#httpapi-ref:7b516188-fbc9-42cd-ab9f-9adb563e9aa2}

La partie `data` contient :

1.  `requestParameters` : contient un résumé des paramètres de la requête en cours (pagination),
1.  `uri` : URI d'accès de la collection,
1.  `userTags` : un tableau de tags utilisateur 

Chaque tag utilisateur est un objet contenant les entrées suivantes :

1.  `id` : identifiant du tag (les identifiants sont sensibles à la casse),
1.  `date` : date de pose du tag,
1.  `value` : valeur du tag,
1.  `uri` : URI d'accès au tag.

Exemple :

    [javascript]
    {
        "success": true,
        "messages": [],
        "data": {
            "uri": "http://www.example.net/api/v1/documents/34757/usertags/",
            "requestParameters": {
                "slice": -1,
                "offset": 0
            },
            "userTags": [
                {
                    "id": "lasttab",
                    "date": "2015-01-07 17:40:43",
                    "uri": "http://www.example.net/api/v1/documents/34757/usertags/lasttab",
                    "value": "tst_t_tab_relations"
                },
                {
                    "id": "VIEWED",
                    "date": "2015-01-07 16:09:13",
                    "uri": "http://www.example.net/api/v1/documents/34757/usertags/VIEWED",
                    "value": ""
                },
                {
                    "id": "my_special",
                    "date": "2014-12-24 09:21:41",
                    "uri": "http://www.example.net/api/v1/documents/34757/usertags/solo",
                    "value": {
                        "a": 1
                    }
                }
            ]
        }
    }


### En cas d'échec  {#httpapi-ref:676e02f2-4c53-4c76-87ae-96e0d46544ca}

Les raisons d'échecs spécifiques à cette requête sont 

|                     Raison                     | Status HTTP | Error Code |
| ---------------------------------------------- | ----------- | ---------- |
| Privilège insuffisant pour accéder au document |         403 | CRUD0201   |
| Document supprimé                              |         404 | CRUD0108   |
| Document non existant                          |         404 | CRUD0200   |

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



## Pagination et tri {#httpapi-ref:f0e8341c-e6a9-4643-93ab-66687d501f4b}


La liste des tags utilisateur est paginée et ordonnée.

Les mots clefs GET sont les suivants :

* **slice** : 
  * il indique le nombre maximum de tag à retourner, sa valeur est un entier. 
    Si sa valeur est inférieur ou égale à 0, toutes les valeurs sont retournés,
  * sa valeur par défaut '-1',
* **offset** :
  * indique de passer ce nombre de tags avant de renvoyer les tags restants.
  * valeur par défaut : 0

<span class="flag inline nota-bene"></span> Les paramètres appliqués sont résumés 
dans le retour de la collection 
`requestParameter`.

Le tri des tags utilisé est basé sur la date de modification. Il est donné dans
l'ordre descendant (du plus récent au plus ancien).

Exemple : 

* GET `/api/v1/documents/my_document/usertags/?slice=10&offset=20`

