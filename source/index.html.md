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

return to document this

## API Summary

### Users

Endpoint | Functionality
--------- | -----------
POST /api/user/signup | Registers a user for the application
POST /api/user/signin | Signs a user into the application
POST /api/user/verify | Verifies a registered user that has forgotten his password
GET /api/users | Allows a logged in user to search for a user

### Groups

Endpoint | Functionality
--------- | -----------
POST /api/group | Allows a logged in user create a new group
POST /api/group/:groupid/user | Allows a group member add other users to a group
POST /api/group/:groupid/message | Allows a logged in user post a message
GET /api/group/:groupid/messages | Allows a logged in user list group messages
GET /api/group/:groupid/users | Allows a logged in user list all users in a group
GET /api/groups | Allows a logged in user to list groups he/she belongs to

# Users

## Signup User

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
        "phone": "00000000001",
        "updatedAt": "2017-09-01T01:53:42.998Z",
        "createdAt": "2017-09-01T01:53:42.998Z"
    },
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6InVzZXIxIiwiZW1haWwiOiJ1c2VyMUBnbWFpbC5jb20iLCJpYXQiOjE1MDQyMzA4MjMsImV4cCI6MTUwNDQwMzYyM30.1KX3u5ylU6XEFeFO2DlujgVVHF7ppGb69ysiBnq6zWI"
}

```


### Request
* Endpoint: POST: `/api/user/signup`
* Body: `(application/json)`

### Response
* Status: `201: Created`
* Body: `(application/json)`


## Signin User

```javascript
Request:

{
  "username": "user1"
  "password": "12345678"
}

Response:

{
    "status": "user1 successfully logged in",
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
* Endpoint: POST: `/api/user/signin`
* Body: `(application/json)`

### Response
* Status: `200: OK`
* Body: `(application/json)`


## Verify User

```javascript
Request:

{
  "email": "user1@gmail.com"
}

Response:

{
  "success": true,
  "message": "verification mail sent"
}

```
### Request
* Endpoint: POST: `/api/user/verify`
* Body: `(application/json)`

### Response
* Status: `200: OK`
* Body: `(application/json)`

## Search for Users

> Sample JSON structured response:

```javascript


[
    {
        "id": 2,
        "username": "emeka",
        "email": "emeka.l.okoro@gmail.com",
        "phone": "0803*****6"
    },
    {
        "id": 11,
        "username": "user1",
        "email": "user1@gmail.com",
        "phone": "00000000001"
    }
]

```
### Request
* Endpoint: GET: `/users?q=e`
* Requires: Authentication

### Response
* Status: `200: OK`
* Body: `(application/json)`

# Groups

## Create Group

```javascript
Request: 

{
  "name": "API documentation test group"
  "description": "midnight documentation test group"
}

Response:

{
    "success": true,
    "message": "Group created",
    "group": {
        "id": 3,
        "name": "API documentation test group",
        "description": "midnight documentation test group",
        "updatedAt": "2017-09-01T03:10:55.342Z",
        "createdAt": "2017-09-01T03:10:55.342Z"
    }
}

```

### Request
* Endpoint: POST: `/group`
* Requires: Authentication
* Body: `(application/json)`

### Response
* Status: `201: Created`
* Body: `(application/json)`

## Add user to group

```javascript
Request:
{
  "userId": 2
}

Response:

{
    "success": true,
    "message": "emeka added to group",
    "user": {
        "id": 2,
        "username": "emeka",
        "email": "emeka.l.okoro@gmail.com"
    }
}
```

### Request
* Endpoint: POST: `/group/:groupid/user`
* Requires: Authentication
* Body: `(application/json)`

### Response
* Status: `201: Created`
* Body: `(application/json)`


## Post message

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
        "groupId": 3,
        "updatedAt": "2017-09-01T05:46:50.441Z",
        "createdAt": "2017-09-01T05:46:50.441Z"
    }
}
```

### Request
* Endpoint: POST: `/group/3/message`
* Requires: Authentication
* Body: `(application/json)`

### Response
* Status: `201: Created`
* Body: `(application/json)`

## Retrieve group messages

``` javascript
Response:

[
    {
        "id": 14,
        "content": "hey everyone. how's it going",
        "userId": 11,
        "groupId": 3,
        "createdAt": "2017-09-01T05:46:50.441Z",
        "updatedAt": "2017-09-01T05:46:50.441Z"
    }
]


```

### Request:
* Endpoint: GET: `/group/3/messages`
* Reauires: Authentication

### Response
* Status: `200: OK`
* Body `(application/json)`

## List group members

``` javascript

Response:

[
    {
        "id": 2,
        "username": "emeka",
        "email": "emeka.l.okoro@gmail.com",
        "phone": "0803******6",
        "UserGroups": {
            "createdAt": "2017-09-01T03:25:19.367Z",
            "updatedAt": "2017-09-01T03:25:19.367Z",
            "groupId": 3,
            "userId": 2
        }
    },
    {
        "id": 11,
        "username": "user1",
        "email": "user1@gmail.com",
        "phone": "00000000001",
        "UserGroups": {
            "createdAt": "2017-09-01T03:10:55.422Z",
            "updatedAt": "2017-09-01T03:10:55.422Z",
            "groupId": 3,
            "userId": 11
        }
    }
]

```

### Request:
* Endpoint: GET: `/group/:groupid/users`
* Requires: Authentication

### Response
* Status: `200: OK`
* Body `(application/json)`


## List user's groups

```javascript
Response:

[
    {
        "id": 3,
        "name": "API documentation test group",
        "description": "midnight documentation test group",
        "createdAt": "2017-09-01T03:10:55.342Z",
        "updatedAt": "2017-09-01T03:10:55.342Z",
        "UserGroups": {
            "createdAt": "2017-09-01T03:10:55.422Z",
            "updatedAt": "2017-09-01T03:10:55.422Z",
            "groupId": 3,
            "userId": 11
        }
    }
]

```

### Request:
* Endpoint: GET: `/groups`
* Requires: Authentication

### Response
* Status: `200: OK`
* Body `(application/json)`
