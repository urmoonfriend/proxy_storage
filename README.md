# Proxy Service

This service acts as a proxy between users and the storage service. Users interact with this service via RESTful API calls, and the service communicates with the storage backend using SOAP. The service can interact with the storage service either via RabbitMQ (asynchronously) or directly via SOAP.

## Table of Contents

- [Features](#features)
- [API Endpoints](#api-endpoints)
    - [Get a Student by Record Book Number](#get-a-student-by-record-book-number)
    - [Get All Students](#get-all-students)
- [Setup](#setup)
- [Running the Service](#running-the-service)
- [Running with Docker Compose](#running-with-docker-compose)
- [Dependencies](#dependencies)

## Features

- Provides a RESTful API to interact with the storage service.
- Retrieves individual student data by record book number.
- Retrieves a list of all students.
- Returns consistent response formats with HTTP status codes.
- Logs events to the console for monitoring.

## API Endpoints

### Get a Student by Record Book Number

**Endpoint:** `GET /proxy/students/{method}/{recordBookNumber}`

**Method:** Parameter in URL, which could be `soap` or `async`

**Description:** Retrieve details of a student by their record book number.

**Request Example:**

```bash
curl --location 'localhost:8087/proxy/students/soap/12412'
```

### Response Examples

**Success:**
```curl
{
    "success": true,
    "message": {
        "id": 2,
        "firstName": "Sara",
        "lastName": "John",
        "fatherName": "John",
        "file": "[encoded_file]",
        "fileName": "3_2.jpg",
        "contentType": "image/jpeg",
        "age": 19,
        "recordBookNumber": "12411",
        "createdAt": "2024-08-03T17:52:22.568544",
        "updatedAt": "2024-08-03T17:52:22.568572"
    }
}
```

**Not Found:**
```curl
{
    "success": false,
    "error": {
        "name": "BAD_REQUEST",
        "code": 400,
        "description": "Student with recordBookNumber: 12412 not found"
    }
}
```

**Server Error:**
```curl:
{
    "success": false,
    "error": {
        "name": "INTERNAL_SERVER_ERROR",
        "code": 500,
        "description": "Ошибка сервера"
    }
}
```

### Get All Students

**Endpoint:** `GET /proxy/students/{method}`

**Method:** Parameter in URL, which could be `soap` or `async`

**Description:** Retrieve details of a student by their record book number.

**Request Example:**

```bash
curl --location 'localhost:8087/proxy/students/soap'
```

### Response Examples

**Success:**
```curl
{
    "success": true,
    "message": [
        {
            "id": 1,
            "firstName": "Clare",
            "lastName": "John",
            "fatherName": "John",
            "file": "[encoded_file]",
            "fileName": "3.jpg",
            "contentType": "image/jpeg",
            "age": 19,
            "recordBookNumber": "12410",
            "createdAt": "2024-08-03T16:05:06.312368",
            "updatedAt": "2024-08-03T16:05:06.312472"
        },
        {
            "id": 2,
            "firstName": "Sara",
            "lastName": "John",
            "fatherName": "John",
            "file": "[encoded_file]",
            "fileName": "3_2.jpg",
            "contentType": "image/jpeg",
            "age": 19,
            "recordBookNumber": "12411",
            "createdAt": "2024-08-03T17:52:22.568544",
            "updatedAt": "2024-08-03T17:52:22.568572"
        }
    ]
}
```

**Server Error:**
```curl
{
    "success": false,
    "error": {
        "name": "INTERNAL_SERVER_ERROR",
        "code": 500,
        "description": "Ошибка сервера"
    }
}
```

## Setup

1) #### Ensure Java and Maven are installed.
2) #### Clone the repository. 
3) #### Navigate to the project directory. 
4) #### Run mvn clean install to build the project. 
5) #### Configure the storage service endpoint in the application.properties file.

## Running the Service
- #### Start the service with mvn spring-boot:run.
- #### The service will be accessible at http://localhost:8087/proxy.

## Running with Docker Compose
You can run the service using Docker Compose. The service will be built and run inside a Docker container.

### Prerequisites
- #### Docker and Docker Compose should be installed on your system.

### Steps

- #### Build the Docker image:
```bash
docker-compose build
```

- #### Start the service:
```bash
docker-compose up
```

#### The service will be accessible at http://localhost:8087/proxy. To stop the service, use:

- #### To stop service, use
```bash
docker-compose down
```

## Dependencies
- ### Spring Boot
- ### Spring Web Services
- ### Lombok
- ### ModelMapper
- ### Jakarta XML Bind (JAXB)
- ### Postgresql
- ### MinIO