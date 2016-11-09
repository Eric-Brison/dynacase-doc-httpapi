# Authentification {#httpapi-ref:a7d043da-bea5-4338-9a83-c04abd484934}


## Authentification par cookie de session {#httpapi-ref:957a7eca-4830-40f7-a2e0-14c462235341}

L'accès à l'api requiert une session valide préexistante et préalablement
ouverte à l'aide des mécanismes d'authentification de Dynacase (voir [manuel de
référence de Dynacase Core][authentification]).

L'api n'est pas utilisable en mode ["anonyme" ou "invité"][authent_guest].


L'utilisation de l'api dans le cadre d'une interface issue d'une action ne
nécessite pas d'autorisation  particulière. Le cookie de session est transmis
naturellement par le navigateur.


## Authentification par jeton d'authentification {#httpapi-ref:1b2cc6c5-8751-4478-8f03-4cc2272fd5db}

L'accès à l'api peut être autorisé à l'aide d'un [jeton d'authentification][openaccess].

Le jeton peut être indiqué dans le header HTTP :

    GET api/v1/documents/9.json
    Authorization: DcpOpen e2bb65612c70e7ac78d5ccbfe12aa234

Le terme "DcpOpen" est le "HTTP Authentication Scheme" pour identifier le mode
d'autorisation.

Il peut être indiqué aussi dans le paramètre GET `dcpopen-authorization`

    GET api/v1/documents/9.json?dcpopen-authorization=e2bb65612c70e7ac78d5ccbfe12aa234

Si un cookie de session est aussi utilisé, il sera ignoré. Le jeton est
prioritaire sur le cookie de session.

### Obtenir un jeton pour l'api {#httpapi-ref:4b674622-dac8-4008-84f7-f9acaab7345d}

Le jeton d'accès peut être obtenu avec la méthode suivante : 


    [php]
    string Dcp\HttpApi\V1\Authent::getAuthorizationToken(
                                        \Account $userAccount,
                                        array    $routes ,
                                        int      $expireDelay = -1, 
                                        bool     $oneshot = false)


Cette méthode retourne un jeton : une chaîne de caractères composée de 
caractères hexadécimaux (0-9a-f).

$userAccount (Account): Utilisateur identifié pour ce jeton
:   Les groupes ou rôles ne sont pas autorisés.

$expireDelay (int): Délai de validité du jeton en secondes.
:   Mettre "-1" (ou false) pour indiquer un délai infini.

$oneshot (bool): Mettre "true" pour indiquer que le jeton sera valide une seule fois
:   Le jeton est supprimé une fois consommée.

$routes (array): Tableau des routes autorisées
:   Indique sous forme d'expressions régulières les routes autorisées avec ce jeton
    la partie `/api/v1`, ne fait pas partie de la vérification.  
    De manière facultative, la méthode HTTP (GET, PUT, POST, DELETE) peut préfixer
     la route afin de limiter à une méthode donnée. Par défaut, les 4 méthodes sont 
     autorisées.
    Note : un tableau vide n'autorise aucune route.

Une route peut être notée de différentes manières :

*   Simple expression régulière
    *   `%^/documents/[0-9]+(.json)?$%`
*   Expression régulière plus une méthode
    *   `PUT %^/documents/[0-9]+(.json)?$%`
*   Route avec contrainte de paramètre d'url
    
        [
            "route" => "%^/vendor/my/logs$%",
            "methods" => ["GET"],
            "query" => ["level" => "warning"]
        ]

Dans ce dernier exemple, la route vers les logs de niveau "waring" sera autorisée seulement: 

    api/v1/vendor/logs?level=warning

### Exemple de routes {#httpapi-ref:64cb57e1-7352-4b1c-9a2c-6964242d51d4}

Exemple : Construction d'un jeton utilisable à vie pour l'utilisateur "john.doe"

    [php]
    $john=new account();
    $john->setLoginName("john.doe");
    
    $t=Dcp\HttpApi\V1\Authent::getAuthorizationToken($john, ["%.*%"]);
    print $t; // > cad68f8e1848f12df16e44857c595f9a72124f41

Cet exemple autorise toutes les routes à vie pour l'utilisateur "john.doe".

Exemple : Construction d'un jeton utilisable à vie pour l'utilisateur "john.doe"
limité aux données de documents.

    [php]
    $john=new account();
    $john->setLoginName("john.doe");
    
    $t=Dcp\HttpApi\V1\Authent::getAuthorizationToken(
        $john, 
        [
          "%^/documents/[0-9]+(.json)?$%",
          "%^/families/[^/]+/[0-9]+(.json)?$%"
        ]);
    

Ici les routes autorisées peuvent être :

* api/v1/documents/1234
* api/v1/documents/5234.json
* api/v1/families/employee/6234.json


Exemple : Construction d'un jeton utilisable à vie pour l'utilisateur "john.doe"
limité à la consultation des données de documents.

    [php]
    $john=new account();
    $john->setLoginName("john.doe");
    
    $t=Dcp\HttpApi\V1\Authent::getAuthorizationToken(
        $john, 
        [
          "GET %^/documents/[0-9]+(.json)?$%",
          "GET %^/families/[^/]+/[0-9]+(.json)?$%"
        ]);
    


### Utilisation du jeton {#httpapi-ref:fab4ff65-d4f4-47e4-823a-5e2fda837bf1}

En ligne de commande : à l'aide du programme `curl` :

    curl  -H "Authorization: DcpOpen 8e05b60aac3f67447ab4e352a524633839924eda" http://www.example.net/api/v1/documents/12

En ligne de commande : à l'aide du programme `wget` :

    wget http://www.example.net/api/v1/documents/12?dcpopen-authorization=cb713422beb7745f9567710ce011039fabf49224 -O 12.json

Sur un client web en javascript avec [jQuery][jQueryAjax] :

    [javascript]
    var token="8e05b60aac3f67447ab4e352a524633839924eda";
    
    $.ajax({
      dataType: "json",
      url: "api/v1/documents/12",
      headers: {
        "Authorization":"DcpOpen " + token
      }
    }).done(function (data) {
         console.log("Response", data);
    }).fail(function () {
         console.error("Fail", arguments);
    });

En PHP :

    [php]
    $token="8e05b60aac3f67447ab4e352a524633839924eda";
    $url="http://www.example.net/api/v1/documents/12";
    $opts = array(
      'http'=>array(
        'method'=>"GET",
        'header'=>"Authorization: DcpOpen $token\r\n" 
      )
    );
    
    $context = stream_context_create($opts);
    
    $file = file_get_contents($url, false, $context);
    print $file;

## Authentification par login et mot de passe {#httpapi-ref:258b40c9-3689-4e16-82cb-7822226cb0d0}

Cette authentification est basée sur l'authentification [HTTP basic][basichttp].



L'accès peut être effectué en indiquant le login et le mot de passe dans le
header HTTP.

    GET api/v1/documents/9.json
    Authorization: Basic am9obi5kb2U6c2VjcmV0

Dans ce cas, celui-ci doit contenir la méthode utilisée (Basic) suivi de la
représentation en Base64 du nom de l'utilisateur et du mot de passe séparés par
le caractère « : » (deux-points).

Certain programme commL'accès peut être effectué via une url comportant le login est le mot passe

*   GET http://john.doe:secret@example.net/api/v1/documents/1234

Ce type d'authentification ne comporte pas d'autorisation particulière comme
pour les jetons. Toute route est accessible en fonction des droits de
l'utilisateur identifié.

Si un cookie de session est aussi utilisé, il sera ignoré. L'utilisation du
login et du mot de passe est prioritaire.

Par contre, la requête HTTP contient un header "Authorization" avec la méthode
"DcpOpen" et que l'url contient le login et le mot de passe alors, ce sera la
méthode "Dcp" qui sera utilisée et non la méthode "Basic".

Note : On ne peut pas avoir 2 headers "Authorization" dans la requête.

### Utilisation du mode basic {#httpapi-ref:fb33eec7-5913-424a-822e-2b77d7ab6663}

En ligne de commande : à l'aide du programme `curl` :

Avec les informations en clair :

    curl   --user  "john.doe:secret" "http://www.example.net/api/v1/documents/12"
    curl   "http://john.doe:secret@www.example.net/api/v1/documents/12"

Note : Dans ce cas `curl` utilise les informations de connexion pour écrire dans
le header  la clef "Authorization" avec la méthode "basic" d'authentification.
Le programme "wget" ne supporte pas cette notation d'url avec les mots de passe.

Avec les informations encodées.

    curl  -H "Authorization: Basic am9obi5kb2U6c2VjcmV0" http://www.example.net/api/v1/documents/12

Sur un client web en javascript avec [jQuery][jQueryAjax] :

    [javascript]
    var login="john.doe";
    var password="secret";
    
    $.ajax({
      dataType: "json",
      url: "api/v1/documents/12",
      headers: {
        "Authorization":"Basic " + btoa(login + ":" + password)
      }
    }).done(function (data) {
         console.log("Response", data);
    }).fail(function () {
         console.error("Fail", arguments);
    });

En php :

    [php]
    $url="http://john.doe:secret@www.example.net/api/v1/documents/12";
    
    $f=file_get_contents($url);
    print $f;


<!-- links -->
[authentification]: #core-ref:b482b82b-ebe2-46e4-8051-c6e83d11a2ae "Mécanisme d'authentification"
[wikipedia_http]: https://fr.wikipedia.org/wiki/Liste_des_codes_HTTP "Source Wikipédia"
[authent_guest]: #core-ref:932e2070-6929-11e2-8218-0021e9fffec1
[openaccess]:   #core-ref:9edc8f2e-6929-11e2-8610-0021e9fffec1
[basichttp]:    https://fr.wikipedia.org/wiki/Authentification_HTTP
[jQueryAjax]:   http://api.jquery.com/jquery.ajax/  "Jquery Ajax"