# Documentación de API REST

## Listar Usuarios

Obtiene el listado de los usuarios.

### Request

**Method** : `GET`

**Path** : `/users`

**Authentication** : SI

**Headers** : `Authorization: Bearer 123456789`

### Success Response

**Code** : `200 OK`

**Content** : Listado de usuarios

```json
[
  {
    "firstName": "Jon",
    "lastName": "Snow",
    "email": "jon.snow@winterfell.com",
    "phone": "+54 11 4444-5555",
    "entity": "Hospital Alemán",
    "job": "Enfermero",
    "place": "Ciudad Autónoma de Buenos Aires"
  }, {
    "firstName": "Daenerys",
    "lastName": "Targaryen",
    "email": "mother.of.dragons@westeros.com",
    "phone": "+54 11 1111-3333",
    "entity": "Hospital Argerich",
    "job": "Cirujana",
    "place": "Buenos Aires"
  },
]
```

### Error Response

**Code** : `401 Unauthorized`

**Content** : Errores.

```json
{
    "errors": ["Invalid Token"]
}
```

## Registrar Usuario

Permite agregar una nueva cuenta de usuario.

**Method** : `POST`

**Path** : `/users`

**Auth required** : NO

**Body required** : json

```json
{
  "firstName": "Jon",
  "lastName": "Snow",
  "email": "jon.snow@winterfell.com",
  "phone": "+54 11 4444-5555",
  "entity": "Hospital Alemán",
  "job": "Enfermero",
  "place": "CABA",
  "pass": "1234"
}
```

### Success Response

**Code** : `201 Created`

**Content** : Información de creación

```json
{
  "created": true
}
```

### Error Response

**Code** : `400 Bad Request`

**Content** : Si hay datos erróneos o email ya registrado.

```json
{
    "created": false,
    "errors": [
        "E-Mail address already exists",
        "Job is required"
    ]
}
```

## Login de Usuario

La autenticación se realizará mediante mail y contraseña y el manejo de sessión
mediante un token otorgado al momento de autenticarse.

**Method** : `POST`

**Path** : `/login`

**Auth required** : NO

**Body required** : json

```json
{
  "email": "jon.snow@winterfell.com",
  "pass": "1234"
}
```

### Success Response

**Code** : `200 OK`

**Content** : Información de creación

```json
{
  "token": "123456789"
}
```

### Error Response

**Code** : `400 Bad Request`

**Content** : Si hay datos erróneos.

```json
{
  "token": false,
  "errors": [
    "Invalid email or password"
  ]
}
```

**Request:**

```json
GET /auth-place

```

**Response:**

`200 OK` (O lo que retorne el endpoint particular)
