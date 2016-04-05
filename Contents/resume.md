# Résumé de l'API {#httpapi-ref:39742615-6b36-4b23-8449-3a9019b9afa8}

## Carte de l'api {#httpapi-ref:cf8c475b-1ea5-474d-bc74-f5902f9d00fb}

|  Type  |                                      URL                                      |    Implanté    |                            Signification                             |
| ------ | ----------------------------------------------------------------------------- | -------------- | -------------------------------------------------------------------- |
| GET    | */api/v1/documents/*                                                          | Oui            | [Liste de documents][get_documents]                                  |
| PUT    | */api/v1/documents/*                                                          | N/A            | N/A                                                                  |
| POST   | */api/v1/documents/*                                                          | N/A            | N/A                                                                  |
| DELETE | */api/v1/documents/*                                                          | N/A            | N/A                                                                  |
|        |                                                                               |                |                                                                      |
| GET    | **/api/v1/documents/&lt;docid&gt;**                                           | Oui            | [Récupération d'un document][GETDOC]                                 |
| PUT    | **/api/v1/documents/&lt;docid&gt;**                                           | Oui            | [Mise à jour d'un document][PUTDOC]                                  |
| POST   | **/api/v1/documents/&lt;docid&gt;**                                           | N/A            | N/A                                                                  |
| DELETE | **/api/v1/documents/&lt;docid&gt;**                                           | Oui            | [Mise à la poubelle d'un document][DELDOC]                           |
|        |                                                                               |                |                                                                      |
| GET    | */api/v1/documents/&lt;docid&gt;/history/*                                    | Oui            | [Récupération des messages d'historique][get_histo]                  |
| PUT    | */api/v1/documents/&lt;docid&gt;/history/*                                    | N/A            | N/A                                                                  |
| POST   | */api/v1/documents/&lt;docid&gt;/history/*                                    | Future version | Ajout d'un message d'historique                                      |
| DELETE | */api/v1/documents/&lt;docid&gt;/history/*                                    | N/A            | N/A                                                                  |
|        |                                                                               |                |                                                                      |
| GET    | */api/v1/documents/&lt;docid&gt;/revisions/*                                  | Oui            | [Liste des révisions d'un document][list_revision]                   |
| PUT    | */api/v1/documents/&lt;docid&gt;/revisions/*                                  | N/A            | N/A                                                                  |
| POST   | */api/v1/documents/&lt;docid&gt;/revisions/*                                  | N/A            | N/A                                                                  |
| DELETE | */api/v1/documents/&lt;docid&gt;/revisions/*                                  | N/A            | N/A                                                                  |
|        |                                                                               |                |                                                                      |
| GET    | **/api/v1/documents/&lt;docid&gt;/revisions/&lt;revisionNumber&gt;**          | Oui            | [Révision `<revisionNumber>`][get_revision]                          |
| PUT    | **/api/v1/documents/&lt;docid&gt;/revisions/&lt;revisionNumber&gt;**          | N/A            | N/A                                                                  |
| POST   | **/api/v1/documents/&lt;docid&gt;/revisions/&lt;revisionNumber&gt;**          | N/A            | N/A                                                                  |
| DELETE | **/api/v1/documents/&lt;docid&gt;/revisions/&lt;revisionNumber&gt;**          | N/A            | N/A                                                                  |
|        |                                                                               |                |                                                                      |
| GET    | */api/v1/families/*                                                           | Oui            | Liste des familles                                                   |
| PUT    | */api/v1/families/*                                                           | N/A            | N/A                                                                  |
| POST   | */api/v1/families/*                                                           | Future version | Création d'une nouvelle famille                                      |
| DELETE | */api/v1/families/*                                                           | N/A            | N/A                                                                  |
|        |                                                                               |                |                                                                      |
| GET    | **/api/v1/families/&lt;family&gt;**                                           | Oui            | [Consultation du document décrivant la famille][GET_FAM]             |
| PUT    | **/api/v1/families/&lt;family&gt;**                                           | Future version | Modification de la configuration de la famille                       |
| POST   | **/api/v1/families/&lt;family&gt;**                                           | N/A            | N/A                                                                  |
| DELETE | **/api/v1/families/&lt;family&gt;**                                           | Future version | Suppression de la famille et des documents associés                  |
|        |                                                                               |                |                                                                      |
| GET    | */api/v1/families/&lt;family&gt;/documents/*                                  | Oui            | [Liste des documents de cette famille][fam_list_document]            |
| PUT    | */api/v1/families/&lt;family&gt;/documents/*                                  | Future version | Modification en masse de documents de cette famille                  |
| POST   | */api/v1/families/&lt;family&gt;/documents/*                                  | Oui            | [Création d'un document de cette famille][POSTDOC]                   |
| DELETE | */api/v1/families/&lt;family&gt;/documents/*                                  | Future version | Mise à la poubelle en masse des documents de cette famille           |
|        |                                                                               |                |                                                                      |
| GET    | **/api/v1/families/&lt;family&gt;/documents/&lt;docid&gt;**                   | Oui            | [Récupération d'un document de la famille][GETDOC]                   |
| PUT    | **/api/v1/families/&lt;family&gt;/documents/&lt;docid&gt;**                   | Oui            | [Mise à jour d'un document de la famille][PUTDOC]                    |
| POST   | **/api/v1/families/&lt;family&gt;/documents/&lt;docid&gt;**                   | N/A            | N/A (Création d'un document avec un identifiant donné)               |
| DELETE | **/api/v1/families/&lt;family&gt;/documents/&lt;docid&gt;**                   | Oui            | [Suppression d'un document de la famille][DELDOC]                    |
|        |                                                                               |                |                                                                      |
| GET    | */api/v1/families/&lt;family&gt;/enumerates/*                                 | Oui            | [Récupération de la liste des énumérés][get_enum_list]               |
| PUT    | */api/v1/families/&lt;family&gt;/enumerates/*                                 | N/A            | N/A                                                                  |
| POST   | */api/v1/families/&lt;family&gt;/enumerates/*                                 | N/A            | N/A                                                                  |
| DELETE | */api/v1/families/&lt;family&gt;/enumerates/*                                 | N/A            | N/A                                                                  |
|        |                                                                               |                |                                                                      |
| GET    | **/api/v1/families/&lt;family&gt;/enumerates/&lt;docid&gt;**                  | Oui            | [Récupération de la liste des valeurs][get_enum]                     |
| PUT    | **/api/v1/families/&lt;family&gt;/enumerates/&lt;docid&gt;**                  | Future version | Modification d'une valeur de la liste des énumérés                   |
| POST   | **/api/v1/families/&lt;family&gt;/enumerates/&lt;docid&gt;**                  | Future version | Ajout d'une valeur à la liste d'énumérés                             |
| DELETE | **/api/v1/families/&lt;family&gt;/enumerates/&lt;docid&gt;**                  | Future version | Suppression d'un énuméré                                             |
|        |                                                                               |                |                                                                      |
| GET    | */api/v1/temporaryFiles/*                                                     | N/A            | N/A                                                                  |
| PUT    | */api/v1/temporaryFiles/*                                                     | N/A            | N/A                                                                  |
| POST   | */api/v1/temporaryFiles/*                                                     | Oui            | [Création d'un fichier temporaire][POSTFILE]                         |
| DELETE | */api/v1/temporaryFiles/*                                                     | Future version | Suppression d'un fichier temporaire                                  |
|        |                                                                               |                |                                                                      |
| GET    | */api/v1/trash/*                                                              | Oui            | [Liste des documents de la poubelle][get_trash]                      |
| PUT    | */api/v1/trash/*                                                              | N/A            | N/A                                                                  |
| POST   | */api/v1/trash/*                                                              | N/A            | N/A                                                                  |
| DELETE | */api/v1/trash/*                                                              | Future version | Supprime définitivement les documents de la poubelle                 |
|        |                                                                               |                |                                                                      |
| GET    | **/api/v1/trash/&lt;docid&gt;**                                               | Oui            | [Récupération d'un document mis à la poubelle][trash_doc]            |
| PUT    | **/api/v1/trash/&lt;docid&gt;**                                               | N/A            | N/A                                                                  |
| POST   | **/api/v1/trash/&lt;docid&gt;**                                               | N/A            | N/A                                                                  |
| DELETE | **/api/v1/trash/&lt;docid&gt;**                                               | Future version | Suppression définitive d'un document mis à la poubelle               |
|        |                                                                               |                |                                                                      |
| GET    | */api/v1/folders/*                                                            | Oui            | [Liste de documents de type dossier][folders_collection]             |
| PUT    | */api/v1/folders/*                                                            | N/A            | N/A                                                                  |
| POST   | */api/v1/folders/*                                                            | N/A            | N/A                                                                  |
| DELETE | */api/v1/folders/*                                                            | N/A            | N/A                                                                  |
|        |                                                                               |                |                                                                      |
|        |                                                                               |                |                                                                      |
| GET    | */api/v1/folders/&lt;folderId&gt;*                                            | Future version | Description du dossier                                               |
| PUT    | */api/v1/folders/&lt;folderId&gt;*                                            | N/A            | N/A                                                                  |
| POST   | */api/v1/folders/&lt;folderId&gt;*                                            | N/A            | N/A                                                                  |
| DELETE | */api/v1/folders/&lt;folderId&gt;*                                            | N/A            | N/A                                                                  |
|        |                                                                               |                |                                                                      |
| GET    | */api/v1/folders/&lt;folderId&gt;/documents/*                                 | Oui            | [Contenu du dossier folderId][folders_content]                       |
| PUT    | */api/v1/folders/&lt;folderId&gt;/documents/*                                 | N/A            | N/A                                                                  |
| POST   | */api/v1/folders/&lt;folderId&gt;/documents/*                                 | Future version | Ajout d'un document au dossier                                       |
| DELETE | */api/v1/folders/&lt;folderId&gt;/documents/*                                 | Future version | Suppression d'un document du dossier                                 |
|        |                                                                               |                |                                                                      |
| GET    | */api/v1/searches/*                                                           | Oui            | [Liste de documents de type recherche][searches_collection]          |
| PUT    | */api/v1/searches/*                                                           | N/A            | N/A                                                                  |
| POST   | */api/v1/searches/*                                                           | N/A            | N/A                                                                  |
| DELETE | */api/v1/searches/*                                                           | N/A            | N/A                                                                  |
|        |                                                                               |                |                                                                      |
| GET    | */api/v1/searches/&lt;searchId&gt;*                                           | Future version | Description de la recherche                                          |
| PUT    | */api/v1/searches/&lt;searchId&gt;*                                           | N/A            | N/A                                                                  |
| POST   | */api/v1/searches/&lt;searchId&gt;*                                           | N/A            | N/A                                                                  |
| DELETE | */api/v1/searches/&lt;searchId&gt;*                                           | N/A            | N/A                                                                  |
|        |                                                                               |                |                                                                      |
| GET    | */api/v1/searches/&lt;searchId&gt;/documents/*                                | Oui            | [Contenu d'une recherche][searches_content]                          |
| PUT    | */api/v1/searches/&lt;searchId&gt;/documents/*                                | N/A            | N/A                                                                  |
| POST   | */api/v1/searches/&lt;searchId&gt;/documents/*                                | N/A            | N/A                                                                  |
| DELETE | */api/v1/searches/&lt;searchId&gt;/documents/*                                | N/A            | N/A                                                                  |
|        |                                                                               |                |                                                                      |
| GET    | */api/v1/documents/&lt;docid&gt;/usertags/*                                   | Oui            | [Liste des tags utilisateurs d'un document][get_usertag_list]        |
| PUT    | */api/v1/documents/&lt;docid&gt;/usertags/*                                   | N/A            | N/A                                                                  |
| POST   | */api/v1/documents/&lt;docid&gt;/usertags/*                                   | N/A            | N/A                                                                  |
| DELETE | */api/v1/documents/&lt;docid&gt;/usertags/*                                   | N/A            | N/A                                                                  |
|        |                                                                               |                |                                                                      |
| GET    | **/api/v1/documents/&lt;docid&gt;/usertags/&lt;tagIdentifier&gt;**            | Oui            | [Tag utilisateur `<tagIdentifier>`][get_usertag]                     |
| PUT    | **/api/v1/documents/&lt;docid&gt;/usertags/&lt;tagIdentifier&gt;**            | Oui            | [Modification d'un tag utilisateur `<tagIdentifier>`][put_usertag]   |
| POST   | **/api/v1/documents/&lt;docid&gt;/usertags/&lt;tagIdentifier&gt;**            | Oui            | [Création d'un tag utilisateur `<tagIdentifier>`][post_usertag]      |
| DELETE | **/api/v1/documents/&lt;docid&gt;/usertags/&lt;tagIdentifier&gt;**            | Oui            | [Suppression d'un tag utilisateur `<tagIdentifier>`][delete_usertag] |
|        |                                                                               |                |                                                                      |
| GET    | */api/v1/documents/&lt;docid&gt;/workflows/states/*                           | Oui            | [Liste des états possibles d'un document][get_states]                |
| PUT    | */api/v1/documents/&lt;docid&gt;/workflows/states/*                           | N/A            | N/A                                                                  |
| POST   | */api/v1/documents/&lt;docid&gt;/workflows/states/*                           | N/A            | N/A                                                                  |
| DELETE | */api/v1/documents/&lt;docid&gt;/workflows/states/*                           | N/A            | N/A                                                                  |
|        |                                                                               |                |                                                                      |
| GET    | **/api/v1/documents/&lt;docid&gt;/workflows/states/&lt;state&gt;**            | Oui            | [Caractéristiques de l'état][get_state]                              |
| PUT    | **/api/v1/documents/&lt;docid&gt;/workflows/states/&lt;state&gt;**            | N/A            | N/A                                                                  |
| POST   | **/api/v1/documents/&lt;docid&gt;/workflows/states/&lt;state&gt;**            | Oui            | [Changement d'état d'un document `<state>`][create_state]            |
| DELETE | **/api/v1/documents/&lt;docid&gt;/workflows/states/&lt;state&gt;**            | N/A            | N/A                                                                  |
|        |                                                                               |                |                                                                      |
| GET    | */api/v1/documents/&lt;docid&gt;/workflows/transitions/*                      | Oui            | [Liste des transitions possibles d'un document][get_transitions]     |
| PUT    | */api/v1/documents/&lt;docid&gt;/workflows/transitions/*                      | N/A            | N/A                                                                  |
| POST   | */api/v1/documents/&lt;docid&gt;/workflows/transitions/*                      | N/A            | N/A                                                                  |
| DELETE | */api/v1/documents/&lt;docid&gt;/workflows/transitions/*                      | N/A            | N/A                                                                  |
|        |                                                                               |                |                                                                      |
| GET    | **/api/v1/documents/&lt;docid&gt;/workflows/transitions/&lt;transitions&gt;** | Oui            | [Caractéristiques de la transition][get_transition]                  |
| PUT    | **/api/v1/documents/&lt;docid&gt;/workflows/transitions/&lt;transitions&gt;** | N/A            | N/A                                                                  |
| POST   | **/api/v1/documents/&lt;docid&gt;/workflows/transitions/&lt;transitions&gt;** | Oui            | [Passage d'une transition][create_transition]                        |
| DELETE | **/api/v1/documents/&lt;docid&gt;/workflows/transitions/&lt;transitions&gt;** | N/A            | N/A                                                                  |
|        |                                                                               |                |                                                                      |










Légende :

* Les URL en *italique* font références à des collections,
* Les URL en **gras** font références à des ressources,

<span class="flag inline nota-bene"></span> Les entrées documents de famillies et de trash possède aussi les sous-collections :

* history,
* revisions.

<span class="flag inline nota-bene"></span> Les entrées en Future version sont prévues pour une implémentation future 
mais non présentes dans la version courante de l'API.

<!--links-->

[get_usertag_list]:     #httpapi-ref:4d01fe57-4cd9-4989-adb8-f088762e4236
[get_usertag]:          #httpapi-ref:1ecfb964-e7ff-4adc-b874-f6887b800fc5
[post_usertag]:         #httpapi-ref:8f415def-6741-49a9-b0dd-fb7824c648be
[put_usertag]:          #httpapi-ref:bcca7517-ecff-4e8a-8339-e8defa71bc4c
[delete_usertag]:       #httpapi-ref:433d5c93-a890-4da2-b0a9-3e4089c14d3d

[GET_FAM]: #httpapi-ref:6b195156-0cda-47c8-9a9a-04ec13562c9a
[POSTDOC]: #httpapi-ref:e769b476-0033-407c-b453-4e8466e09975
[GETDOC]: #httpapi-ref:1d7b939f-d5fc-4b57-b33f-d216913efc22
[PUTDOC]: #httpapi-ref:db2cb01a-7325-4f78-8cec-ceac9858caf2
[DELDOC]: #httpapi-ref:3358b3bd-bdf6-44ef-b1d7-438f8eb21067

[POSTFILE]: #httpapi-ref:5797255d-128d-4aa4-9c11-2c8195cca63d
[trash_doc]: #httpapi-ref:52be10c1-9f46-456b-a22f-24909386567f
[get_enum]: #httpapi-ref:bb13e401-1859-4c73-b299-70b801ed7eb0
[get_histo]: #httpapi-ref:9ae75dac-8eb5-4fa9-a608-7313b90fe33c
[list_revision]: #httpapi-ref:2dd5afbe-1d3d-4830-8241-c93077d88430
[get_revision]: #httpapi-ref:eb7b6954-0945-4f02-8e10-16e69729c529
[get_enum_list]: #httpapi-ref:69fba1cf-5754-4189-ac07-c16f348e7fda
[get_documents]: #httpapi-ref:2ee6dd78-5b5a-4e00-aba5-4cd85c8a1cdc
[get_trash]: #httpapi-ref:4052b9db-d36c-4535-809f-1fad107e8270
[searches_collection]: #httpapi-ref:9b8f4a2b-3f56-4b21-a7b7-bb299b4ac7b3
[searches_content]: #httpapi-ref:b7fb15e0-6e51-4ace-8a5f-aec7d565e24d
[folders_collection]: #httpapi-ref:b3f83d12-4ea7-44e2-8509-1145d05003d6
[folders_content]: #httpapi-ref:f3e3869b-4dcf-40ee-9733-3f4b57e2386f
[fam_list_document]: #httpapi-ref:f21d3f3f-82ea-48a9-bb9e-ba986bae9b62
[get_states]:       #httpapi-ref:af743cfe-c089-4706-a5fb-a131f68020d2
[get_transitions]:  #httpapi-ref:a91dc2b7-3248-452a-b51e-3f660d7d3cf2
[get_state]:        #httpapi-ref:89142988-9b2b-42f6-af33-f68749c7af35
[create_state]:     #httpapi-ref:f3d33034-af23-48c6-b535-e609266e5bc5
[get_transition]:   #httpapi-ref:d370f800-2a17-4589-90ae-d505b5f71c71
[create_transition]:#httpapi-ref:697b7714-d986-4ae5-8020-a5602cfbe7d5