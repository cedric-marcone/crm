# Interface CRM

## Fourniture du token par la plateforme MseM

L’authentification se fait via notre serveur d'identité OAuth2 en `client_credentials`.

### Origines pour la récupération d’un token d'accès

| Environnement | URL serveur                        |
| ------------- | ---------------------------------- |
| Intégration   | https://auth-integration.msem.tech |
| UAT           | https://auth-uat.msem.tech         |
| Production    | https://auth.msem.tech             |

**Paramétrage**
Pour pouvoir vous connecter nous devrons vous fournir des credentials (client_id & client_secret)

**Requête du endpoint /api/oauth/token**

```http
POST https://auth.msem.tech/api/oauth/token
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&scope=all&client_id={client_id}&client_secret={client_secret}
```

**Réponse du endpoint /api/oauth/token**

```http
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

{
  "access_token": "249443c5703c7c4c35a3c7a87d13d7b505176d2f",
  "token_type": "Bearer",
  "expires_in": 3599,
  "scope": "all"
}
```

L’access_token se récupère dans le corps de la réponse : `response.body.access_token`

## Services exposés par la plateforme MseM

### Intégration

Chaque appel doit fournir un token valide via le header Authorization :

```http
Authorization: Bearer {access_token}
```

Chaque appel consomme et fourni des données au format `JSON` et les chaînes de caractères sont encodées en UTF-8. Vous devrez préciser le Content-Type de manière adéquate :

```http
Content-Type: application/json; charset=utf-8
```

### Origines pour l'invocation des services

| Environnement | URL serveur                            |
| ------------- | -------------------------------------- |
| Intégration   | https://services-integration.msem.tech |
| UAT           | https://services-uat.msem.tech         |
| Production    | https://services.msem.tech             |

**Requête du endpoint /api/crm/crm-to-msem/notification**

```http
POST  https://services.msem.tech/api/crm/crm-to-msem/notification
Content-Type: application/json; charset=utf-8
Authorization: Bearer {access_token}

```

**Réponse du endpoint /api/crm/crm-to-msem/notification**

```http
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

{
 "channel": "LBM",
 "operations": [
   {
     "type": "CreateCustomer",
     "value": {
       "id": 1581190,
       "lastname": "Marcone",
       "firstname": "Cédric",
       "email": "cedric.marcone@valraiso.fr"
     }
   },
   {
     "type": "UpdateCustomer",
     "value": {
       "id": 1581191,
       "lastname": "Rampon",
       "firstname": "Benjamin",
       "email": "benjamin.rampon@valraiso.fr"
     }
   }
 ]
}
```

_Opérations supportées : CreateCustomer, UpdateCustomer, CreateOrder & UpdateOrder_
