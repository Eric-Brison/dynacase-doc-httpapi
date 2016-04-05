# Log {#httpapi-ref:da3b8415-1789-4d9a-9186-c2e97985d3c4}

L'API REST possède un système de logging permettant de logger les cas d'erreur (accès à des ressources inexistantes,
erreur PHP, erreur documentaire, etc.)

## Système de log par défaut {#httpapi-ref:21b1b002-97fb-4159-bb52-b89f9b973aae}

Par défaut le système de log utilise [la classe de log][core_log] de Dynacase et les logs sont enregistrés à chaque
erreur et envoyés dans syslog.

## Ajout d'un système de log tiers {#httpapi-ref:40296f2c-7ba1-4aae-8dd8-2c72b6c4eced}

Il est possible d'ajouter un système de log tiers (mail, outil de supervision, etc.) Il y a deux étapes à suivre  :

### Création d'une classe de Log {#httpapi-ref:94138d08-9c26-4d55-8637-f281620f36da}

Il faut réer une classe qui hérite de classe abstraite de log de l'API `\Dcp\HttpApi\V1\Logger\Logger`.

Ci-dessous un exemple avec une classe (fournie en standard par l'API mais non activée par défaut) qui log les erreurs
dans error.log de Apache.

    [php]
    <?php

    namespace Dcp\HttpApi\V1\Logger;


    class ErrorLog extends Logger
    {

        public function writeError($message, $context = null, $stack = null)
        {
            if ($context === null && \Doc::getUserId()) {
                $context = "User : " . $this->getUserInfo();
            }
            $stack = preg_replace('!\s+!', ' ', $stack);
            $logMessage = sprintf("Error : ## Message : %s ## Context : %s ## Stack : %s", $message, $context, $stack);
            error_log($logMessage);
        }

        public function writeMessage($message, $context)
        {

        }

        public function writeWarning($message, $context = null, $stack = null)
        {
            if ($context === null && \Doc::getUserId()) {
                $context = "User : " . $this->getUserInfo();
            }
            $stack = preg_replace('!\s+!', ' ', $stack);
            $logMessage = sprintf("Warning : ## Message : %s ## Context : %s ## Stack : %s", $message, $context, $stack);
            error_log($logMessage);
        }

        protected function getUserInfo()
        {
            return \Doc::getUserId() . " " . \Doc::getUserName();
        }
    }

La classe ci-dessus log les erreurs de type `Warning` et `Erreur` et ignore les `Message`.

### Enregistrement de la classe de log {#httpapi-ref:f87a6aed-fd61-4f26-83bd-b26a5f8cca85}

La nouvelle classe de log doit ensuite être enregistrée auprès de l'application `HTTPAPI_V1`, ceci se fait via le
paramètre applicatif `CUSTOM_LOGGER`.

Celui doit contenir un array json listant les classes de log que vous souhaitez ajouter, celle-ci seront initialisées
lors d'une requête auprès de l'API et les messages seront envoyés si besoin.

Par exemple, pour enregistrer la classe ci-dessus, il faut ajouter le JSON suivant :

    [javascript]
    ["\\Dcp\\HttpApi\\V1\\Logger\\ErrorLog"]

[core_log]: ../../../dynacase-doc-core-reference/website/book//core-ref:2b8f4534-e749-46ba-b69e-afaa470c4b5c.html
