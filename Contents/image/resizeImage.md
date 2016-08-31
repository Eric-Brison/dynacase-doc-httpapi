# Redimensionnement d'une image  {#httpapi-ref:3275f342-b0c8-45db-a736-d987f7c41866}

## Url    {#httpapi-ref:af2a4884-4481-48b0-9db6-bb6a80ce23b9}

Téléchargement d'une image redimensionnée.


<span class="flag inline nota-bene"></span> Le redimensionnement ne peux excéder
 la taille de l'image originale.

### Images enregistrées sur le serveur  {#httpapi-ref:da05edb4-7ffd-4758-a3e1-ca683ee7222e}

Pour les images enregistrées sur le serveur, l'url d'accès est : 

*   `api/v1/images/assets/sizes/<sizes>/<fileName>`

Cette ressource télécharge l'image **redimensionnée** du fichier image original
contenu dans le répertoire `Images` du serveur. Le nom du fichier `<fileName>`
est le chemin relatif du fichier à télécharger. Le chemin est relatif au
répertoire `Ìmages` du serveur. Le chemin ne peut pas contenir `..`.

Les extensions de fichiers autorisées sont `png`, `jpg`, `jpeg` et `gif`.

Le type du fichier retourné est le même que celui de l'original.

Exemple :

*   api/v1/images/assets/original/mime-source_php.png
*   api/v1/images/assets/original/flags/fr.png

### Images publiques enregistrées dans le coffre  {#httpapi-ref:30654763-84eb-4652-bffb-87e16ad8d82d}

Pour les images [publiques][vaultpublic] enregistrées dans le [coffre][dbvault]

*   `api/v1/images/recorded/sizes/<sizes>/<vaultIdentifier>.<extension>`


L'extension du fichier est obligatoire. Les valeurs autorisées sont `png`, `jpg`
et `gif`. 

Le fichier retourné correspond à l'image originale redimensionnée et
**convertie** en fonction de l'extension.


### Attribut image des documents  {#httpapi-ref:14b593b6-2056-4804-8f4f-df28a9bdaef6}

Redimensionnement d'une image incluse dans un attribut "image" d'un document.

*    `api/v1/documents/<documentId>/images/<attributeId>/<index>/sizes/<size>.<extension>`

Paramètres :

*   `<documentId>` : Identifiant du document
*   `<attributeId>` : Identifiant de l'attribut image
*   `<index>` : Vaut `-1` si l'attribut n'est pas dans un tableau, 
    indique la rangée du tableau dans le cas contraire (0 indique la première rangée).
*   `<size>` : La taille demandée
*   `<extension>` : Extension du fichier produit

L'extension du fichier est obligatoire. Les valeurs autorisées sont `png`, `jpg`
et `gif`. 

Le fichier retourné correspond à l'image originale redimensionnée et
**convertie** en fonction de l'extension.

Cette url est notamment utilisée pour afficher les miniatures (thumbnail) des
attributs images.

Exemple : informations sur l'attribut image "es_photo" du document n°66518.

    GET api/v1/documents/66518?fields=document.attributes.es_photo

    [javascript]
    {
        "success": true,
        "messages": [],
        "data": {
            "document": {
                "attributes": {
                    "es_photo": {
                        "thumbnail": "api/v1/documents/66518/images/es_photo/-1/sizes/48.png",
                        "size": "317762",
                        "creationDate": "2016-08-29 08:24:05",
                        "fileName": "panda-bambou-alimentation.jpg",
                        "url": "api/v1/documents/66518/files/es_photo/-1/panda-bambou-alimentation.jpg",
                        "mime": "image/jpeg",
                        "icon": "api/v1/images/assets/sizes/24x24c/mime-image.png",
                        "value": "image/jpeg|7168768396445052131|panda-bambou-alimentation.jpg",
                        "displayValue": "panda-bambou-alimentation.jpg"
                    }
                },
                "uri": "/tmp/api/v1/documents/66518.json"
            }
        }
    }

Cas particulier, l’icône du document peut être adressée avec cette url en utilisant
comme nom d'attribut `icon`.

    GET api/v1/documents/66518/images/icon/-1/sizes/48x48c.png

Cela permet notamment d'accéder à des icônes qui ne serait pas publiques. Cela
est le cas pour les documents qui ont des icônes spécifiques en fonction des
attributs images.

## Syntaxe pour le redimensionnement {#httpapi-ref:e64cf3e4-8401-4675-898c-8eb698a5541c}

Le paramètre `<size>` permet d'indiquer la nouvelle taille avec la syntaxe suivante :

Soit l'image originale :

    GET api/v1/images/assets/original/sky.jpg

![ Image de référence (600x338px)](sky.jpg "Image originale")

### Définir une largeur  : `<width>` indique la largeur (en pixel) {#httpapi-ref:b90a5c29-1c03-41d5-af7c-aefefa88d1f5}

La hauteur est calculée pour garder la proportion de l'image.

    GET api/v1/images/assets/sizes/200/sky.jpg

![ Image 200x113px ](Images_200-sky.jpg "Size 200")

### Définir une hauteur  : `x<height>` indique la hauteur (en pixel) {#httpapi-ref:160cf4ec-daf1-4229-825a-a48f20595e21}

La largeur est calculée pour garder la proportion de l'image.

    GET api/v1/images/assets/sizes/x200/sky.jpg

![ Image 355x200px ](Images_x200-sky.jpg "Size x200")


### Définir la largeur et la hauteur  : `<width>x<height>`  {#httpapi-ref:11d2da18-10d0-4166-97a6-9e28f6a99db0}

L'image est redimensionnée pour contenir dans la dimension indiquée tout en
gardant sa proportion. 

    GET api/v1/images/assets/sizes/200x200/sky.jpg

![ Image 200x113px ](Images_200-sky.jpg "Size 200x200")

    GET api/v1/images/assets/sizes/200x100/sky.jpg

![ Image 178x100px ](Images_200x100-sky.jpg "Size 200x100")

### Définir la largeur et la hauteur en découpant l'image : `<width>x<height>c` {#httpapi-ref:0e463edb-e78f-433a-b34a-fc7f7479f297}

Pour que l'image contienne dans les exactes dimensions fournies, cette option
permet de rogner (crop) l'image. Ce rognage est centré, les bords (haut / bas) ou
(droite /gauche) sont supprimés avec la même proportion.

    GET api/v1/images/assets/sizes/50x200c/sky.jpg

![ Image 50x200px ](Images_50x200c-sky.jpg "Size 50x200c")

    GET api/v1/images/assets/sizes/200x50c/sky.jpg

![ Image 200x50px ](Images_200x50c-sky.jpg "Size 200x50c")


### Définir la largeur et la hauteur en étirant l'image : `<width>x<height>s`  {#httpapi-ref:2d175df6-a627-4405-9697-a211b1452766}

L'image est déformée (strech) pour contenir dans la dimension indiquée.

    GET api/v1/images/assets/sizes/50x200s/sky.jpg

![ Image 50x200px ](Images_50x200s-sky.jpg "Size 50x200s")

    GET api/v1/images/assets/sizes/200x50s/sky.jpg

![ Image 200x50px ](Images_200x50s-sky.jpg "Size 200x50s")

## Structure de retour     {#httpapi-ref:0428c85e-d246-44a7-9fa7-ee6c960778a9}

Le retour est le contenu du fichier. En cas d'échec, une structure JSON est
retournée avec l'erreur si aucune extension n'est indiquée dans l'url. Si une
extension (différente de `json`) est indiquée alors l'erreur affiché sera un
texte brut.

## Paramètres    {#httpapi-ref:fd632cbe-dd62-45f5-9d49-36728bfdc8a7}

Le paramètre `inline` permet de modifier l'entête HTTP pour demander au
navigateur un téléchargement de fichier ou un affichage dans le navigateur dans
le cas où celui-ci permet son affichage.

Par défaut, le téléchargement est demandé. L'entête contient 

    Content-Disposition: attachment

Si `inline` vaut "true", "1" ou "yes", la disposition vaut :

    Content-Disposition: inline







[getdoc]:           #httpapi-ref:1d7b939f-d5fc-4b57-b33f-d216913efc22
[dbvault]:      #core-ref:7c41b8d8-5ace-489c-886f-a6500c717423
[vaultpublic]:  #httpapi-ref:e297a894-5a8f-4c9a-beb6-6b0d0ec8fbf1