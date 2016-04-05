# Suppression d'un document  {#httpapi-ref:3358b3bd-bdf6-44ef-b1d7-438f8eb21067}

## URL {#httpapi-ref:f992edec-ca5c-4fa1-b829-f88d9e17347d}

    DELETE /api/v1/documents/<id>

Suppression de la lignée documentaire dont un des membres a l'identifiant `<documentId>`. 

L'extension ".json" peut être ajoutée pour expliciter le format de sortie.

Exemple :

    DELETE /api/v1/documents/1234.json

La suppression donne l'ordre de mettre le document dans la poubelle. 
Dans ce cas, toute la lignée documentaire du document est mise à la poubelle.

L'identifiant du document peut être son nom logique, son identifiant numérique, ou celui de n'importe laquelle de ses
révisions.

La suppression "physique" (réelle suppression de la ligne en base de données) n'est pas possible.

## Content {#httpapi-ref:dd7728aa-309c-42f1-a496-9172a929be9f}

Le contenu de la requête est vide.

## Structure de retour {#httpapi-ref:70a8207a-abd0-498c-b788-48b86a274c3a}

Le retour est une donnée JSON.

### En cas de réussite : {#httpapi-ref:305ab52d-2313-4594-b524-736875acd1b6}

La partie `data` contient 3 champs :

1.  `document.uri` : uri d'accès à la ressource supprimée;
2.  `document.properties` : liste des valeurs des propriétés du document supprimé;
3.  `document.attributes` : liste des valeurs des attributs du document supprimé.

Les attributs en visibilité "I" ne sont pas retournés. Leur existence n'est pas
dévoilée ni dans les données ni dans la structure.

Un document mis à la poubelle n'est plus accessible via la ressource `documents`
mais reste accessible par la ressource `trash`.

Exemple :

    [javascript]
    {"success" :             true,
        "messages" :         [],
        "data" :             {
            "document" : {
                "uri" :        "api\/v1\/trash\/1057.json",
                "properties" : {
                    "title" :     "La vie des escargots",
                    [...]
                },
                "attributes" : {
                    "my_title" : {"value" : "La vie des escargots", "displayValue" : "La vie des escargots"},
                    [...]
                }
            }
        },
        "exceptionMessage" : ""
    }

La liste des propriétés est documentée dans la [documentation de format collection][properties].

### En cas d'échec {#httpapi-ref:0e94bbdd-2051-436e-af38-fe718a1e87ee}

Les raisons d'échecs spécifiques à cette requête sont 

|                      Raison                      | Status HTTP | Error Code |
| ------------------------------------------------ | ----------- | ---------- |
| Privilège insuffisant pour supprimer le document |         403 | API0216    |
| Erreur lors de la suppression                    |         400 | API0215    |
| Document déjà supprimé                           |         404 | API0219    |

Exemple : 

Cas d'erreur de privilège

    [javascript]
    {"success" :             false,
        "messages" :         [
            {
                "type" :        "error",
                "contentText" : "Document \"1057\" deleted",
                "contentHtml" : "",
                "code" :        "API0219",
                "uri" :         "",
                "data" :        null
            }
        ],
        "data" :             null,
        "exceptionMessage" : "Document \"1057\" deleted"
    }

## Autres URL d'accès {#httpapi-ref:68c41d29-93e9-45a3-82c2-20858d2d3a04}

Vous pouvez aussi accéder à cette ressources via :

    DELETE /api/v1/families/<famName>/documents/<documentId>

Suppression de la lignée documentaire de la famille `<famName>` dont un des
membres a l'identifiant `<documentId>`.

<span class="flag inline nota-bene"></span> La différence entre les collection
`families` et `documents` est que pour la collection
`families/<famName>/documents/` l'identifiant doit être dans la famille indiquée
pour être retourné sinon une erreur 404 (ressource non trouvée) est retournée.

[properties]: http://docs.anakeen.com/dynacase/3.2/dynacase-doc-core-reference/website/book/core-ref:74ce9ce4-8e4e-42ee-a0df-415eb6897a81.html#core-ref:9ebcbfd6-d094-45ee-a993-9b221fb4d893