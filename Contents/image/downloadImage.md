# Téléchargement d'une image publique {#httpapi-ref:6531bf94-1004-45eb-878e-572d80280caa}

## Url   {#httpapi-ref:b03b1518-ddf6-486a-a11e-a11edbb360a1}

Téléchargement d'un fichier image contenu dans le répertoire `Images` ou dans le
coffre en accès public.

### Images enregistrées sur le serveur {#httpapi-ref:43cf5182-eb2e-4759-ad18-40ee9189ae9c}

Pour les images enregistrées sur le serveur, l'url d'accès est : 

*   `api/v1/images/assets/original/<fileName>`

Cette ressource télécharge le fichier image contenu dans le répertoire `Images`
du serveur. Le nom du fichier `<fileName>` est le chemin relatif du fichier à
télécharger. Le chemin est relatif au répertoire `Ìmages` du serveur. Le chemin
ne peut pas contenir `..`.

Les extensions de fichiers autorisées sont `png`, `jpg`, `jpeg` et `gif`.

Exemple :

*   api/v1/images/assets/original/mime-source_php.png
*   api/v1/images/assets/original/flags/fr.png

### Images publiques enregistrées dans le coffre {#httpapi-ref:e297a894-5a8f-4c9a-beb6-6b0d0ec8fbf1}

Pour les images enregistrées dans le [coffre][dbvault]

*   `api/v1/images/recorded/original/<vaultIdentifier>`
*   `api/v1/images/recorded/original/<vaultIdentifier>.<extension>`

Depuis l'interface de consultation d'un document famille, d'un dossier ou d'une
recherche, il est possible de modifier l'icône de celle-ci. Dans ce cas, l'image
est enregistrée dans le coffre en accès public c'est à dire sans nécessité de
vérifier un contrôle sur le document. L'identifiant utilisée doit correspondre à
un identifiant d'une image. Si l'identifiant indique un autre type de fichier,
le téléchargement ne sera pas autorisé.

L'extension du fichier est facultative. Si elle est présente elle doit
correspondre à l'extension du fichier enregistré dans le coffre.

L'identifiant du coffre peut être obtenu par  la récupération des
[informations][getdoc] sur la propriété "icon" du document (ici n°66518).

    GET http://localhost/tmp/api/v1/documents/66518?fields=document.properties

Réponse :

    [javascript]
    {
        "success": true,
        "messages": [],
        "data": {
            "document": {
                "properties": {
                    "id": 66518,
                    "title": "Les ondes gravitationnelles",
                    "icon": "api/v1/images/recorded/sizes/24x24c/3865333998465762597.png",
                    "initid": 66518,
                    "name": null,
                    "revision": 0
                },
                "uri": "/tmp32/api/v1/documents/66518.json"
            }
        }
    }

L'url indiquée dans la caractéristique "icon" est l'image [redimensionnée][resizeimg]. 
L'url de l'image original est :

    api/v1/images/recorded/original/3865333998465762597.png

## Structure de retour   {#httpapi-ref:25b95e0c-5563-4fc2-b52e-dea92728fc30}

Le retour est le contenu du fichier. En cas d'échec, une structure JSON est
retournée avec l'erreur si aucune extension n'est indiquée dans l'url. Si une
extension (différente de `json`) est indiquée alors l'erreur affiché sera un
texte brut.

## Paramètres  {#httpapi-ref:314114a5-5136-403c-8e20-6cd08f73ed3a}

Le paramètre `inline` permet de modifier l'entête HTTP pour demander au
navigateur un téléchargement de fichier ou un affichage dans le navigateur dans
le cas où celui-ci permet son affichage.

Par défaut, le téléchargement est demandé. L'entête contient 

    Content-Disposition: attachment

Si `inline` vaut "true", "1" ou "yes", la disposition vaut :

    Content-Disposition: inline

## Exemple :  {#httpapi-ref:1cf846fe-3b64-4419-b230-4671f35c4fdf}

Récupération des [informations][getdoc] sur la propriété "icon" du document `66518`.

    GET http://localhost/tmp/api/v1/documents/66518?fields=document.properties

Réponse :

    [javascript]
    {
        "success": true,
        "messages": [],
        "data": {
            "document": {
                "properties": {
                    "id": 66518,
                    "title": "Panda",
                    "icon": "api/v1/images/recorded/sizes/24x24c/3865333998465762597.png",
                    "initid": 66518,
                    "name": null,
                    "revision": 0
                },
                "uri": "/tmp32/api/v1/documents/66518.json"
            }
        }
    }

La réponse de la consultation d'un document contient l'url de téléchargement
(dans la propriété `url`) de l'attribut.

Téléchargement de l'icône "mime-pdf.png" associée au type mime "application/pdf":

    GET http://localhost/tmp/api/v1/images/assets/original/mime-pdf.png

Téléchargement de l'original d'une icône enregistrée dans le coffre

    GET http://localhost/tmp/api/v1/images/recorded/original/3865333998465762597.jpg




[getdoc]:           #httpapi-ref:1d7b939f-d5fc-4b57-b33f-d216913efc22
[dbvault]:      #core-ref:7c41b8d8-5ace-489c-886f-a6500c717423
[resizeimg]:    #httpapi-ref:3275f342-b0c8-45db-a736-d987f7c41866