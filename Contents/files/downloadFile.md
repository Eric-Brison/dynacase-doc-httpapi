# Téléchargement d'un fichier de document {#httpapi-ref:674fcf43-a53c-41e0-828d-ab700ff85c77}

## Url  {#httpapi-ref:691835ed-8891-441f-a5dc-942a78d129c1}

Téléchargement d'un fichier contenu dans un attribut de type [`file`][attrfile]
ou [`image`][attrimg]. :

    GET api/v1/documents/<documentId>/files/<attrid>

Pour des attributs inclus dans un tableau, l'index indique le rang dans le tableau

    GET api/v1/documents/<documentId>/files/<attrid>/<index>


Téléchargement du fichier associé à l'attribut `<attrid>` du document `<documentId>`.

L'index  `<index>` facultatif vaut `-1` pour les attributs fichiers non contenu 
dans un tableau.

Dans le cas, d'un attribut inclus dans un tableau, l'index contient le rang dans
le tableau de fichier. 0 indique la première rangée.

Si l'index n'est pas indiqué pour un attribut inclus dans un tableau, alors
l'ensemble des fichiers sera téléchargé dans une archive "zip". Les fichiers
sont alors numérotés suivant l'ordre du tableau.

Ce fichier est extrait du vault.

De manière optionnelle, le nom du fichier peut être indiqué après l'index pour
préciser le fichier mais ce nom n'est pas utilisé pour récupérer le fichier.

Pour accéder au fichier, le droit `view` sur le document est vérifié. De plus,
si l'attribut est en [visibilité][visbility] "I" alors le téléchargement du fichier ne sera
pas autorisé.

## Structure de retour  {#httpapi-ref:af7bc77f-d8a5-4f8e-b74b-8b32e1ed42bc}

### En cas de réussite {#httpapi-ref:289744fe-70f1-4ff7-80bd-120da96a83ed}

Le retour est le contenu du fichier. 

### En cas d'échec  {#httpapi-ref:39d21b01-0359-4cff-bf68-0eafe63c7b80}

En cas d'échec, une structure JSON est
retournée avec l'erreur si aucune extension n'est indiquée dans l'url. Si une
extension (différente de `json`) est indiquée alors l'erreur affiché sera un
texte brut.

Les raisons d'échecs spécifiques à cette requête sont 

|                     Raison                     | Status HTTP | Error Code |
| ---------------------------------------------- | ----------- | ---------- |
| Document non trouvé                            |         404 | CRUD0200   |
| Privilège insuffisant pour accéder au document |         403 | CRUD0201   |
| Document supprimé                              |         404 | CRUD0219   |
| Propriété demandée inexistante                 |         400 | CRUD0202   |
| Attribut demandé inexistant                    |         400 | CRUD0605   |
| Visibilité "I" sur l'attribut                  |         403 | CRUD0606   |
| Attribut n'est pas de type file ou image       |         400 | CRUD0614   |
| Erreur dans l'index                            |         400 | CRUD0610   |
| Erreur dans l'index                            |         400 | CRUD0611   |
| Erreur dans l'index                            |         400 | CRUD0612   |
| Pas de fichier enregistré                      |         404 | CRUD0607   |
| Erreur sur la valeur de l'attribut             |         400 | CRUD0608   |
| Fichier non référencé dans le coffre           |         400 | CRUD0609   |


## Paramètres {#httpapi-ref:2404ad25-a6e7-428d-9756-9d6afe7d6dba}

Le paramètre `inline` permet de modifier l'entête HTTP pour demander au
navigateur un téléchargement de fichier ou un affichage dans le navigateur dans
le cas où celui-ci permet son affichage.

Par défaut, le téléchargement est demandé. L'entête contient 

    Content-Disposition: attachment

Si `inline` vaut "true", "1" ou "yes", la disposition vaut :

    Content-Disposition: inline

## Exemple : {#httpapi-ref:1d44910f-297b-48b2-86f1-f0faef759946}

Récupération des [informations][getdoc] sur l'attribut `my_file` du document `41429`.

    GET /tmp/api/v1/documents/41429?fields=document.attributes.my_file

Réponse :

    [javascript]
    {
        "success": true,
        "messages": [],
        "data": {
            "document": {
                "attributes": {
                    "my_file": {
                        "size": "459285",
                        "creationDate": "2016-05-09 09:11:58",
                        "fileName": "web api.pdf",
                        "url": "api/v1/documents/41429/files/my_file/-1/web%20api.pdf",
                        "mime": "application/pdf",
                        "icon": "api/v1/images/assets/sizes/20x20c/mime-pdf.png",
                        "value": "application/pdf|4090445749180739423|web api.pdf",
                        "displayValue": "web api.pdf"
                    }
                },
                "uri": "/tmp/api/v1/documents/41429.json"
            }
        }
    }

La réponse de la consultation d'un document contient l'url de téléchargement
(dans la propriété `url`) de l'attribut.

Téléchargement  :

    GET http://localhost/tmp/api/v1/documents/41429/files/my_file

ou avec le nom du fichier en option et pour un affichage web: 

    GET http://localhost/tmp/api/v1/documents/41429/files/my_file/-1/web%20api.pdf?inline=1


[update_doc]:       #httpapi-ref:db2cb01a-7325-4f78-8cec-ceac9858caf2
[create_document]:  #httpapi-ref:e769b476-0033-407c-b453-4e8466e09975
[formData]:         https://developer.mozilla.org/en-US/docs/Web/Guide/Using_FormData_Objects "Utiliser les objets FormData"
[attrfile]:         #core-ref:0e904376-317c-426e-bc6d-e56fd52bad89
[attrimg]:          #core-ref:4fca7712-59e0-4186-bfd0-6214104a0f60
[getdoc]:           #httpapi-ref:1d7b939f-d5fc-4b57-b33f-d216913efc22
[visbility]:        #core-ref:3e67d45e-1fed-446d-82b5-ba941addc7e8