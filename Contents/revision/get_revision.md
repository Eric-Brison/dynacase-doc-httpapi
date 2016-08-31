# Consultation d'une révision d'un document {#httpapi-ref:eb7b6954-0945-4f02-8e10-16e69729c529}

## URL {#httpapi-ref:bc473017-a87d-497e-a374-23ac9eb9f45c}

    GET /api/v1/documents/<documentId>/revisions/<revisionNumber>

Récupération de la révision `<revisionNumber>` de la dernière révision d'un
document ayant l'id `<documentId>`.

L'extension ".json" peut être ajoutée pour expliciter le format de sortie.

Exemple :

    GET /api/v1/documents/1234/revisions/0.json

L'identifiant du document peut être son nom logique, son identifiant numérique.

## Content {#httpapi-ref:b0150fea-005b-4c7e-92d3-79ee9a07f5a4}

Le contenu de la requête est vide.

## Structure de retour {#httpapi-ref:d3f1d448-be2d-48da-9e41-d0c58889e104}

Le retour est une donnée JSON.

### En cas de réussite : {#httpapi-ref:1ba36666-d1d6-47f0-b807-fc767c7c79b0}

La partie `data` contient 3 champs :

1.  `revision.uri` : uri d'accès à la ressource modifiée
1.  `revision.properties` : liste des valeurs des propriétés
1.  `revision.attributes` : liste des valeurs des attributs

Les attributs en visibilité "I" ne sont pas retournés. 

Exemple :

    [javascript]
    {
        "success": true,
        "messages": [],
        "data": {
            "revision": {
                "properties": {
                    "id": 28203,
                    "title": "Le monde sous-marin",
                    "icon": "api/v1/images/assets/sizes/24x24c/article.gif",
                    "initid": 7120,
                    "name": null,
                    "revision": 17,
                    "state": {
                        "reference": "my_transmited",
                        "color": "#A8E5FF",
                        "activity": null,
                        "stateLabel": "Transmis",
                        "displayValue": "Transmis"
                    }
                },
                "attributes": {
                    "de_reference": {
                        "value": "1",
                        "displayValue": "1"
                    },
                    "de_version": {
                        "value": "Deuxième",
                        "displayValue": "Deuxième"
                    },...
                },
                "uri": "http://www.example.net/api/v1/documents/7120.json"
            }
        }
    }




### En cas d'échec {#httpapi-ref:8fe1927f-ba0b-495e-b04d-a86233431f44}

Les raisons d'échecs spécifiques à cette requête sont 

|                     Raison                     | Status HTTP | Error Code |
| ---------------------------------------------- | ----------- | ---------- |
| Révision ou document non trouvé                |         404 | API0220    |
| Privilège insuffisant pour accéder au document |         403 | API0201    |
| Document supprimé                              |         404 | API0219    |
| Propriété demandée inexistante                 |         400 | API0202    |
| Attribut demandé inexistant                    |         400 | API0218    |

Exemple : 

Cas d'erreur de document non trouvé

    [javascript]
    {"success" :             false,
        "messages" :         [
            {
                "type" :        "error",
                "contentText" : "Document \"1200\" not found",
                "contentHtml" : "",
                "code" :        "API0200",
                "uri" :         "",
                "data" :        null
            }
        ],
        "data" :             null,
        "exceptionMessage" : "Document \"1200\" not found"
    }

## Résultat partiel {#httpapi-ref:3d1a9fa7-8afa-41cf-9c01-b4088d1ead6a}

Le document peut être retourné avec plus ou moins d'information.

* GET /documents/1234/revisions/0.json?fields=document.properties
* GET /documents/1234/revisions/0.json?fields=document.properties.status,document.attributes
* GET /documents/1234/revisions/0.json?fields=document.properties.id,document.properties.title,document.attributes.my_exemple
* GET /documents/1234/revisions/0.json?fields=document.properties.id,document.properties.title,document.attributes,family.structure

Par défaut : `fields=document.properties,document.attributes`

|            fields            |                        Signification                         |                       Remarques                        |
| ---------------------------- | ------------------------------------------------------------ | ------------------------------------------------------ |
| `document.properties`        | Récupère l'ensemble des propriétés "visibles"                | "state",  "id",  "initid",  "revision", "icon", "name" |
| `document.properties.<prop>` | Récupère la propriété indiquée                               |                                                        |
| `document.attributes`        | Récupère les valeurs et les valeurs affichable des attributs |                                                        |
| `document.attributes.<id>`   | Récupère la valeur d'un attribut particulier                 |                                                        |
| `document.family.structure`  | Récupère la structure de la famille                          |                                                        |

La liste des propriétés est documentée dans la [documentation de format collection][properties].

## Cache {#httpapi-ref:d9fde0fc-824e-41b7-aac6-ed07a52b6b56}

Dans le cadre du [cache][cache], le `Etag` est calculé à l'aide des éléments suivants :

* id de la révision,
* date de dernière modification,
* identifiant de l'utilisateur,
* identifiant des droits portés sur le document (vecteur de droits),
* langue sélectionnée.

L'ensemble de ces éléments sont concaténés et ensuite le [sha1][sha1] de cette concaténation consitue le `Etag`.

## Autres URL d'accès {#httpapi-ref:1bbf4922-a12f-4cc4-8aa0-16853148ac14}

Vous pouvez aussi accéder à cette ressources via :

    GET /api/v1/families/<famName>/documents/<documentId>/revisions/<revisionNumber>

Récupération de la révision `<revisionNumber>` de la dernière révision d'un document de la famille `<famName>` ayant
l'identifiant `<documentId>`.

<span class="flag inline nota-bene"></span> La différence entre les collection `families` et `documents` est que pour
la collection `/api/v1/families/<famName>/documents/<documentId>/revisions/<revisionNumber>` l'identifiant doit être dans la famille indiquée pour être retourné sinon une
erreur 404 (ressource non trouvée) est retournée.


<span class="flag inline nota-bene"></span> La révision d'un document
"supprimé" pevent être récupérés via l'url

    GET /api/v1/trash/<documentId>/revisions/<revisionNumber>


[trash]: #httpapi-ref:52be10c1-9f46-456b-a22f-24909386567
[cache]: #httpapi-ref:804f8d68-acfa-4a35-bb41-27b2a27c14dc
[sha1]: https://fr.wikipedia.org/wiki/SHA-1
[properties]: http://docs.anakeen.com/dynacase/3.2/dynacase-doc-core-reference/website/book/core-ref:74ce9ce4-8e4e-42ee-a0df-415eb6897a81.html#core-ref:9ebcbfd6-d094-45ee-a993-9b221fb4d893