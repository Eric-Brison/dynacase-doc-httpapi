# API HTTP REST {#httpapi-ref:153c2369-c0bf-4e39-8f25-fd42e6a90280}

## Présentation {#httpapi-ref:ad5d1530-7806-4cf2-be3d-c407923cb67a}


L'API HTTP a pour but d'offrir un accès standard aux données de Dynacase.

L'architecture de cette api s'appuie sur une architecture [REST (Representational state transfer)][wikipedia_rest]
et permet de manipuler les **ressources** Dynacase suivantes :

* les documents,
* les familles,
* les fichiers.

## Quelques notions de REST {#httpapi-ref:4135ea93-d7ec-4903-b4c1-acb08623dcd5}

L'api présentée dans la suite du document est de type REST et utilise le vocabulaire propre à ces API, ce qui inclut :

* `ressource` : une ressource est un type de donnée et son contenu, dans notre cas un document, la description d'un
fichier, etc.,
* `collection` : une collection est un type de ressource. Dans notre cas, la liste des documents, la liste des fichiers, etc.
La collection est utilisée pour référencer une ressource, créer une ressource, détruire une ressource,
* `identifiant` : l'identifiant est la clef unique permettant de trouver une ressource au sein d'une collection. Dans
notre cas, c'est l'`id` d'un document, le `vaultid` d'un fichier, etc.

## Structure de l'API {#httpapi-ref:5883cb06-807a-4b58-8f60-88a0078fbbdd}

La structure est conforme au standard REST.

Les actions du [CRUD][wikipedia_crud] sont implémentées et associées aux méthodes de HTTP, suivant la liste
d'équivalence suivante :

| Action   | Méthode HTTP | URL                         | Action effectuée                                                               |
| :-     : | :      :     | :                      :    | :                                                                              |
| `Create` | `POST`       | `/api/v1/<collection>`      | Créé une nouvelle ressource dans la collection et répond avec la ressource     |
| `Read`   | `GET`        | `/api/v1/<collection>`      | Lit le contenu de la collection  et répond avec une liste de ressources        |
| `Read`   | `GET`        | `/api/v1/<collection>/<id>` | Lit la ressource `id` et répond avec la ressource                              |
| `Update` | `PUT`        | `/api/v1/<collection>`      | Remplace l'intégralité de la collection et répond avec une liste de ressources |
| `Update` | `PUT`        | `/api/v1/<collection>/<id>` | Modifie la ressource `id` de la collection et répond avec la ressource         |
| `Delete` | `DELETE`     | `/api/v1/<collection>`      | Supprime l'intégralité de la collection et répond avec une liste de ressources |
| `Delete` | `DELETE`     | `/api/v1/<collection>/<id>` | Supprime la ressource `id` de la collection et répond avec la ressource        |

Les accès à l'API HTTP Dynacase se font par l'url `http[s]://<url_du_contexte>/api/v1/`.

### Encodage {#httpapi-ref:3f228840-491e-4c4d-8d0a-2da3a3934179}

Tous les échanges, entrées et retours de l'API, sont encodés en **UTF-8**.

### Requête {#httpapi-ref:20d76bcc-4312-48e0-a4f1-ad73b4fe4f9b}

Les éléments de la requête sont les suivants :

#### Type de retour attendu {#httpapi-ref:646bac7c-ab37-4808-affa-023efcc935c3}

Le type de retour attendu (format) est précisé soit :

* dans l'URL

        [http]
        PUT /api/v1/<collection>/<identifier>.<type>

* dans le header HTTP : `accept`.

Actuellement, seul le type `json` est géré. Celui-ci est renvoyé dans le corps
de la requête HTTP.

Le type exprimé dans l'URL est prioritaire à celui du header HTTP.

Pour supporter certains navigateurs ne possédant pas les objets XMLHttpRequest
v2, il est possible d'ajouter le paramètre GET `alt=html` et d'avoir ainsi le
retour de la donnée `json` dans un textarea au sein d'une page web. <span
class="flag from release">1.0.1</span>

#### PUT (Mise à jour), POST (Création) {#httpapi-ref:24701246-e383-4f76-b431-09f56d34b6d3}

Les données à enregistrer dans la ressource peuvent être envoyées sous 2 formes :

1.  Sous la forme d'un objet JSON (application/json) transmis dans le corps de la requête HTTP,
2.  Sous la forme de variables encodées (x-www-form-urlencoded) transmis dans le code de la requête HTTP.

Les options de mise à jour sont envoyées via des variables sur l'URL.

#### GET (Consultation), DELETE (Suppression) {#httpapi-ref:97da33b4-26bb-4e36-9b32-d99a9df24eb0}

Le corps de la requête est vide.

Les options de consultation, suppression sont envoyées dans l'url à l'aide de variables dans l'URL.

Exemples :

 * Consultation du document `212` : `GET` : `/api/v1/documents/212`
 * Suppression du document `IUSER_JEAN_REMI` : `DELETE` : `/api/v1/documents/IUSER_JEAN_REMI`

#### Compatibilité {#httpapi-ref:}

Certains clients ne permettant pas d'effectuer des requêtes autre que `GET` et
`POST`, un fonctionnement en mode  compatiblité est possible. Pour ce faire, il
faut modifier le header HTTP en ajoutant `X-HTTP-Method-Override` :

Simuler une méthode `PUT` :

* POST `X-HTTP-Method-Override: PUT`

Simuler une méthode `DELETE` :

* POST `X-HTTP-Method-Override: DELETE`


### Réponse {#httpapi-ref:d886bec2-e0f8-48ed-b7e3-b13dd5375cd8}

L'API répond via plusieurs éléments, le contenu (`content`) du retour et les
entêtes (`headers`) HTTP.

#### Contenu {#httpapi-ref:2d4427f0-f2ec-493b-b443-343b525f76f2}

Dans le cas d'un retour JSON, la structure retournée contient les éléments
suivants :

    [javascript]
    {
        "success" : true,       // false ou true
        "messages" : [{
            "type" : "warning", // type de message error, userMessage, warning, notice, notification
            "contentText" : "once upon a time",
            "contentHtml" : "",
            "code" : "",        // code identifiant la catégorie du message
            "uri" : "",         // url d'accès à la page web correspondant à l'erreur
            "data" : { }        // données supplémentaires
            }],
        "data" : {}             // données demandées par la requête

    }

Ce retour est envoyé quelque soit le résultat de la requête, y compris en cas
d'erreur.

#### Code de retour http {#httpapi-ref:7e10161c-0412-4927-8e73-09795e58b571}

L'API utilise les [codes standards du protocole HTTP][wikipedia_http].

##### Les retours sans erreur {#httpapi-ref:c52418a7-218f-4794-88a8-f92feb88641d}

Si l'action demandée a été exécutée, le code HTTP 2xx est retourné.

**200**
:  Ressource trouvée

**201**
:  Ressource créé

Exemple de retour :

    [javascript]
    {
        "success" : true, // false ou true
        "messages" : [],
        "data" : {
            "document" : {
                    "properties" : {
                        "id" : 1224,
                        "title" : "Hello world"
                    },
                    "attributes" : {
                        "ba_ttle" : {"value" : "Hello world"},
                        "ba_desc" : {"value" : "Nice Day"},
                    }
              }
        }
    }

Dans l'enveloppe retournée, le booléen **success** est à **true**.

##### Les retours d'erreur {#httpapi-ref:e56e8fcd-13ab-456b-85d3-f9ce9b3c4123}

Si l'action demandée n'a pas pu aboutir, un code HTTP `4xx` ou en `5xx` est retourné.

**400**
:  Données en entrée invalides

**403**
:  Ressource protégée (accès interdit)

**404**
:  Ressource non trouvée

**501**
:  La méthode demandée n'est pas disponible sur cette ressource

**507**
:  Espace de stockage insuffisant sur le serveur


Exemple de retour, cas d'un 404 :

    [javascript]
    {"success" :             false,
        "messages" :         [
            {
                "type" :        "error",
                "contentText" : "Document \"22222\" not found",
                "contentHtml" : "",
                "code" :        "API0200",
                "uri" :         "",
                "data" :        null
            }
        ],
        "data" :             null,
        "exceptionMessage" : "Document \"22222\" not found"
    }

Dans l'enveloppe retournée, le booléen **success** est à **false** est un ensemble de messages explicitant l'erreur
est ajouté dans le tableau `messages`.

### Version de l'API {#httpapi-ref:4bd0efac-b454-4825-8efe-7dc85019a82c}

Il est nécessaire de préciser la version de l'API pour s'assurer de l'immuabilité
des retours et des entrées.

La version de l'api doit être indiquée dans l'url :

    /api/v1/documents/1234.json

La version de l'api est le chiffre indiqué après la lettre "v".



## Droit d'accès {#httpapi-ref:cc9f6059-0ad6-4372-b82a-d5a1ca3ef6f3}

L'accès aux ressources est contrôlé par les ressources elles-même mais l'utilisation
de l'api est aussi contrôlée de manière générale par l'application "HTTPAPI".

Cette application définit 4 droits (ACL) qui autorisent l'utilisation des
méthodes sur les ressources/collections :

*   `PUT`;
*   `POST`;
*   `GET`;
*   `DELETE`.

Ces droits s'appliquent de manière globale sur toute l'API quelque soit la
ressource/collection concernée.

Si un utilisateur ne possède pas le droit, il ne peut pas effectuer de demande
de ce type.

L'accès aux routes est contrôlé si la configuration de la route indique un 
[droit spécifique][routeadd].


[wikipedia_rest]: https://en.wikipedia.org/wiki/Representational_state_transfer "Source Wikipédia"
[wikipedia_crud]: https://en.wikipedia.org/wiki/Create,_read,_update_and_delete "Source Wikipédia"
[authentification]: #core-ref:b482b82b-ebe2-46e4-8051-c6e83d11a2ae "Mécanisme d'authentification"
[wikipedia_http]: https://fr.wikipedia.org/wiki/Liste_des_codes_HTTP "Source Wikipédia"
[authent_guest]: #core-ref:932e2070-6929-11e2-8218-0021e9fffec1
[coreacls]:      #core-ref:a98b72ea-c063-4907-abc4-e5171ab55e59

[routeadd]:     #httpapi-ref:7466f89c-87de-4dbe-89af-fdc2db37b9a4