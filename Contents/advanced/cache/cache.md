# Cache {#httpapi-ref:804f8d68-acfa-4a35-bb41-27b2a27c14dc}

L'API inclut un système de cache par [etag][wikipediaEtag].

## Principe de fonctionnement {#httpapi-ref:f1385a83-6bfe-43bf-b974-0caf17e88614}

Les etag sont une partie du standard HTTP 1.1 et permettent de ne pas recalculer les ressources systématiquement.

Le fonctionnement est le suivant :

* Lors de la première requête le serveur envoie en plus des données un identifiant `etag` (via le header `ETag`),
* lors d'une seconde requête le client envoie dans la demande le `etag` précédemment reçu (via le header `If-None-Match`),
* en réponse à la seconde requête le serveur recalcul le `etag` et le compare avec le header `If-None-Match` reçu :
 * si le résultat est positif, la ressource n'a pas été modifiée alors le retour est un status `304 Not Modified` et la ressource n'est pas renvoyée,
 * si le résultat est négatif, la ressource est retournée comme dans le cas de la première requête.

Le cache n'est bien évidemment actif que pour les ressources accédées via la méthode `GET`.

Le cache fait partie du standard HTTP et est implémenté en standard (sans modification à apporter) sur les principaux navigateurs.

##Désactivation du cache {#httpapi-ref:dfde9829-d6d7-492f-b608-60b023b783ae}

Vous pouvez désactiver le cache :
 
* globalement en modifiant le paramètre applicatif `ACTIVATE_CACHE` à false,
* pour une requête en ajoutant le header `Cache-Control: no-cache`.


[wikipediaEtag]: https://fr.wikipedia.org/wiki/Balise-entit%C3%A9_ETag_HTTP