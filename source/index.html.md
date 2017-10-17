---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - javascript

toc_footers:
  - <a href='#'>&copy; Andela, developed by Chukwuemeka Lionel Okoro</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>


search: true
---



# Introduction

PostIt API consists of several API endpoints that allows registered users to create accounts, create groups, add registered users to groups and post messages to groups which they belong. It also ensures that only group members can perform certain operations.

## Development

This application was developed using [NodeJs](https://nodejs.org), with [Express](http://expressjs.com) used for routing and [PostgreSQL](https://www.postgresql.org/) database with [Sequelize](http://docs.sequelizejs.com/) used as the ORM.

## Installation

Start up your terminal (or Command Prompt on Windows OS).
Ensure that you have node and postgresql installed on your PC.
Clone the repository by entering the command git clone https://github.com/3m3kalionel/PostIt/master in the terminal.
Navigate to the project root folder using cd PostIt on your terminal (or command prompt).
After cloning, install the application's dependencies with the command npm install.
Create a .env file in your root directory as described in .env.sample file.
Variable such as DATABASE_URL (which must be a postgresql URL) is defined in the .env file and it is essential to create this file before running the application.

## API Summary

### Users

Endpoint | Functionality
--------- | -----------
POST /api/v1/user/signup | Registers a user for the application
POST /api/v1/user/signin | Signs a user into the application
POST /api/v1/user/google_auth | Signs a user into the application
POST /api/v1/user/verify | Verifies a registered user that has forgotten his password
GET /api/v1/user/reset/:token | Allows a registered user to reset his password
GET /api/v1/users | Allows a logged in user to search for a user
GET /api/v1/users/search | Allows a logged in user to search all users in the application


### Groups

Endpoint | Functionality
--------- | -----------
POST /api/v1/group | Allows a logged in user create a new group
POST /api/v1/group/:groupid/user | Allows a group member add other users to a group
POST /api/v1/group/:groupid/message | Allows a logged in user post a message
GET /api/v1/group/:groupid/messages | Allows a logged in user list group messages
GET /api/v1/group/:groupid/users | Allows a logged in user list all users in a group
GET /api/v1/groups | Allows a logged in user to list groups he/she belongs to
GET /api/v1/groups/search | Allows a logged in user to search all groups in the application

# Users

## Sign Up User
This endpoint handles signing up(registering) a new user for the application. Required details are username, email, password and phone number.

```javascript
Request:

{
  "username": "user1"
  "password": "12345678"
  "email": "user1@gmail.com"
  "phone": "00000000001"
}

Response: 

{
    "user": {
        "id": 11,
        "username": "user1",
        "email": "user1@gmail.com",
        "phone": "00000000001"
    },
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6InVzZXIxIiwiZW1haWwiOiJ1c2VyMUBnbWFpbC5jb20iLCJpYXQiOjE1MDQyMzA4MjMsImV4cCI6MTUwNDQwMzYyM30.1KX3u5ylU6XEFeFO2DlujgVVHF7ppGb69ysiBnq6zWI"
}

```


### Request

* Endpoint: POST: `/api/v1/user/signup`

### Response
* Status: `201: Created`


## Sign In User
This endpoint verifies the login details and signs in a user if the details match. Required details are username and password.

```javascript
Request:

{
  "username": "user1"
  "password": "12345678"
}

Response:

{
    "message": "Login successful",
    "user": {
        "id": 11,
        "username": "user1",
        "email": "user1@gmail.com",
        "phone": "00000000001"
    },
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6InVzZXIxIiwiZW1haWwiOiJ1c2VyMUBnbWFpbC5jb20iLCJpZCI6MTEsImlhdCI6MTUwNDIzMjEyMiwiZXhwIjoxNTA0NDA0OTIyfQ.jtgiwpADFsVpfdt1h0NC_ss6frdHq_92LbbsK2IB97A"
}

```
### Request

* Endpoint: POST: `/api/v1/user/signin`

### Response
* Status: `200: OK`


## Google Authentication
This endpoint handles authentcating a user using his google email address, signing him in if the email already exists and signing him up(creating a new user) if the email isn't found. Required details are google email address and username.

```javascript
Request:

{
  "username": "user"
  "email": "user1@gmail.com"
}

Response:

{
    "message": "Login successful",
    "user": {
        "id": 1,
        "username": "user1",
        "email": "user1@gmail.com"
    },
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6ImVtZWthIiwiZW1haWwiOiJlbWVrYS5sLm9rb3JvQGdtYWlsLmNvbSIsImlkIjoxLCJpYXQiOjE1MDYyNTk2NTcsImV4cCI6MTUwNjQzMjQ1N30.k1OvP6QgIth7ow2ZPdIYGn8FhxgpOsKp6gzj2gWRvG0"
}

```
### Request

* Endpoint: POST: `/api/v1/user/google_auth`

### Response
* Status: `200: OK`

## Verify User
This endpoint handles the verification of the email provided when the user clicks the forgot password button and sends the user an email containing a link to the password reset form. Required details are the user's email address.

```javascript
Request:

{
  "email": "user1@gmail.com"
}

Response:

{
  "message": "verification email sent"
}

```
### Request

* Endpoint: POST: `/api/v1/user/verify`

### Response
* Status: `200: OK`



-------------------
## Reset Password

```javascript
Request:

{
  "email": "user1@gmail.com"
}

Response:

{
  "user": "user1",
  "success": true,
  "message": "verification email sent"
}

```
### Request
This endpoint handles the verification of the token obtained from the url when the user clicks the link in the reset password email and resets the password.
* Endpoint: POST: `/api/v1/user/verify`
* Body: `(application/json)`

### Response
* Status: `200: OK`

## Search for Users

This endpoint handles the search for registered users based on the search term(username), the number of results per page(limit) and the first(n) number of names to skip(offset). Required details are the username, limit and the offset.

> Sample JSON structured response:

```javascript


{
    "count": 18,
    "rows": [
        {
            "id": 3,
            "username": "femi"
        },
        {
            "id": 4,
            "username": "terungwa"
        },
        {
            "id": 5,
            "username": "zuckerberg"
        }
    ]
}

```
### Request

* Endpoint: GET: `/api/v1/users`
* Requires: Authentication

### Response
* Status: `200: OK`

<aside class="success">
Has default values of limit and offset set to 3 and 0 respectively
</aside>

<aside class="warning">
Requires a username query to be specified
</aside>

## Search all Users

This endpoint handles the request for all registered users in the application. Required details are the username. Returns Pagination metadata.

```javascript
Response:

{
    "totalPageCount": 6,
    "currentPage": 1,
    "nextPage": 2,
    "rowCount": 16,
    "users": [
        {
            "id": 3,
            "username": "femi",
            "email": "femi@andela.com",
            "phone": "395853"
        },
        {
            "id": 4,
            "username": "terungwa",
            "email": "terungwa@andela.com",
            "phone": "15"
        },
        {
            "id": 5,
            "username": "zuckerberg",
            "email": "zuckerberg@andela.com",
            "phone": "4455"
        }
    ]
}

```

### Request:
* Endpoint: GET: `localhost:8080/api/v1/users/search?username=e`
* Requires: Authentication

### Response
* Status: `200: OK`

<aside class="success">
Has default values of limit and offset set to 3 and 0 respectively
</aside>

<aside class="warning">
Requires a username query to be specified
</aside>


# Groups

## Create Group
This endpoint handles group creation and adding the creator to it. Required details are the name of the group and the description.

```javascript
Request: 

{
  "name": "API documentation test group"
  "description": "midnight documentation test group"
}

Response:

{
    "message": "Group created",
    "group": {
        "id": 3,
        "name": "API documentation test group",
        "description": "midnight documentation test group",
    }
}

```

### Request

* Endpoint: POST: `/api/v1/group`
* Requires: Authentication

### Response
* Status: `201: Created`

## Add user to group

This endpoint handles adding a registered user to a group. Required details are the groupId and the userId.

```javascript
Request:
{
  "userId": 2
}

Response:

{
    "message": "jonjones added to group",
    "user": {
        "id": 59,
        "username": "jonjones",
        "email": "jonjones@andela.com"
    }
}
```

### Request

* Endpoint: POST: `/api/v1/group/:groupid/user`
* Requires: Authentication

### Response
* Status: `200: OK`


## Post message

This endpoint handles creating and posting a message in a group. Required details are the groupId, content and the priority(normal, critical or urgent) of the message.

```javascript
Request:

{
  "content": "hey everyone. how's it going"
}

Response:

{
    "message": {
        "id": 14,
        "content": "hey everyone. how's it going",
        "userId": 11,
        "groupId": 3
    }
}
```

### Request

* Endpoint: POST: `/api/v1/group/14/message`
* Requires: Authentication

### Response
* Status: `201: Created`

## Retrieve group messages

This endpoint handles the request for all the messages belonging to a group. Required details are the groupId.

``` javascript
Response:

[
   {
        "id": 60,
        "content": "see world",
        "userId": 1,
        "groupId": 14,
        "priority": "normal"
    },
    {
        "id": 61,
        "content": "see no evil",
        "userId": 1,
        "groupId": 14,
        "priority": "normal"
    }
]


```

### Request:
* Endpoint: GET: `/api/v1/group/3/messages`
* Requires: Authentication

### Response
* Status: `200: OK`

## List group members

This endpoint handles the request for a list of all the users belonging to a group. Required details are the groupId.

``` javascript

Response:

[
    {
        "id": 1,
        "username": "emeka",
        "email": "emeka@gmail.com",
        "phone": "080377377383"
    },
    {
        "id": 2,
        "username": "okoro",
        "email": "emeka.l.okoro@gmail.com",
        "phone": "0804455325"
    },
    {
        "id": 3,
        "username": "user1",
        "email": "user1@gmail.com",
        "phone": "2223455678"
    },
]

```

### Request:

* Endpoint: GET: `/api/v1/group/:groupid/users`
* Requires: Authentication

### Response
* Status: `200: OK`


## List User's groups

This endpoint handles the request for a list of all the groups a user belongs to.

```javascript
Response:

[
    {
        "name": "class 28",
        "id": 17
    },
    {
        "name": "white belt devs",
        "id": 27
    },
    {
        "name": "amity_devs",
        "id": 35
    },
    {
        "name": "retards",
        "id": 46
    },
    {
        "name": "cohort 28",
        "id": 47
    }
]

```

### Request:
* Endpoint: GET: `/api/v1/groups`
* Requires: Authentication

### Response
* Status: `200: OK`

## Search all Groups

This endpoint handles the request for all registered users in the application. Required details are the username. Returns Pagination metadata.

```javascript
Response:

{
    "totalPageCount": 4,
    "currentPage": 1,
    "nextPage": 2,
    "rowCount": 12,
    "groups": [
        {
            "id": 64,
            "name": "class 30",
            "description": "next level"
        },
        {
            "id": 65,
            "name": "react developers",
            "description": "it is what it is"
        },
        {
            "id": 66,
            "name": "Doggfathers",
            "description": "the original doggfathers"
        }
    ]
}

```

### Request:
* Endpoint: GET: `localhost:8080/api/v1/groups/search?name=e`
* Requires: Authentication

### Response
* Status: `200: OK`

<aside class="success">
Has default values of limit and offset set to 3 and 0 respectively
</aside>

<aside class="warning">
Requires a name query to be specified for the group.
</aside>


# Errors

The following error codes were used in PostIt

Endpoint | Description/Implementation
--------- | -----------
400 | Bad Request -- The server cannot or will not process the request due to something that is perceived to be a client error. Use cases are: requests with empty input fields or input fields consisting of space(s) only.
401 | Unauthorized -- Used for authentication(token) issues - when the user did not provide a valid authentication or any authentication at all.
403 | Forbidden/Not permitted -- Here authentication is provided, but the authenticated user is not permitted to perform the requested operation. The user might not have the necessary permissions for a resource. Use cases here - Users trying to list messages in a group or add another user to a group he does not belong to, trying to list out users in a group he does not belong to etc.
422 | Unprocessable entity -- The request was well-formed but was unable to be followed due to semantic errors. Use cases - Where wrong data types were used. Example: inputing a string for limits or offset values used in pagination, userId, groupId etx.
