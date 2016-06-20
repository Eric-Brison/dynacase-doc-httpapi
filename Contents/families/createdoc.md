# Création d'un document  {#httpapi-ref:e769b476-0033-407c-b453-4e8466e09975}

## URL {#httpapi-ref:ea0fbaaf-b3aa-4307-9a92-eed7359c323e}

    POST /api/v1/families/<famName>/documents/


Création d'un document de la famille `<famName>`

Exemple :

    POST /api/v1/families/my_cookbook/documents/


<span class="flag inline nota-bene"></span> Le nom de la famille est insensible à la casse.

## Content {#httpapi-ref:09cc3928-048c-4724-bd92-2fe099d37244}

### Format JSON {#httpapi-ref:f9058127-a6a9-4f34-94d7-cf6ca89458fd}

Le contenu de la requête doit contenir une donnée JSON avec la liste des attributs à valuer.

    [javascript]
    {
        "document": {
          "attributes" : {
              "<attrName>" : {
                "value" : <newValue>
              }
          }
        }
    }

Le type de la requête est `application/json`.

Exemple :

    [javascript]
    {
      
        "document": {
          "attributes" : {
              "ba_ttle" : {"value" : "Hello world"},
              "ba_desc" : {"value" : "Nice Day"}
        }
    }


Note : Toute donnée additionnelle sera ignorée.

Cette forme ne permet pas d'enregistrer directement des fichiers dans le document.

Pour enregistrer le fichier, il sera nécessaire de passer par la ressource
[`file`][tmpfile] qui retournera un identifiant valide qui pourra être utilisé
comme valeur d'attribut de type fichier.


### Format urlEncoded {#httpapi-ref:a7f23059-e18a-4441-817c-2e08e60bb6ab}

Le contenu de la requête contient la liste des valeurs d'attributs à enregistrer.
Chaque variable (POST) est le nom de l'attribut (casse insensible).

Le type de la requête est `application/x-www-form-urlencoded`.

<span class="flag inline nota-bene"></span> Ce format peut être utilisé directement depuis un formulaire HTML.

Cette forme permet aussi d'enregistrer des fichiers dans le document.

## Structure de retour {#httpapi-ref:5c923b50-4a3c-404f-874b-94d4d0b1b367}

Le retour est une donnée JSON.

### En cas de réussite : {#httpapi-ref:e8c59df1-2313-4023-83b5-4c6786948eae}

La partie `data` contient un champ `document` qui inclut 3 champs :

1.  `document.uri` : uri d'accès à la nouvelle ressource
1.  `document.properties` : liste des valeurs des propriétés
1.  `document.attributes` : liste des valeurs des attributs

Exemple :

    [javascript]
    {
      success :true,
      messages : [],
      data : {
        "document" : {
              "uri" : "api/v1/documents/1256",
              "properties" : { 
                 "id" : 1256,
                 "title" : "Hello world",
                 ....
               }
              "attributes" : { 
                "ba_title" : {
                    "value" : "Hello world",
                    "displayValue" : "Hello world"
                },
                "ba_cost" : {
                    "value" : 234,
                    "displayValue" : "234.00 €"
                }
              }
            }
      }
    }

La liste des propriétés est documentée dans la [documentation de format collection][properties].

### En cas d'échec {#httpapi-ref:481851fe-c39f-424f-b13f-9756db981ad9}

Les raisons d'échecs spécifiques à cette requête sont 

|                             Raison                            | Status HTTP | Error Code |
| ------------------------------------------------------------- | ----------- | ---------- |
| Privilège insuffisant pour créer le document de cette famille |         403 | API0204    |
| Tentative de modification d'un attribut inconnu               |         403 | API0205    |
| Tentative de modification d'un attribut en visibilité I       |         403 | API0205    |
| Erreur lors de la création                                    |         403 | API0205    |
| Famille inconnue                                              |         404 | API0206    |


[properties]: http://docs.anakeen.com/dynacase/3.2/dynacase-doc-core-reference/website/book/core-ref:74ce9ce4-8e4e-42ee-a0df-415eb6897a81.html#core-ref:9ebcbfd6-d094-45ee-a993-9b221fb4d893
[tmpfile]:  #httpapi-ref:5797255d-128d-4aa4-9c11-2c8195cca63d