# Create punch list item

## Table of Contents
- [Create punch list item](#create-punch-list-item)
  - [Table of Contents](#table-of-contents)
  - [Related design reviews](#related-design-reviews)
  - [Endpoints](#endpoints)
    - [Create punch list item](#create-punch-list-item-1)
      - [POST /projects/{projectId}/punchlistitems](#post-projectsprojectidpunchlistitems)
      - [Request](#request)
        - [Additional info](#additional-info)
      - [Response](#response)
      - [Statuses codes](#statuses-codes)
        - [Success](#success)
        - [Errors](#errors)
      - [Licensing](#licensing)
      - [Examples](#examples)
      - [Notes](#notes)

## Related design reviews

| Date             | Description                                                                                                                              | Author       |
| -----------------|----------------------------------------------------------------------------------------------------------------------------------------- |--------------|
| March 07, 2022   | [Create punch list item](./DesignReviews/PunchListItems/create_punchlist_item.md)                                                        | Peaky Coders |
| April 26, 2022   | [Create punch list item missing ID field](./DesignReviews/PunchListItems/create_punchlist_item_missing_id.md)                            | Peaky Coders |

## Endpoints

### Create punch list item

- Epic - [NPC-16577 - Punch List API](https://newforma.atlassian.net/browse/NPC-16577)
- Task - [NPC-17165 - [API] Create Punch List item](https://newforma.atlassian.net/browse/NPC-17165)

The endpoint is used to add new punch list item for the project and return its id.

#### POST /projects/{projectId}/punchlistitems

#### Request

| Name        | Source     | Type          |  Character Limit   | Required?   | Description                                                                |
| ----------- | ---------- | ------------- | ------------------ | ------------|--------------------------------------------------------------------------- |
| projectId   | Path Param | string        | Limited by type    | Yes         | Project id for which to create a new punch list item.                      |
| type        | body       | KeywordDto    | 255 (keyword name) | Yes         | The type of punchlist item (we support custom values)                      |
| spaceId     | body       | string        | Limited by type    | Yes         | The space id of punchlist item                                             |
| location    | body       | KeywordDto    | 255 (keyword name) | Yes         | The location of punchlist item                                             |
| element     | body       | KeywordDto    | 255 (keyword name) | Yes         | The element of the location in punchlist item (we support custom values)   |
| description | body       | KeywordDto    | 255 (keyword name) | Yes         | The description of element in punchlist item (we support custom values)    |
| assignedTo  | body       | string[]      | Limited by type    | No          | The users ids to which the punch list item is asigned                      |
| capturedBy  | body       | string        | Limited by type    | No          | The user id which captured the punch list item                             |
| members     | body       | string[]      | Limited by type    | No          | The members ids of the punch list item                                     |
| dueDate     | body       | string        | Limited by type    | No          | The due date of the punch list item                                        |
| specSection | body       | KeywordDto    | 255 (keyword name) | No          | The spec section of the punch list item (we support custom values)         |
| date        | body       | string        | Limited by type    | Yes         | The recorded date for punch list item                                      |
| discipline  | body       | KeywordDto    | 255 (keyword name) | Yes         | The discipline of the punch list item (we support custom values).          |
| purpose     | body       | KeywordDto    | 255 (keyword name) | Yes         | The purpose of the punch list item (we support custom values).             |
| status      | body       | KeywordDto    | 255 (keyword name) | Yes         | The status of the punch list item (from predefined list in NPC).           |
| keywords    | body       | KeywordDto[]  | 255 (keyword name) | No          | The keywords of the punch list item                                        |
| remarks     | body       | string        | 65454              | No          | The remarks of the punch list item                                         |
| source      | body       | SourceInfoDto | See below          | No          | The source for providing primary key and descriptor from connector         |
| id          | body       | string        | 128                | Conditional | The ID is a punchlist item number. It's a unique value within the project. |


##### Additional info

ID
The behaviour of the ID field is based on the numbering settings within Activity Center Setup. It holds unique values for punch list items within a project. If a user provides a custom value 

SourceInfoDto

The `SourceInfoDto` is a structure created for external services to store information that identify the object in their system. The pair has to be unique within a project. The values can be anything less then 255 characters. 

| Name             | Type   |  Character Limit | Required? | Description                             |
| ---------------- | ------ | ---------------- | --------- | --------------------------------------- |
| sourcePrimaryKey | string | 255              | Yes       | Our clients external primary key value  |
| sourceDescriptor | string | 255              | Yes       | Our clients description for the key     |

Notes:
- `sourcePrimaryKey` and `sourceDescriptor` work as a pair and as a pair they have to be unique within a project.
- `sourcePrimaryKey` and `sourceDescriptor` are required within the `source`. The `source` is not a required field. 

```typescript
{
  sourcePrimaryKey: string;
  sourceDescriptor: string;
}
``` 


```typescript
{
  type: Keyword<KeywordType.Generic>;
  spaceId: string;
  location: Keyword<Location>;
  element: Keyword<KeywordType.Generic>;
  description: Keyword<KeywordType.Generic>;
  assignedTo?: string[];
  capturedBy?: string;
  members?: string[];
  dueDate?: string;
  specSection?: Keyword<KeywordType.Generic>;
  date: string;
  discipline: Keyword<KeywordType.Generic>;
  purpose: Keyword<KeywordType.Generic>;
  status: Keyword<PunchlistItemStatus>;
  keywords?: Keyword<KeywordType.Generic>[];
  remarks?: string;
  source?: SourceInfo;
  id?: string;
}
```

Sample request body:

```json
{
  "type": {
    "name": "Punch List",
    "type": "generic"
  },
  "spaceId": "95bec6f1-6121-4992-8de3-2064ffeef9af",
  "location": {
    "name": "North",
    "type": "generic",
  },
  "element": {
    "name": "Element",
    "type": "generic"
  },
  "description": {
    "name": "Description",
    "type": "generic"
  },
  "assignedTo": ["0000f160-89f3-47e1-a942-0cb2869edf8c"],
  "capturedBy": "0000f160-89f3-47e1-a942-0cb2869edf8c",
  "members": ["acb7cc;054eb07f-d7cb-464c-9a3f-2efaec26e6f3", "95bec6f1-6121-4992-8de3-2064ffeef9af"],
  "dueDate":  "2022-03-15T12:03:34.296Z",
  "specSection": {
    "name": "00 00 10",
    "type": "generic"
  },
  "date":  "2022-03-15T12:03:34.296Z",
  "discipline": {
    "name": "Architectural",
    "type": "generic"
  },
  "purpose": {
    "name": "Architectural",
    "type": "generic"
  },
  "status":  {
    "name": "Open",
    "type": "open"
  },
  "keywords": [
    {
      "name": "Design",
      "type": "generic"
    },
  ],
  "remarks": "remarks",
  "source": {
    "sourcePrimaryKey": "123",
    "sourceDescriptor": "test"
  },
  "id": "00001"
}
```

#### Response

| Name | Description                         | Type   |
| ---- | ----------------------------------- | ------ |
| id   | Guid of the created punch list item | string |


```typescript
{
  id: string
}
```

Excample:

```json
{      
  "id": "c9a77953-0b1a-464f-8fde-fa5da4607834"
}
```

#### Statuses codes
##### Success

| Code | Status Message | Description |
| ---- | -------------- | ---------------------------------------------- |
| 201  | Created        | The punch list item was created                |

##### Errors

| HTTP Status | Status Message        | Description                                                                                |
| ----------- | --------------------- | ------------------------------------------------------------------------------------------ |
| 400         | Bad Request           | Requested payload is empty                                                                 |
| 400         | Bad Request           | The 'projectId' must be a valid GUID.                                                      |
| 400         | Bad Request           | The 'spaceId' field must be a valid GUID.                                                  |
| 400         | Bad Request           | The 'spaceId' field is required.                                                           |
| 400         | Bad Request           | Exceeded maximum length of 128 characters for 'id' property.                               |
| 400         | Bad Request           | The 'type' field is required.                                                              |
| 400         | Bad Request           | The 'name' property is required.                                                           |
| 400         | Bad Request           | The 'type' property is required.                                                           |
| 400         | Bad Request           | The 'type' property of keyword is not of type 'generic'.                                   |
| 400         | Bad Request           | Exceeded maximum length of 255 characters for 'name' property.                             |
| 400         | Bad Request           | The 'location' field is required.                                                          |
| 400         | Bad Request           | The 'element' field is required.                                                           |
| 400         | Bad Request           | The 'description' field is required.                                                       |
| 400         | Bad Request           | The 'discipline' field is required.                                                        |
| 400         | Bad Request           | The 'purpose' field is required.                                                           |
| 400         | Bad Request           | The 'status' field is required.                                                            |
| 400         | Bad Request           | The 'type' property must be set to 'draft', 'open', 'closed', or 'void'.                   |
| 400         | Bad Request           | The 'assignedTo' element of array must be a valid GUID.                                    |
| 400         | Bad Request           | The 'assignedTo' must be an array.                                                         |
| 400         | Bad Request           | The 'capturedBy' field must be a valid GUID.                                               |
| 400         | Bad Request           | The 'members' element of array must be a valid GUID.                                       |
| 400         | Bad Request           | The 'members' must be an array.                                                            |
| 400         | Bad Request           | The 'date' field is required.                                                              |
| 400         | Bad Request           | The 'keywords' must be an array.                                                           |
| 400         | Bad Request           | The 'keywords' must be an array of 'generic' keywords.                                     |
| 400         | Bad Request           | Exceeded maximum length of 65535 characters for 'remarks' property.                        |
| 400         | Bad Request           | The 'sourcePrimaryKey' field is required.                                                  |
| 400         | Bad Request           | Exceeded maximum length of 255 characters for 'sourcePrimaryKey' property.                 |
| 400         | Bad Request           | The 'sourceDescriptor' field is required.                                                  |
| 400         | Bad Request           | Exceeded maximum length of 255 characters for 'sourceDescriptor' property.                 |
| 403         | Forbidden             | User is not licensed for Project Center.                                                   |
| 401         | Unauthorized          | Required authentication information is either missing or invalid for the resource.         |
| 403         | Forbidden             | Access is denied to the requested resource. The user might not have enough permission.     |
| 403         | Forbidden             | The user does not have a professional user role.                                           |
| 404         | Not Found             | Project not found: projectId                                                               |
| 404         | Not Found             | The `spaceId` not found: spaceId                                                           |
| 404         | Not Found             | The `assignedTo` contact GUID was not found: contactId                                     |
| 404         | Not Found             | The `members` contact GUID was not found: contactId                                        |
| 404         | Not Found             | The `capturedBy` contact GUID was not found: contactId                                     |
| 409         | Conflict              | Source primary key and source descriptor already exist                                     |
| 409         | Conflict              | The `punchListItem` with `id`: id already exist                                            |
| 500         | Internal Server Error | There was an internal server error while processing the request.                           |

#### Licensing

- NCM (Newforma Contract Management) licensing is required for this endpoint.
- The user must have professional user role to use the endpoint.

#### Examples

<details>
  <summary><b>POST /projects/c9a77953-0b1a-464f-8fde-fa5da4607834/punchlistitems</b></summary>
  
  Request body
  ```json
  {
    "type": {
      "name": "Punch List",
      "type": "generic"
    },
    "spaceId": "95bec6f1-6121-4992-8de3-2064ffeef9af",
    "location": {
      "name": "North",
      "type": "generic",
    },
    "element": {
      "name": "Element",
      "type": "generic"
    },
    "description": {
      "name": "Description",
      "type": "generic"
    },
    "assignedTo": ["0000f160-89f3-47e1-a942-0cb2869edf8c"],
    "capturedBy": "0000f160-89f3-47e1-a942-0cb2869edf8c",
    "members": ["acb7cc;054eb07f-d7cb-464c-9a3f-2efaec26e6f3", "95bec6f1-6121-4992-8de3-2064ffeef9af"],
    "dueDate":  "2022-03-15T12:03:34.296Z",
    "specSection": {
      "name": "00 00 10",
      "type": "generic"
    },
    "date":  "2022-03-15T12:03:34.296Z",
    "discipline": {
      "name": "Architectural",
      "type": "generic"
    },
    "purpose": {
      "name": "Architectural",
      "type": "generic"
    },
    "status":  {
      "name": "Open",
      "type": "open"
    },
    "keywords": [
      {
        "name": "Design",
        "type": "generic"
      },
    ],
    "remarks": "remarks",
    "source": {
      "sourcePrimaryKey": "123",
      "sourceDescriptor": "test"
    },
    "id": "100"
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
  <summary><b>POST /projects/995ff699-26b8-467e-a212-1bd1dcedff36/punchlistitems</b></summary>

  Http 400 BadRequest
  
```json
{
  "ErrorMessage": "The request is invalid.",
  "ModelState": {
    "model.capturedBy": [
      "Captured By must be a valid Guid"
    ]
  }
}
```
</details>

<details>
  <summary><b>POST /projects/995ff699-26b8-467e-a212-1bd1dcedff36/punchlistitems</b></summary>

  Http 400 BadRequest
  
```json
{
  "ErrorMessage": "The request is invalid.",
  "ModelState": {
    "model.members[0]": [
      "Not a valid Guid"
    ]
  }
}
```
</details>

<details>
  <summary><b>POST /projects/995ff699-26b8-467e-a212-1bd1dcedff36/punchlistitems</b></summary>

  Http 400 BadRequest
  
```json
{
  "ErrorMessage": "The request is invalid.",
  "ModelState": {
    "model.capturedBy": [
      "Captured By must be a valid Guid"
    ]
  }
}
```
</details>

<details>
  <summary><b>POST /projects/995ff699-26b8-467e-a212-1bd1dcedff36/punchlistitems</b></summary>

  Http 400 BadRequest
  
```json
{
  "ErrorMessage": "The request is invalid.",
  "ModelState": {
    "model.spaceId": [
      "Space Id must be a valid Guid"
    ]
  }
}
```
</details>

<details>
  <summary><b>POST /projects/995ff699-26b8-467e-a212-1bd1dcedff36/punchlistitems</b></summary>

  Http 400 BadRequest
  
```json
{
    "ErrorMessage": "The request is invalid.",
    "ModelState": {
        "projectId": [
            "projectId must be a valid Guid"
        ]
    }
}
```
</details>

<details>
  <summary><b>POST /projects/995ff699-26b8-467e-a212-1bd1dcedff36/punchlistitems</b></summary>

  Http 400 BadRequest
  
```json
{
    "ErrorMessage": "The request is invalid.",
    "ModelState": {
        "model.spaceId": [
            "The spaceId field is required."
        ]
    }
}
```
</details>

<details>
  <summary><b>POST /projects/995ff699-26b8-467e-a212-1bd1dcedff36/punchlistitems</b></summary>

  Http 400 BadRequest
  
```json
{
    "ErrorMessage": "The request is invalid.",
    "ModelState": {
        "model.type": [
            "The type field is required."
        ]
    }
}
```
</details>

<details>
  <summary><b>POST /projects/995ff699-26b8-467e-a212-1bd1dcedff36/punchlistitems</b></summary>

  Http 400 BadRequest
  
```json
{
    "ErrorMessage": "The request is invalid.",
    "ModelState": {
        "model.location": [
            "The location field is required."
        ]
    }
}
```
</details>

<details>
  <summary><b>POST /projects/995ff699-26b8-467e-a212-1bd1dcedff36/punchlistitems</b></summary>

  Http 400 BadRequest
  
```json
{
    "ErrorMessage": "The request is invalid.",
    "ModelState": {
        "model.element": [
            "The element field is required."
        ]
    }
}
```
</details>

<details>
  <summary><b>POST /projects/995ff699-26b8-467e-a212-1bd1dcedff36/punchlistitems</b></summary>

  Http 400 BadRequest
  
```json
{
    "ErrorMessage": "The request is invalid.",
    "ModelState": {
        "model.description": [
            "The description field is required."
        ]
    }
}
```
</details>

<details>
  <summary><b>POST /projects/995ff699-26b8-467e-a212-1bd1dcedff36/punchlistitems</b></summary>

  Http 400 BadRequest
  
```json
{
    "ErrorMessage": "The request is invalid.",
    "ModelState": {
        "model.date": [
            "The date field is required."
        ]
    }
}
```
</details>

<details>
  <summary><b>POST /projects/995ff699-26b8-467e-a212-1bd1dcedff36/punchlistitems</b></summary>

  Http 400 BadRequest
  
```json
{
    "ErrorMessage": "The request is invalid.",
    "ModelState": {
        "model.discipline": [
            "The discipline field is required."
        ]
    }
}
```
</details>

<details>
  <summary><b>POST /projects/995ff699-26b8-467e-a212-1bd1dcedff36/punchlistitems</b></summary>

  Http 400 BadRequest
  
```json
{
    "ErrorMessage": "The request is invalid.",
    "ModelState": {
        "model.purpose": [
            "The purpose field is required."
        ]
    }
}
```
</details>

<details>
  <summary><b>POST /projects/995ff699-26b8-467e-a212-1bd1dcedff36/punchlistitems</b></summary>

  Http 400 BadRequest
  
```json
{
    "ErrorMessage": "The request is invalid.",
    "ModelState": {
        "model.status": [
            "The status field is required."
        ]
    }
}
```
</details>

<details>
  <summary><b>POST /projects/995ff699-26b8-467e-a212-1bd1dcedff36/punchlistitems</b></summary>

  Http 400 BadRequest
  
```json
{
  "ErrorMessage": "The request is invalid.",
  "ModelState": {
    "model.type.type": [
      "keyword is not of generic type"
    ]
  }
}
```
</details>

<details>
  <summary><b>POST /projects/995ff699-26b8-467e-a212-1bd1dcedff36/punchlistitems</b></summary>

  Http 400 BadRequest
  
```json
{
  "ErrorMessage": "The request is invalid.",
  "ModelState": {
    "model.location.type": [
      "keyword is not of generic type"
    ]
  }
}
```
</details>

<details>
  <summary><b>POST /projects/995ff699-26b8-467e-a212-1bd1dcedff36/punchlistitems</b></summary>

  Http 400 BadRequest
  
```json
{
  "ErrorMessage": "The request is invalid.",
  "ModelState": {
    "model.element.type": [
      "keyword is not of generic type"
    ]
  }
}
```
</details>

<details>
  <summary><b>POST /projects/995ff699-26b8-467e-a212-1bd1dcedff36/punchlistitems</b></summary>

  Http 400 BadRequest
  
```json
{
  "ErrorMessage": "The request is invalid.",
  "ModelState": {
    "model.description.type": [
      "keyword is not of generic type"
    ]
  }
}
```
</details>

<details>
  <summary><b>POST /projects/995ff699-26b8-467e-a212-1bd1dcedff36/punchlistitems</b></summary>

  Http 400 BadRequest
  
```json
{
  "ErrorMessage": "The request is invalid.",
  "ModelState": {
    "model.specSection.type": [
      "keyword is not of generic type"
    ]
  }
}
```
</details>

<details>
  <summary><b>POST /projects/995ff699-26b8-467e-a212-1bd1dcedff36/punchlistitems</b></summary>

  Http 400 BadRequest
  
```json
{
  "ErrorMessage": "The request is invalid.",
  "ModelState": {
    "model.discipline.type": [
      "keyword is not of generic type"
    ]
  }
}
```
</details>

<details>
  <summary><b>POST /projects/995ff699-26b8-467e-a212-1bd1dcedff36/punchlistitems</b></summary>

  Http 400 BadRequest
  
```json
{
  "ErrorMessage": "The request is invalid.",
  "ModelState": {
    "model.keywords[0].type": [
      "keyword is not of generic type"
    ]
  }
}
```
</details>

<details>
  <summary><b>POST /projects/995ff699-26b8-467e-a212-1bd1dcedff36/punchlistitems</b></summary>

  Http 400 BadRequest
  
```json
{
  "ErrorMessage": "The request is invalid.",
  "ModelState": {
    "model.purpose.type": [
      "keyword is not of generic type"
    ]
  }
}
```
</details>

<details>
  <summary><b>POST /projects/995ff699-26b8-467e-a212-1bd1dcedff36/punchlistitems</b></summary>

  Http 400 BadRequest
  
```json
{
  "ErrorMessage": "The request is invalid.",
  "ModelState": {
    "model.status.type": [
      "Requested value 'ope' was not found."
    ],
    "model.status": [
      "the type is not supported"
    ]
  }
}
```
</details>


<details>
  <summary><b>POST /projects/995ff699-26b8-467e-a212-1bd1dcedff36/punchlistitems</b></summary>

  Http 400 BadRequest
  
```json
{
  "ErrorMessage": "Types does not allow custom values"
}
```
</details>

<details>
  <summary><b>POST /projects/995ff699-26b8-467e-a212-1bd1dcedff36/punchlistitems</b></summary>

  Http 400 BadRequest
  
```json
{
  "ErrorMessage": "Locations does not allow custom values"
}
```
</details>

<details>
  <summary><b>POST /projects/995ff699-26b8-467e-a212-1bd1dcedff36/punchlistitems</b></summary>

  Http 400 BadRequest
  
```json
{
  "ErrorMessage": "Spec Section does not allow custom values"
}
```
</details>

<details>
  <summary><b>POST /projects/995ff699-26b8-467e-a212-1bd1dcedff36/punchlistitems</b></summary>

  Http 400 BadRequest
  
```json
{
  "ErrorMessage": "Discipline does not allow custom values"
}
```
</details>

<details>
  <summary><b>POST /projects/995ff699-26b8-467e-a212-1bd1dcedff36/punchlistitems</b></summary>

  Http 400 BadRequest
  
```json
{
  "ErrorMessage": "Purpose does not allow custom values"
}
```
</details>

<details>
  <summary><b>POST /projects/995ff699-26b8-467e-a212-1bd1dcedff36/punchlistitems</b></summary>

  Http 400 BadRequest
  
```json
{
  "ErrorMessage": "Status does not allow custom values"
}
```
</details>

<details>
  <summary><b>POST /projects/995ff699-26b8-467e-a212-1bd1dcedff36/punchlistitems</b></summary>

  Http 400 BadRequest
  
```json
{
  "ErrorMessage": "Keywords does not allow custom values"
}
```
</details>

<details>
  <summary><b>POST /projects/995ff699-26b8-467e-a212-1bd1dcedff36/punchlistitems</b></summary>

  Http 404 Not found
  
```json
{
  "ErrorMessage": "Space index item with GUID: 2d439f19-6b77-4fd8-be30-0fab7b163dc7 was not found in the project"
}
```
</details>

<details>
  <summary><b>POST /projects/995ff699-26b8-467e-a212-1bd1dcedff36/punchlistitems</b></summary>

  Http 404 Not found
  
```json
{
  "ErrorMessage": "The `assignedTo` contact GUID was not found: 2d439f19-6b77-4fd8-be30-0fab7b163dc6"
}
```
</details>

<details>
  <summary><b>POST /projects/995ff699-26b8-467e-a212-1bd1dcedff36/punchlistitems</b></summary>

  Http 404 Not found
  
```json
{
  "ErrorMessage": "The `members` contact GUID was not found: 2d439f19-6b77-4fd8-be30-0fab7b163dc6"
}
```
</details>

<details>
  <summary><b>POST /projects/995ff699-26b8-467e-a212-1bd1dcedff36/punchlistitems</b></summary>

  Http 404 Not found
  
```json
{
  "ErrorMessage": "The `capturedBy` contact GUID was not found: 2d439f19-6b77-4fd8-be30-0fab7b163dc6"
}
```
</details>

<details>
  <summary><b>POST /projects/995ff699-26b8-467e-a212-1bd1dcedff36/punchlistitems</b></summary>

  Http 409 Conflict
  
```json
{
  "ErrorMessage": "Punch List Item with ID 100 already exists"
}
```
</details>

<details>
  <summary><b>POST /projects/995ff699-26b8-467e-a212-1bd1dcedff36/punchlistitems</b></summary>

  Http 409 Conflict
  
```json
{
  "data": {
    "id": "7934a41f-21fe-46b0-a46b-e3f02250c11d",
    "source": {
      "sourcePrimaryKey": "primKey",
      "sourceDescriptor": "primDesc"
    }
  },
  "ErrorMessage": "Punchlist Item already exists"
}
```
</details>


#### Notes