# Middleware {#httpapi-ref:e578fdbe-f6fd-4fa0-9a07-a8207d8757e9}

Les middlewares permettent de modifier le comportement d'une route enregistrée.
Un middleware permet d'exécuter un code spécifique avant ou après l'éxécution du code de la route.


Les middlewares exécutés **avant** le code de la route ne peuvent pas modifier les données retournés par
la route. Ils peuvent ajouter des messages ou modifier les entêtes HTTP de la réponse.

Les middleware exécutés **après** le code de la route peuvent en plus modifier les données retournées.

## Enregistrement d'un middleware {#httpapi-ref:6640b5ba-9c29-468a-90d0-4279bc41c60c}

Le principe d'enregistrement d'un middleware est identique à celui de
l'[enregistrement d'une route][crudrecord].

La différence est que le fichier `json` de description doit être placé dans le répertoire
 `HTTPAPI_V1/rules.d/middleware.d`.

    [javascript]
    [
         {
             "order" :        300,
             "process":       "before"
             "class" :        "\\My\\Message",
             "regExp" :       "%^/documents/?(?P<identifier>[^/\\.]+)%",
             "description" :  "Add message",
             "canonicalURL" : "documents/<documentId>"
         },
         {
             "order" :        100,
             "process":       "after"
             "class" :        "\\My\\SayHello",
             "regExp" :       "%^/documents/?(?P<identifier>[^/\\.]+)$%",
             "description" :  "Say Hello World",
             "canonicalURL" : "documents/"
         },
         {
             "order" :        1000,
             "process":       "before"
             "class" :        "\\My\\Always",
             "regExp" :       "%%",
             "description" :  "Always do process",
             "canonicalURL" : "*"
         }
    ]

La clef `process` doit être indiquée. La valeur `before` indique que le middleware
est exécuté avant la route. La valeur `after` indique que le middleware
est exécuté après la route.

Tous les middlewares dont la `rexExp` correspond à l'url sont exécutés.
L'ordre est donné par la clef `order`. L'ordre d'exécution est du plus grand au
plus petit.

Les middleware exécutés pour une requête sont indiqués dans les headers de la 
réponse http avec le mot-clef "`X-Middleware`".

## Classe de middlewares {#httpapi-ref:774d64c3-9a3f-41b8-8d5d-21b01f90ed9a}

La clef `class` indique la classe de middleware qui doit être exécutée.
Cette classe doit hériter de la classe `Dcp\HttpApi\V1\Crud\MiddleWare`.

Suivant la méthode de la requête http, les méthodes suivantes sont exécutées :

| HTTP Method | Middleware method       |
|:------------|:------------------------|
| GET         | `::read($identifier)`   |
| POST        | `::create() `           |
| PUT         | `::update($identifier)` |
| DELETE      | `::delete($identifier)` |

La variable `$identifier` correspond à la valeur du paramètre "`identifier`" présent dans l'expression régulière (`regexp`) de la déclaration.

La classe de Middleware dispose de 2 méthodes pour consulter les informations sur la requêtes et pour manipuler la réponse.

### getHttpRequest {#httpapi-ref:05cdd5f1-9aa9-4b10-b9ee-d0da8255aaf4}

La méthode `getHttpRequest()` retourne un objet `Dcp\HttpApi\V1\Api\HttpRequest`.

Cet objet permet d'accéder aux éléments de la requête envoyée.

*   string getMethod() : retourne la méthode de la requête GET, POST, PUT , DELETE 
*   array getHeaders : retourne la liste des entêtes HTTP envoyés
*   array getQuery() : retourne les variables de la requête (voir [contentParameters][crudclass])
*   array getUri() : retourne la composition de l'url 

### getHttpResponse {#httpapi-ref:0ef3f968-6a18-4929-ba0b-86e52f6423ea}


La méthode `getHttpResponse()` retourne un objet `Dcp\HttpApi\V1\Api\HttpResponse`.

Cet objet permet d'accéder aux éléments de la réponse en cours et permet la modification de celle-ci.

Méthodes d'accès : (*getter*)

*   `mixed getBody()` : retourne les données à envoyer
*   `string getStatusHeader()` : retourne le statut http à envoyer
*   `string[]` getHeaders() : retourne la liste des headers http à envoyer
*   `RecordReturnMessage  getMessages()` : retourne la liste des messages à envoyer


Méthodes de modification : (*setter*)

*   `addMessage(RecordReturnMessage $message)` : ajouter un message
*   `setStatusHeader($httpStatusHeader)` : modifier le status HTTP
*   `addHeader($key, $value, $replace = true)` : ajouter un header http
*   `setBody($data)` : modifie les données à renvoyer

Les méthodes `setStatusHeader` et `setBody` ne sont pas prises en comptes dans les middlewares "before". En effet, le contenu (body) et le statut http sont fournis par le code de la route. Ces éléments, body et status, peuvent être 
utilisés dans les middleware "after".

### Exemple {#httpapi-ref:3a2e01d9-f354-4c2f-81a6-4e7ff3ab29db}

Ajout d'un message dans un middleware "before".

    [php]
    namespace My;
    
    use Dcp\HttpApi\V1\Crud\MiddleWare;
    use Dcp\HttpApi\V1\DocManager\DocManager as DocManager;
    use Dcp\HttpApi\V1\Api\RecordReturnMessage;
    
    class Message extends MiddleWare
    {
        public function read($resourceId)
        {
            $document=DocManager::getDocument($resourceId);
            
            if ($document) {
                $message=new RecordReturnMessage();
                $message->contentText=sprintf("Loading %s - %s", 
                                                $document->getTitle(), 
                                                date("Y-m-d"));
                $this->getHttpResponse()->addMessage($message);
            }
        }
    }

Ajout d'un champ "myCustom" dans la réponse d'un middleware "after".

    [php]
    namespace My;
        
    use Dcp\HttpApi\V1\Crud\MiddleWare;
    use Dcp\HttpApi\V1\DocManager\DocManager as DocManager;
    use Dcp\HttpApi\V1\Api\RecordReturnMessage;
    
    class SayHello extends MiddleWare
    {
        public function read($resourceId)
        {
            $document=DocManager::getDocument($resourceId);
            
            if ($document) {
                $httpResponse=$this->getHttpResponse();
                $response=$httpResponse->getBody();
                if (is_array($response)) {
                    $response["myCustom"] = "Hello world";
                    $httpResponse->setBody($response);
                } 
            }
        }
    }

Le retour sera de la forme :

    [javascript]
    {
        "success":true,
        "messages":[
            {"type":"message",
            "contentText":"Loading Testing - 2017-03-15"}],
        "data":{
            "uri":"\/api\/v1\/documents\/35699\/locks\/permanent",
            "lock":null,
            "myCustom":"Hello world"}
        }
    }

[crudrecord]:   #httpapi-ref:7466f89c-87de-4dbe-89af-fdc2db37b9a4
[crudclass]:    #httpapi-ref:651043a1-f290-466e-977f-d39a195a1195
