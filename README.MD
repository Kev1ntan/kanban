# KANBAN
​
​
​
## Table of Content
​
- **Kanban-API**
  - [Base URL](#base-url)
  - [Installation](#installation)
  - [Usage](#usage)
  - [List of Routes User](#list-of-routes-user)
  - [List of Errors](#list-of-errors)
  - **User Endpoints**
    - [Login](#post-login)
    - [Register](#post-register)
  - **Todo Endpoint**
    - [Show All](#get-tasks)
    - [Create Task](#post-tasks)
    - [Show Task by Id](#get-tasks-by-id)
    - [Change Status Todo Task](#patch-tasks)
    - [Delete Todo Task](#delete-tasks)
​
------
​
#### **Base URL :** `https://k-kanban.firebaseapp.com`
​
#### **Installation:**
​
Clone this API from repository and install npm, then on `server` directory install the neccessary npm package
​
```
$ git clone https://github.com/Kev1ntan/kanban.git
$ cd ../server
$ npm install
```
​
Do the same on `client` directory
​
#### **Usage:**
​
Run script from packages by using the command below on `server` directory
​
```
$ npm start
```
​
Run script from packages by using the command below on `client` directory
​
```
$ npm run dev
```
​
#### **User Routes:**
| **Route**           | **HTTP** | **Description**                                     |
| ------------------- | -------- | --------------------------------------------------- |
| /user/login         | POST     | Log in and get an access token based on credentials |
| /user/register      | POST     | Sign up new user                                    |
| /user/googlelogin   | POST     | Sign in OAuth Google Login                          |
| /user/facebooklogin | POST     | Sign in OAuth facebook Login                        |
​
#### **Task Routes:**
**login required**
| **Route**  | **HTTP** | **Description**                                              |
| ---------- | -------- | ------------------------------------------------------------ |
| /tasks     | GET      | Show all task of every user                                  |
| /tasks     | POST     | Create new task                                              |
| /task/:id  | PUT      | Change category of specified task *(authorization required)* |
| /task/:id  | PATCH    | Change category of specified task *(authorization required)* |
| /task/:id  | DELETE   | Delete task by id *(authorization required)*                 |
​
#### **List of Errors:**
| **Code** | **Name**              | **Description**                                |
| -------- | --------------------- | ---------------------------------------------- |
| 400      | Bad Request           | Incorrect user access or form validation       |
| 403      | Forbidden             | Unauthorized access                            |
| 404      | Not Found             | Requested resource not found                   |
| 500      | Internal Server Error | Server currently unable to handle this request |
​
​
​
## **POST Login**
​
- **URL:** `/login`
​
- **Method:** `POST`
​
- **Description:** `Log in and get an access token based on credentials`
​
- **Request body:**
​
  ```javascript
  {
      "email": "email@domain.com", //required
      "password": "password" //required
  }
  ```
​
- **Success Response:**
​
  - **Status:** `200`
​
    ```javascript
    {
        "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6MiwiZW1haWwiOiJoYWZpenVscmlma2loYXdhcmlAZ21haWwuY29tIiwiaWF0IjoxNTgxNjQ0ODM4fQ.cUwd7Tv2KeusXaa58oEeHsF7L9SzK0HxPLWkb8Vfa88",
        "name": "John Doe"
    }
    ```
​
- **Error Response:**
​
  - **Status:** `400`
​
    ```javascript
    {
        "errors": [
            "Incorrect Username or Password"
        ]
    }
    ```
​
  - **Status:** `500`
​
    ```javascript
    {
        "errors": "Server currently unable to handle this request"
    }
    ```
​
## **POST Register**
​
- **URL:** `/register`
​
- **Method:** `POST`
​
- **Description:** `Sign up new user`
​
- **Request body:**
​
  ```javascript
  {
      "name": "user", //required
      "email": "email@domain.com", //required
      "password": "password" //required
  }
  ```
​
- **Success Response:**
​
  - **Status:** `201`
​
    ```javascript
    {
        "name": "user",
        "email": "email@domain.com"
    }
    ```
​
- **Error Response:**
​
  - **Status:**`400`
​
    ```javascript
    {
        "errors": [
            "Validation error: Please input name,",
            "Validation error: Please input password"
        ]
    }
    ```
​
  - **Status:** `500`
​
    ```javascript
    {
        "errors": "Server currently unable to handle this request"
    }
    ```
​
## GET Tasks
​
- **URL:** `/tasks`
​
- **Method:** `GET`
​
- **Description:** `Show all tasks on kanban`
​
- **Request Header:**
​
  ```javascript
  {
      "token": eyJhbGciOiJIUzI1NiIsInR, //required
  }
  ```
​
- **Success Response:**
​
  - **Status:** `200`
​
    ```javascript
    [
        {
            "id": 1,
            "title": "Creating Kanban",
            "category": "Development",
            "UserId": 1,
            "createdAt": "2020-02-12T09:42:38.329Z",
            "updatedAt": "2020-02-13T12:14:22.000Z",
            "User": {
                "id": 1,
                "name": "John Doe",
                "email": "email@domain.com",
                "createdAt": "2020-02-11T09:28:54.666Z",
                "updatedAt": "2020-02-11T09:28:54.666Z"
            }
        },
        {
            "id": 2,
            "title": "Installing NPM",
            "category": "Done",
            "UserId": 1,
            "createdAt": "2020-02-12T09:42:20.676Z",
            "updatedAt": "2020-02-13T13:26:35.901Z",
            "User": {
                "id": 1,
                "name": "John Doe",
                "email": "email@domain.com",
                "createdAt": "2020-02-11T09:28:54.666Z",
                "updatedAt": "2020-02-11T09:28:54.666Z"
            }
        },
    ]
    ```
​
- **Error Response:**
​
  - **Status:** `500`
​
    ```javascript
    {
        "msg": "Server currently unable to handle this request"
    }
    ```
​
​
​
## POST Task​

- **URL:** `/task`
​
- **Method:** `POST`
​
- **Description:** `Create new task on user to-do list`
​
- **Request Header:**
​
  ```javascript
  {
      "token": eyJhbGciOiJIUzI1NiIsInR, //required
  }
  ```
​
- **Request body:** Acceptable value of `category` in tasks are `['Backlog', 'Development', 'Production', 'Done']`
​
  ```javascript
  {
      "title": "Redesign template login kanban",
      "category": "Backlog",
  }
  ```
​
- **Success Response:**
​
  - **Status:** `201`
​
    ```javascript
    {
        "id": 3,
        "title": "Redesign template login kanban",
        "category": "Backlog",
        "UserId": 1,
        "updatedAt": "2020-02-14T01:57:21.368Z",
        "createdAt": "2020-02-14T01:57:21.368Z"
    }
    ```
  
- **Error Response:**
​
  - **Status:** `400`
​
    ```javascript
    {
        "errors": [
            "Validation error: Please input todo title,",
            "Validation error: Please input todo category"
        ]
    }
    ```
  
  - **Status:** `500`
  
    ```javascript
    {
        "errors": "Server currently unable to handle this request"
    }
    ```
  
    
​
## GET Task
​
- **URL:** `/task/:id`
​
- **Method:** `GET`
​
- **Description:** `Show user task from kanban`
​
- **URL Params:** 
​
  ```javascript
  "id": 1, //required
  ```
​
- **Request Header:**
​
  ```javascript
  {
      "token": eyJhbGciOiJIUzI1NiIsInR, //required
  }
  ```
​
- **Success Response:**
​
  - **Status:** `201`
  
    ```javascript
    {
        "id": 4,
        "title": "Restructuring Kanban using vue component",
        "category": "Development",
        "UserId": 2,
        "createdAt": "2020-02-12T16:13:00.603Z",
        "updatedAt": "2020-02-13T01:48:07.850Z"
    }
    ```
    
  
- **Error Response:**
​
  - **Status:** `404`
​
    ```javascript
    {
        "errors": [
            "Data not found!"
        ]
    }
    ```
​
  - **Status:** `500`
  
    ```javascript
    {
        "errors": "Server currently unable to handle this request"
    }
    ```
  
  
​
## PATCH Task
​
- **URL:** `/task/:id`
​
- **Method:** `PATCH`
​
- **Description:** `Change category of specified task` 
​
- **URL Params:** 
​
  ```javascript
  "id": 1, //required
  ```
​
- **Request Header:**
​
  ```javascript
  {
      "token": eyJhbGciOiJIUzI1NiIsInR, //required
  }
  ```
​
- **Request Body:**
​
  ```javascript
  {
      "category": "Production"
  }
  ```
​
- **Success Response:**
​
  - **Status:** `200`
​
    ```javascript
    {
        "id": 4,
        "title": "Restructuring Kanban using vue component",
        "category": "Production",
        "UserId": 2,
        "createdAt": "2020-02-14T01:57:21.368Z",
        "updatedAt": "2020-02-14T02:34:24.514Z"
    }
    ```
  
- **Error Response:**
​
  - **Status:** `404`
​
    ```javascript
    {
        "errors": [
            "Data not found!"
        ]
    }
    ```
    
  - **Status:** `500`
    
    ```javascript
      {
          "errors": "Server currently unable to handle this request"
      }
    ```
    
    
## DELETE Task
​
- **URL:** `/task/:id`
​
- **Method:** `DELETE`
​
- **Description:** `Delete specified task from list of Kanban tasks`
​
- **URL Params:** 
​
  ```javascript
  "id": 1 //required
  ```
  
- **Request Header:**
​
  ```javascript
  {
      "token": eyJhbGciOiJIUzI1NiIsInR, //required
  }
  ```
​
​
- **Success Response:**
​
  - **Status:** `200`
​
    ```javascript
    {
        "id": 6,
        "title": "Adding feature socket on app",
        "category": "Done",
        "UserId": 2,
        "createdAt": "2020-02-13T09:50:51.289Z",
        "updatedAt": "2020-02-13T09:50:51.289Z"
    }
    ```
  
- **Error Response:**
​
  - **Status:** `404`
​
    ```javascript
    {
        "errors": [
            "Data not found!"
        ]
    }
    ```
    
  - **Status:** `500`
    
    ```javascript
      {
          "errors": "Server currently unable to handle this request"
      }
    ```