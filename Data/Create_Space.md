# Create Space

## Table of Contents

- [Create Space](#create-space)
  - [Table of Contents](#table-of-contents)
  - [Overview](#overview)
    - [Spatial Index Overview](#spatial-index-overview)
  - [Endpoints](#endpoints)
    - [Create Space](#create-space-1)
      - [POST /projects/{projectId}/spaces](#post-projectsprojectidspaces)
      - [Request](#request)
        - [POST /projects/a03bd562-7e9d-4751-8a56-f36e435dcac1/spaces](#post-projectsa03bd562-7e9d-4751-8a56-f36e435dcac1spaces)
        - [Path parameters](#path-parameters)
        - [Request Body](#request-body)
        - [Sample Request Body](#sample-request-body)
    - [Response](#response)
      - [Http Status code](#http-status-code)
      - [Success](#success)
      - [Error codes](#error-codes)
    - [Examples](#examples)
    - [Licensing](#licensing)
    - [Notes](#notes)

## Overview

### Spatial Index Overview

Use Project Center's Spatial Index activity center to create spaces that track and index rooms, areas, and spaces from Autodesk Revit models. You can analyze various aspects of a project using the spatial index, such as site, building, level, and room, for example. You can also track standard properties from Revit, including room name, number, type, occupancy, department; wall, floor, and ceiling finish, as well as area, volume and perimeter.

## Endpoints

### Create Space

- Epic - [NPC-17164 - Spatial Index API](https://newforma.atlassian.net/browse/NPC-17164)
- Task - [NPC-17157 - [API] Create Space](https://newforma.atlassian.net/browse/NPC-17157)

This endpoint used to add new space definitions for the project and returns the space id.

#### POST /projects/{projectId}/spaces

#### Request

##### POST /projects/a03bd562-7e9d-4751-8a56-f36e435dcac1/spaces

##### Path parameters

| Name      | Type         | Required? | Description              |
| --------- | ------------ | --------- | ------------------------ |
| projectId | string(guid) | Yes       | Specifies the project id |

##### Request Body

```
{
  "source": {
    "sourcePrimaryKey": "string",
    "sourceDescriptor": "string"
  },
  "name": "string",
  "number": "string",
  "description": "string",
  "keywords": [
    {
      "name": "string",
      "type": "generic"
    }
  ]
}
```

| Name        | Data Type       | Character Limit        | Required?   | Description                                                           |
| -- | --| -- | -- | -- |
| name        | string          | 255                    | Conditional | Specifies the name of the space, either name or number are required   |
| number      | string          | 128                    | Conditional | Specifies the number of the space, either name or number are required |
| description | string          | 1 million              | No          | Specifies the description about the space.                            |
| keywords    | keywordDto[]    | 255 (keywordname)      | No          | Specifies any keyword applied to the space.                           |
| source      | SourceInfoDto[] | 255 (sourcePrimaryKey),255 (sourceDescriptor)| No  | Specifies source primary key and source descriptor. |
                                                               

##### Sample Request Body

```
{
  "source": {
    "sourcePrimaryKey": "71234567890",
    "sourceDescriptor": "access from external application"
  },
  "name": "party space",
  "number": "party123",
  "description": "this space is for party area",
  "keywords": [
    {
      "name": "general",
      "type": "generic"
    }
  ]
}
```

### Response

#### Http Status code

#### Success

| HTTP Status | Status Message | Description                                  |
| ----------- | -------------- | -------------------------------------------- |
| 201         | Created        | Space created and returns the space id(Guid) |

#### Error codes

| Code | Status Message        | Description                                                                                |
| ---- | --------------------- | ------------------------------------------------------------------------------------------ |
| 400  | Bad Request           | The request is invalid (projectId is not valid GUID)                                       |
| 400  | Bad Request           | The request is invalid (keywords are not in array format)                                  |
| 400  | Bad Request           | The request is invalid (both name and number are empty)                                    |
| 400  | Bad Request           | The request is invalid (keywords must be generic)                                          |
| 400  | Bad Request           | The request is invalid (keyword name is empty)                                             |
| 400  | Bad Request           | The request is invalid (keyword type is empty)                                             |
| 400  | Bad Request           | The request is invalid (name supports upto 255 characters)                                 |
| 400  | Bad Request           | The request is invalid (number supports upto 128 characters)                               |
| 400  | Bad Request           | The request is invalid (source primaryKey supports upto 255 characters)                    |
| 400  | Bad Request           | The request is invalid (source descriptor supports upto 255 characters)                    |
| 400  | Bad Request           | The request is invalid (request payload is empty)                                          |
| 400  | Bad Request           | The request is invalid (keyword name supports upto 255 characters)                         |
| 400  | Bad Request           | `{PropertyName} property contains forbidden character backslash` <br /> <i>We don't allow create space with `\` character via a `name` and a `number` </i>|
| 401  | Unauthorized          | Required authentication information is either missing                                      |
| 403  | Forbidden             | Access is denied to the requested resource. The user might not have sufficient permissions |
| 404  | Not Found             | The requested resource does not exist (projectId doesnot exists)                           |
| 409  | Conflict              | Source primary key and source descriptor already exist for the project                     |
| 500  | Internal Server Error | There was an internal server error while processing the request.                           |

### Examples

<details>
  <summary><b>POST /projects/a03bd562-7e9d-4751-8a56-f36e435dcac1/spaces</b></summary>
  
   Request body
   ```json
  {
  "source": {
    "sourcePrimaryKey": "71234567890",
    "sourceDescriptor": "external descriptor"
  },
  "name": "space001",
  "number": "001",
  "description": "this space is for hall area",
  "keywords": [
    {
      "name": "general",
      "type": "generic"
    }
  ]
}
```

HTTP 201 Created

```json
{
  "id": "5d333498-71a6-4d36-b866-34dc1ff4c536"
}
```

</details>

<details>
  <summary><b>POST /projects/a03bd562-7e9d-4751/spaces</b></summary>

Http 400 BadRequest

```json
{
  "ErrorMessage": "The request is invalid.",
  "ModelState": {
    "projectId": ["projectId must be a valid Guid"]
  }
}
```

</details>

</details>

<details>
  <summary><b>POST /projects/a03bd562-7e9d-4751-8a56-f36e435dcac2/spaces</b></summary>

Http 404 NotFound

```json
{
  "ErrorMessage": "Project not found: a03bd562-7e9d-4751-8a56-f36e435dcac2"
}
```

</details>

<details>
  <summary><b>POST /projects/a03bd562-7e9d-4751-8a56-f36e435dcac1/spaces</b></summary>

Request body

```json
{
  "source": {
    "sourcePrimaryKey": "71234567890",
    "sourceDescriptor": "external descriptor"
  },
  "name": "",
  "number": "",
  "description": "this space is for hall area",
  "keywords": [
    {
      "name": "general",
      "type": "generic"
    }
  ]
}
```

Http 400 BadRequest

```json
{
  "ErrorMessage": "The request is invalid.",
  "ModelState": {
    "model": ["name or number are required"]
  }
}
```

</details>

<details>
  <summary><b>POST /projects/a03bd562-7e9d-4751-8a56-f36e435dcac1/spaces</b></summary>

Request body

```json
{
  "source": {
    "sourcePrimaryKey": "71234567890",
    "sourceDescriptor": "external descriptor"
  },
  "name": "space002",
  "number": "002",
  "description": "this space is for hall area",
  "keywords": [
    {
      "name": "general",
      "type": "generic"
    }
  ]
}
```

Http 409 Conflict

```json
{
  "data": {
    "id": "5d333498-71a6-4d36-b866-34dc1ff4c536",
    "source": {
      "sourcePrimaryKey": "71234567890",
      "sourceDescriptor": "external descriptor"
    }
  },
  "ErrorMessage": "Space already exists"
}
```

</details>

<details>
  <summary><b>POST /projects/a03bd562-7e9d-4751-8a56-f36e435dcac1/spaces</b></summary>

Request body

```json
{
  "source": {
    "sourcePrimaryKey": "71234567800",
    "sourceDescriptor": "test descriptor"
  },
  "name": "space002",
  "number": "002",
  "description": "this space is for hall area",
  "keywords": [
    {
      "name": "general",
      "type": "abcd"
    }
  ]
}
```

Http 400 BadRequest

```json
{
    "ErrorMessage": "The request is invalid.",
    "ModelState": {
        "model": [
            "Error occurred while processing property 'keywords'. Keyword is not of generic type"
        ]
    }
}
```

</details>

<details>
  <summary><b>POST /projects/a03bd562-7e9d-4751-8a56-f36e435dcac1/spaces</b></summary>

Request body

```json
{
  "source": {
    "sourcePrimaryKey": "71234567800",
    "sourceDescriptor": "test descriptor"
  },
  "name": "space002",
  "number": "002",
  "description": "this space is for hall area",
  "keywords": {
    "name": "general",
    "type": "abcd"
  }
}
```

Http 400 BadRequest

```json
{
    "ErrorMessage": "The request is invalid.",
    "ModelState": {
        "model": [
            "Error occurred while processing property 'keywords'. Field is not an array"
        ]
    }
}
```
</details>

<details>
  <summary><b>POST /projects/a03bd562-7e9d-4751-8a56-f36e435dcac1/spaces</b></summary>

Request body

```json
{
  "source": {
    "sourcePrimaryKey": "71234567800",
    "sourceDescriptor": "test descriptor"
  },
  "name":"mpe2aCuMM1S8gJ1lQOcT9aAUPxZVy2KUcKScQrVArTGFhcBoYGurelE24pFlTHxqDwJ9h700V7tUaAMcprFPhnqBGmjG3A8s9m5ZZmFq7D4tyYuytuQh2BVYbuenImc5MfVMXe3bob0jC5tchyLGwXf7runliUYIQd4FSLhyUXRX2qVOPcRSeZ5SvXAVzkKAhFpsgQnAs3GUqcqkLfClyT5IJtKVRF3VblEAX1q3IqR4uscT7eoSVf1ePHCDrMpi",
  "number": "123",
  "description": "this space is for hall area",
  "keywords": [
    {
      "name": "general",
      "type": "generic"
    }
  ]
}
```

HTTP 400 BadRequest   

``` json
{
    "ErrorMessage": "The request is invalid.",
    "ModelState": {
        "model.name": [
            "The length of 'Name' must be 255 characters or fewer. You entered 268 characters."
        ]
    }
}
```
</details>

HTTP 400 BadRequest   

``` json
{
    "ErrorMessage": "The request is invalid.",
    "ModelState": {
        "model.name": [
            "Name property contains forbidden character backslash."
        ]
    }
}
```
</details>

#### Licensing

- The user must have professional user role to use the endpoint.

### Notes

- All `Parameters` are case-sensitive except of `GUID`.
- The keyword "type" should be always a generic.
- In "source" object the combination of "sourceprimarykey" and "sourcedescriptor" must be unique.
- `\` - back slash is forbidder for `name` and `number` fields
