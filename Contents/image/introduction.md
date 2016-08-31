# Images {#httpapi-ref:5f29912a-6056-44da-9946-cd8c6b2ef707}

Cette collection décrit l'accès aux images publiques notamment les icônes.

## URL  {#httpapi-ref:e803ca05-6541-4313-affc-ea5afea54c88}

Pour les images enregistrées sur le serveur, l'url d'accès est : 

*   `api/v1/images/assets/original/<fileName>`

Pour les images enregistrées dans le [coffre][dbvault]

*   `api/v1/images/recorded/original/<vaultIddentifier>`

## Méthodes   {#httpapi-ref:21a60377-1256-44e9-852f-69825a20c09d}

La collection images implémente les éléments suivants :

* Collection :

| Action   | URL                                             | Action effectuée                                                                                 |
| :-     : | :                                :              | :                                                :                                               |
| `GET`    | `api/v1/images/assets/original/<fileName>`      | [Télécharge l'image <fileName>][download_img]                                                    |
| `GET`    | `api/v1/images/recorded/original/<vaultId>`     | [Télécharge l'image libre identifié par <vaultId> depuis le coffre[download_img]                 |
| `GET`    | `api/v1/images/assets/sizes/<size>/<fileName>`  | [Télécharge l'image redimensionnée <fileName>][download_img]                                     |
| `GET`    | `api/v1/images/recorded/sizes/<size>/<vaultId>` | [Télécharge l'image libre redimensionnée identifié par <vaultId> depuis le coffre][download_img] |
| `POST`   | `api/v1/images/*`                               | N/A                                                                                              |
| `PUT`    | `api/v1/images/*`                               | N/A                                                                                              |
| `DELETE` | `api/v1/images/*`                               | N/A                                                                                              |



<!-- links -->
[download_img]: #httpapi-ref:6531bf94-1004-45eb-878e-572d80280caa
[dbvault]:      #core-ref:7c41b8d8-5ace-489c-886f-a6500c717423