# Valeurs possibles d'un attribut énuméré  {#httpapi-ref:bb13e401-1859-4c73-b299-70b801ed7eb0}

## URL {#httpapi-ref:e7530c89-82d0-4888-9d1e-3b354476928f}

    GET /api/v1/families/<famName>/enumerates/<attrid>

Récupération des valeurs de l'énuméré de l'attribut `attrid` et de la famille `famName`.

## Content {#httpapi-ref:d10aa78c-e870-4504-b566-c7f53e137e1a}

Le contenu de la requête est vide.

## Structure de retour {#httpapi-ref:0fd3954e-3cb5-46d7-b96f-8dd33b44fa5f}

Le retour est une donnée JSON.

### En cas de réussite : {#httpapi-ref:d562ebc3-3b03-4b6b-a471-855a04a821ba}

La partie `data` contient 3 champs :

1.  `document.data.uri` : URI d'accès à la ressource;
1.  `document.data.label` : label de l'attribut traduit dans la langue de l'utilisateur;
1.  `document.data.requestParameters` : filtre appliqués à la recherche;
1.  `document.data.enumItems` : tableau d'objets :
    1.  `key` : valeur de l'élément
    1.  `label` : libellé (traduit dans la langue de l'utilisateur)


Exemple :

    [javascript]
    {
        "success": true,
        "messages": [],
        "data": {
            "uri": "http://www.example.net/api/v1/families/my_article/enumerates/my_continent.json",
            "label": "Continents",
            "requestParameters": {
                "operator": "contains",
                "keyword": ""
            },
            "enumItems": [
                {
                    "key": "Asia",
                    "label": "Asie"
                },
                {
                    "key": "Europa",
                    "label": "Europe"
                },
                {
                    "key": "Africa",
                    "label": "Afrique"
                },
                {
                    "key": "SouthAmerica",
                    "label": "Amérique du sud"
                },
                {
                    "key": "NorthAmerica",
                    "label": "Amérique du nord"
                },
                {
                    "key": "Oceania",
                    "label": "Océanie"
                },
                {
                    "key": "Antartica",
                    "label": "Antartique"
                }
            ]
        }
    }

### En cas d'échec {#httpapi-ref:704f567e-eb1c-4800-bc03-e9076b2fb849}

Les raisons d'échecs spécifiques à cette requête sont 

|                     Raison                     | Status HTTP | Error Code |
| ---------------------------------------------- | ----------- | ---------- |
| Famille non trouvée                            |         404 | API0200    |
| Attribut non trouvé                            |         404 | API0400    |
| Attribut de type non énuméré                   |         403 | API0401    |

Exemple : 

Cas d'erreur de en cas d'attribut non énuméré

    [javascript]
    {"success" :             false,
        "messages" :         [
            {
                "type" :        "error",
                "contentText" : "Attribute \"an_tatouage\" is not an enum (type \"int\") in family \"ZOO_ANIMAL\"",
                "contentHtml" : "",
                "code" :        "API0401",
                "uri" :         "",
                "data" :        null
            }
        ],
        "data" :             null,
        "exceptionMessage" : "Attribute \"an_tatouage\" is not an enum (type \"int\") in family \"ZOO_ANIMAL\""
    }

## Résultat partiel {#httpapi-ref:2b3c2de1-4a64-4d07-98b5-ad89f148be3e}

L'attribut peut être retourné avec plus ou moins d'items.

* GET `/api/v1/families/IUSER/enumerates/us_rolesorigin.json?keyword=<keyword>`
* GET `/api/v1/families/IUSER/enumerates/us_rolesorigin.json?operator=startswith&keyword=<keyword>`

Les paramètres sont :

* `keyword` : celui-ci correspond au label,
* `operator` : celui-ci correspond à l'opérateur appliqué, deux choix sont possibles :
* `startswith` : le label commence par le `keyword`,
* `contains` : le label contient le `keyword`.
