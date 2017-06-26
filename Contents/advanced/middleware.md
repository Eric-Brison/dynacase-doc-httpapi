# Middleware {#httpapi-ref:e578fdbe-f6fd-4fa0-9a07-a8207d8757e9}

Les middlewares permettent de modifier le comportement d'une route enregistrée.
Un middleware permet d'exécuter un code spécifique avant ou après l'éxécution du code de la route.


Les middlewares exécutés **avant** le code de la route ne peuvent pas modifier les données retournés par
la route. Ils peuvent ajouter des messages ou modifier les entêtes HTTP de la réponse.

Les middlewares exécutés **après** le code de la route peuvent en plus modifier les données retournées.

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

Les middlewares exécutés pour une requête sont indiqués dans les headers de la 
réponse http avec le mot-clef "`X-Dcp-Middleware`". 
La description du middleware est utilisé comme valeur de cet header.

## Classe de middlewares {#httpapi-ref:774d64c3-9a3f-41b8-8d5d-21b01f90ed9a}

La clef `class` indique la classe de middleware qui doit être exécutée.
Cette classe doit hériter de la classe `Dcp\HttpApi\V1\Crud\MiddleWare`.

Suivant la méthode de la requête http, les méthodes suivantes sont exécutées :

| HTTP Method | Middleware method             |
|:------------|:------------------------------|
| GET         | `void  ::read($identifier)`   |
| POST        | `void  ::create() `           |
| PUT         | `void  ::update($identifier)` |
| DELETE      | `void  ::delete($identifier)` |

La variable `$identifier` correspond à la valeur du paramètre "`identifier`" présent dans l'expression régulière (`regexp`) de la déclaration. Le retour de ces méthodes n'est pas utilisé.

La classe de Middleware dispose de 2 méthodes pour consulter les informations sur la requête et pour manipuler la réponse.

### getHttpRequest {#httpapi-ref:05cdd5f1-9aa9-4b10-b9ee-d0da8255aaf4}

La méthode `getHttpRequest()` retourne un objet `Dcp\HttpApi\V1\Api\HttpRequest`.

Cet objet permet d'accéder aux éléments de la requête reçue.

*   string getMethod() : retourne la méthode de la requête GET, POST, PUT , DELETE 
*   array getHeaders : retourne la liste des entêtes HTTP reçus
*   array getQuery() : retourne les variables de la requête (voir [contentParameters][crudclass])
*   array getUri() : retourne la composition de l'url (idem [parse_url][parseUrl])

### getHttpResponse {#httpapi-ref:0ef3f968-6a18-4929-ba0b-86e52f6423ea}


La méthode `getHttpResponse()` retourne un objet `Dcp\HttpApi\V1\Api\HttpResponse`.

Cet objet permet d'accéder aux éléments de la réponse en cours et permet la modification de celle-ci.

Méthodes d'accès : (*getter*)

*   `mixed getBody()` : retourne les données (partie `data` )à envoyer
*   `mixed getResponse()` : retourne les données personnalisées à envoyer (retourne la valeur donnée par setResponse()).
*   `string getStatusHeader()` : retourne le statut http à envoyer
*   `string[] getHeaders()` : retourne la liste des headers http à envoyer
*   `RecordReturnMessage  getMessages()` : retourne la liste des [messages][message] à envoyer


Méthodes de modification : (*setter*)

*   `addMessage(RecordReturnMessage $message)` : ajouter un [messages][message]
*   `setStatusHeader($httpStatusHeader)` : modifier le status HTTP
*   `addHeader($key, $value, $replace = true)` : ajouter un header http
*   `setBody($data)` : modifie les données (partie `data` de la réponse) à renvoyer
*   `setResponse($response)` : modifie la réponse complète - Écrase la partie `data`.
*   `sendResponse()` : donne l'ordre d'envoi de la réponse, les traitements suivants
    (autres middlewares ou code principal) **sont ignorés**.

L'objet retourné par la  méthode `::getHttpResponse()` est le même pour tous les middlewares.
Le middleware suivant accède à la réponse du middleware précédent.

Attention : l'appel de la route principale écrase les éventuelles affectations 
effectuées par les middlewares "before" au moyen des méthodes `setBody`, `setResponse` et `setStatusHeader`.

Lorsque le contenu est de la donnée `json`, les méthodes `getBody` et `setBody`
manipulent la partie [`data`][jsondata] fournie par les méthodes CRUD.

Pour retourner des données personnalisées, il faut utiliser la méthode `setResponse()`.
Lorsque cette méthode est utilisée, les messages et la partie `data` ne sont pas transmises
dans le message. Pour les inclure, il faut explicitement les ajouter.

    [php]
    $httpResponse->setResponse([
                 "data"=>$httpResponse->getBody(),
                 "messages"=>$httpResponse->getMessages(),
                 "myCustom"=> $myCustomValues
    ]);

Lorsque le contenu est du HTML, ces méthodes manipulent directement la chaîne de caractères
correspondant au code HTML.

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


Construction d'une réponse personnalisée "myCustom" dans la réponse d'un middleware "after".

    [php]
    namespace My;
        
    use Dcp\HttpApi\V1\Crud\MiddleWare;
    use Dcp\HttpApi\V1\DocManager\DocManager as DocManager;
    use Dcp\HttpApi\V1\Api\RecordReturnMessage;
    
    class SayCustomHello extends MiddleWare
    {
        public function read($resourceId)
        {
            $document=DocManager::getDocument($resourceId);
            
            if ($document) {
                $httpResponse=$this->getHttpResponse();
                $body=$httpResponse->getBody();
                if (is_array($body)) {
                    $response["myCustom"] = "Hello world";
                    $response["myData"] = $body;
                    $httpResponse->setResponse($response);
                } 
            }
        }
    }

    Le retour sera de la forme :

    [javascript]
    {
        "myCustom":"Hello world",
        "myData":{
            "uri":"\/api\/v1\/documents\/35699\/locks\/permanent",
            "lock":null}
        }
    }

[crudrecord]:   #httpapi-ref:7466f89c-87de-4dbe-89af-fdc2db37b9a4
[crudclass]:    #httpapi-ref:651043a1-f290-466e-977f-d39a195a1195
[message]:      #httpapi-ref:cde0afab-54a8-4337-8038-be9b335f6b54
[jsondata]:     #httpapi-ref:e082f087-3b94-482c-8938-faee8048f3b1
[parseUrl]:     http://php.net/manual/fr/function.parse-url.php "Fonction parse_url"
