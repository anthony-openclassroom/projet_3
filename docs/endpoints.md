# Endpoint ChaTop

## Base URL

The base URL for all API requests is:

```txt
/api
```

## Status codes

- **200 OK**: The request was successful.
- **201 Created**: The request was successful and a new resource was created.
- **400 Bad Request**: The request was invalid or cannot be processed.
- **401 Unauthorized**: Authentication is required and has failed or has not been provided.
- **404 Not Found**: The requested resource could not be found.
- **500 Internal Server Error**: An error occurred on the server.

## Endpoints

| URL            | Méthode | Description                                          | Params (path/query) | Body (JSON)                                                                                                       | Status        | Exemple de réponse                                                                                                                                                                                     |
| -------------- | ------- | ---------------------------------------------------- | ------------------- | ----------------------------------------------------------------------------------------------------------------- | ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| /auth/register | POST    | Inscription d'un nouvel utilisateur                  | -                   | `{ "email": "string", "name": "string", "password": "string" }`                                                   | 200, 400      | `{ "token": "jwt" }`                                                                                                                                                                                   |
| /auth/login    | POST    | Authentification d'un utilisateur                    | -                   | `{ "email": "string", "password": "string" }`                                                                     | 200, 401      | `{ "token": "jwt" }`                                                                                                                                                                                   |
| /auth/me       | GET     | Récupérer les informations de l'utilisateur connecté | -                   | -                                                                                                                 | 200, 401      | `{ "id": number, "name": "string", "email": "string", "created_at": "date", "updated_at": "date" }`                                                                                                    |
| /rentals       | GET     | Récupérer toutes les locations                       | -                   | -                                                                                                                 | 200, 401      | `{ "rentals": [ { "id": number, "name": "string", "surface": number, "price": number, "picture": "url", "description": "string", "owner_id": number, "created_at": "date", "updated_at": "date" } ] }` |
| /rentals/{id}  | GET     | Récupérer une location spécifique                    | id                  | -                                                                                                                 | 200, 401      | `{ "id": number, "name": "string", "surface": number, "price": number, "picture": "url", "description": "string", "owner_id": number, "created_at": "date", "updated_at": "date" }`                    |
| /rentals       | POST    | Créer une nouvelle location                          | -                   | `{ "name": "string", "surface": number, "price": number, "picture": "file", "description": "string" }` (FormData) | 200, 400      | `{ "message": "Rental created !" }`                                                                                                                                                                    |
| /rentals/{id}  | PUT     | Mettre à jour une location                           | id                  | `{ "name": "string", "surface": number, "price": number, "picture": "file", "description": "string" }` (FormData) | 200, 401      | `{ "message": "Rental updated !" }`                                                                                                                                                                    |
| /user/{id}     | GET     | Récupérer un utilisateur par son ID                  | id                  | -                                                                                                                 | 200, 401      | `{ "id": number, "name": "string", "email": "string", "created_at": "date", "updated_at": "date" }`                                                                                                    |
| /messages      | POST    | Envoyer un message                                   | -                   | `{ "message": "string", "user_id": number, "rental_id": number }`                                                 | 200, 400, 401 | `{ "message": "Message sent with success" }`                                                                                                                                                           |

## Entity Models

### 1. User

Représente un utilisateur de la plateforme.

**Champs**

- `id: number`
- `name: string`
- `email: string`
- `created_at: date`
- `updated_at: date`

**Endpoints associés**

- `GET /auth/me`
- `GET /user/{id}`

---

### 2. Rental

Représente une location (bien immobilier) publiée.

**Champs**

- `id: number`
- `name: string`
- `surface: number`
- `price: number`
- `picture: string (URL/image path)`
- `description: string`
- `owner_id: number` (référence vers `User.id`)
- `created_at: date`
- `updated_at: date`

**Endpoints associés**

- `GET /rentals`
- `GET /rentals/{id}`
- `POST /rentals`
- `PUT /rentals/{id}`

---

### 3. Message

Représente un message envoyé par un utilisateur à propos d'une location.

**Champs (payload de création)**

- `message: string`
- `user_id: number` (référence vers `User.id`)
- `rental_id: number` (référence vers `Rental.id`)

**Endpoints associés**

- `POST /messages`

---

### 4. AuthToken

Représente le jeton d'authentification renvoyé après connexion/inscription.

**Champs**

- `token: string`

**Endpoints associés**

- `POST /auth/register`
- `POST /auth/login`

---

### Relations métiers

- Un `User` peut posséder plusieurs `Rental` (`Rental.owner_id`).
- Un `User` peut envoyer plusieurs `Message`.
- Un `Rental` peut recevoir plusieurs `Message`.
