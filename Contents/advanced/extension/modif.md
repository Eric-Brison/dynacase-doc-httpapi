# Extension de l'API REST {#httpapi-ref:50aff82b-8921-42ff-81b1-69a1a0103d98}

Il est possible à un module d'étendre l'API REST pour ajouter de nouvelles
ressources ou surcharger les ressources existantes.

L'extension de l'API REST passe par deux éléments :

* une classe de CRUD : celle-ci fournit les 4 méthodes du CRUD et retourne les valeurs.
* une inscription dans la liste des API existantes.

## Classe de CRUD {#httpapi-ref:651043a1-f290-466e-977f-d39a195a1195}

Les classes de CRUD étendent la classe abstraite `\Dcp\HttpApi\V1\Crud\Crud` et
doivent implémenter les méthodes suivantes :

* `create` : création d'une ressource,
* `read` : récupération d'une ressource,
* `update` : mise à jour d'une ressource,
* `delete` : suppression d'une ressource.

Chacune de ces méthodes doit retourner la réponse. Si le type retourné est `json`
le retour doit être un tableau ou un objet implémentant l'interface JsonSerializable.

Dans le cas d'une réponse HTML le résultat doit être une chaîne de caractères
contenant le code html de la page.

L'attribut `urlParameters` contient le résultat de l'interprétation de l'expression
régulière définie dans la route.

Exemple : pour la route

    "regExp" :       "%^/documents/(?P<identifier>[^/]+)/locks/(?P<lockType>(temporary|permanent))$%",

L'url `api/v1/documents/35699/locks/temporary` retourne le résultat

    $this->urlParameter :
    Array
    (
        [0] => /documents/35699/locks/temporary
        [identifier] => 35699
        [1] => 35699
        [lockType] => temporary
        [2] => temporary
        [3] => temporary
    )


L'attribut `contentParameters` contient les données soumises par la requête
sous la forme d'un tableau indexé.

L'extraction des `contentParameters` varie suivant le type de requête :

* `GET` : le `contentParameters` est le contenu du `$_GET`,
* `POST` : le `contentParameters` est :
 * si le `header` `content-type` est `application/json` : le contenu est le `php://input`,
 * si le `header` `content-type` est `x-www-form-urlencoded` ou `form-data` : le contenu est le `$_POST`,
* `PUT` :  le `contentParameters` est :
 * si le `header` `content-type` est `application/json` : le contenu est le `php://input`,
 * si le `header` `content-type` est `x-www-form-urlencoded` ou `form-data` : le contenu est le `php://input` form décodé,
* `DELETE` :  le `contentParameters` est un array vide.


La méthode `::addMessage(RecordReturnMessage $message)` permet d'ajouter un message.

Cette méthode permet d'ajouter un message dans la donnée renvoyée si le type retourné est du json.
Si le type est html, seuls les messages d'erreur sont pris en compte. Dans ce cas
le retour se fait par une page html contenant le message d'erreur.

Exemple de message retourné :

    {
        "success":true,
        "messages":[
            {"type":"message","contentText":"Hello world"},
            {"type":"notice","contentText":"This is a lock request"}],
        "data":{
            "uri":"\/api\/v1\/documents\/35699\/locks\/permanent",
            "lock":null}
    }

La classe fournit aussi quelques autres méthodes surchargeables :

* `setUrlParameters` : cette méthode reçoit les paramètres extraits par le
routeur de l'url d'appel (voir [enregistrement CRUD][save_CRUD]), ces paramètres
sont ensuite mis à disposition dans l'attribut `urlParameters` de type tableau de la classe,

* `setContentParameters` : cette méthode reçoit les paramètres extraits par le
routeur du contenu de la requête, ces paramètres sont ensuite mis à disposition
dans l'attribut `contentParameters` de type tableau de la classe,

* `execute` : cette méthode est celle qui est exécutée par le routeur une fois
la classe CRUD identifiée,

* `getEtagInfo` : cette méthode est utilisée pour calculer un etag dans le cadre
de la gestion du [cache][cache].


## Enregistrement d'une route {#httpapi-ref:7466f89c-87de-4dbe-89af-fdc2db37b9a4}

La nouvelle route de CRUD doit ensuite être enregistrée auprès de l'application
`HTTPAPI_V1`, ceci se fait via l'ajout d'un fichier `json` dans le répertoire
`HTTPAPI_V1/rules.d/`.

Ce paramètre doit contenir un tableau encodé en JSON semblable à :

    [javascript]
    [
        {
            "order" :        300,
            "class" :        "\\Dcp\\HttpApi\\V1\\Crud\\Document",
            "regExp" :       "%^/documents/?(?P<identifier>[^/]+)$%",
            "description" :  "Documents <documentId>",
            "canonicalURL" : "documents/<documentId>"
        },
        {
            "order" :        100,
            "class" :        "\\Dcp\\HttpApi\\V1\\Crud\\DocumentCollection",
            "regExp" :       "%^/documents/$%",
            "description" :  "List of all the documents",
            "canonicalURL" : "documents/"
        },
        {
            "order" :        100,
            "class" :        "\\Dcp\\HttpApi\\V1\\Crud\\Family",
            "regExp" :       "%^/families/(?P<identifier>[^/]+)$%",
            "description" :  "Family <familyId>",
            "canonicalURL" : "families/<familyId>"
        },
        {
            "order" :        100,
            "class" :        "\\Dcp\\HttpApi\\V1\\Crud\\ImageAsset",
            "regExp" :       "%^/images/assets/sizes/(?P<size>[0-9x]+[cfs]?)/(?P<identifier>.+)$%",
            "description" :  "Download resized file image",
            "canonicalURL" : "images/assets/sizes/<width>x<height>/<fileImageName>",
            "standalone" :    true,
            "acceptExtensions" : ["jpg","jpeg","png","gif"]
        }
    ]

Ce tableau contient des objets contenant chacun :

* `regExp` : une expression régulière, celle-ci est exécutée sur  l'URL d'appel
(sans la partie `api/v1/`) si l'expression régulière est validée la route est
considérée comme valide,

* `order` : un ordre d'exécution si plusieurs routes de même ordre sont valides,
c'est celle ayant l'ordre le plus élevé qui est sélectionnée,

* `class` : identifiant d'une classe de type `CRUD` qui est exécutée si la route
 est sélectionnée,

* `description` (optionnel) : chaîne de caractères non traduites décrivant la
ressource (cette clef est utilisée dans la page par défaut de l'API),

* `canonicalURL` (optionnel) : URL d'accès à la ressources (cette clef est
utilisée dans la page par défaut de l'API).

* `standalone` (optionnel) : (booléen) Si la valeur est vrai, l'authentification
n'est pas vérifiée et l'initialisation du contexte Dynacase n'est pas réalisée.
Par défaut, la valeur est faux. La méthode utilisée doit être autonome en terme
 de contrôle d'accès et de traitement.

* `acceptExtensions` (optionnel) : Liste (tableau) indiquant les extensions autorisées.
Par défaut seul l'extension ".json" et ".html" sont autorisées. Il faut dans ce
cas que la méthode indique elle-même le `Content-type` dans le header HTTP.

* `acl` (optionnel) : Indique le [droit applicatif][coreacls] nécessaire pour accéder à la route.
    Si l'objet `{application, acl}` est fourni alors ce droit est vérifié pour
    chacune des méthodes HTTP.  
    Pour indiquer des droits différents par méthode, il faut utiliser la notation
    par méthode.
    Si aucun droit n'est indiqué, alors seul le droit sur la méthode est
    [vérifié][aclrest]. Par contre, si un droit est indiqué, seul ce droit est vérifié.

    [javascript]
    {"GET": {"application":"MYAPP", "acl":"MYGETACL"},
     "PUT": {"application":"MYAPP", "acl":"MYPUTACL"},
     "POST": {"application":"MYAPP", "acl":"MYPOSTACL"},
     "DELETE": {"application":"MYAPP", "acl":"MYDELETEACL"},
    }

<span class="flag inline nota-bene"></span> Si une route à égalité est ajoutée
avec une route `system` (regexp positive et order égal), alors c'est la route
`custom` qui est sélectionnée. Cela permet de surcharger les routes
systèmes.

Une fois le fichier ajouté sur le serveur, la compilation des règles doit être
réalisée en exécutant l'action `INIT_RULES` de l'application `HTTAPI_V1`.

* soit via l'url suivante connecté en admin : `?app=HTTPAPI_V1&action=INIT_RULES`,
* soit via l'appel suivant (dans le info.xml, ou en ligne de commande) `./wsh.php --app=HTTPAPI_V1 --action=INIT_RULES`.

[save_CRUD]: #httpapi-ref:651043a1-f290-466e-977f-d39a195a1195
[cache]: #httpapi-ref:804f8d68-acfa-4a35-bb41-27b2a27c14dc
[aclrest]:  #httpapi-ref:cc9f6059-0ad6-4372-b82a-d5a1ca3ef6f3

[coreacls]:      #core-ref:a98b72ea-c063-4907-abc4-e5171ab55e59
