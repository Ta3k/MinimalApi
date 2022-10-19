# Delete Space

## Table of Contents

- [Delete Space](#delete-space)
  - [Table of Contents](#table-of-contents)
  - [Endpoints](#endpoints)
    - [Delete Space](#delete-space-1)
      - [DELETE /projects/{projectId}/spaces/{spaceId}](#delete-projectsprojectidspacesspaceid)
      - [Request](#request)
        - [DELETE /projects/a03bd562-7e9d-4751-8a56-f36e435dcac1/spaces/2d439f19-6b77-4fd8-be30-0fab7b163dc6](#delete-projectsa03bd562-7e9d-4751-8a56-f36e435dcac1spaces2d439f19-6b77-4fd8-be30-0fab7b163dc6)
        - [Path parameters](#path-parameters)
    - [Response](#response)
      - [Http Status code](#http-status-code)
      - [Success](#success)
      - [Error codes](#error-codes)
    - [Examples](#examples)
    - [Licensing](#licensing)

## Endpoints

### Delete Space

- Epic - [NPC-17164 - Spatial Index API](https://newforma.atlassian.net/browse/NPC-17164)
- Task - [NPC-17162 - [API] Delete Space](https://newforma.atlassian.net/browse/NPC-17162)

This endpoint used to delete a space within the project.

#### DELETE /projects/{projectId}/spaces/{spaceId}

#### Request

##### DELETE /projects/a03bd562-7e9d-4751-8a56-f36e435dcac1/spaces/2d439f19-6b77-4fd8-be30-0fab7b163dc6

##### Path parameters

| Name      | Type         | Required? | Description                     |
| --------- | ------------ | --------- | ------------------------------- |
| projectId | string(guid) | Yes       | Specifies the project id        |
| spaceId   | string(guid) | Yes       | Specifies the space id to delete|

### Response

#### Http Status code

#### Success

| HTTP Status | Status Message | Description                                  |
| ----------- | -------------- | -------------------------------------------- |
| 204         | No content     | The space item has been deleted              |

#### Error codes

| HTTP Status | Status Message        | Description                                                                                      |
| ----------- | --------------------- | ------------------------------------------------------------------------------------------------ |
| 400         | Bad Request           | The 'projectId' must be a valid GUID.                                                            |
| 400         | Bad Request           | The 'spaceId' field must be a valid GUID.                                                        |
| 401         | Unauthorized          | Required authentication information is either missing or invalid for the resource.               |
| 403         | Forbidden             | Permission to project with id: `projectId` is denied.                                            |
| 403         | Forbidden             | The space cannot be deleted in project. The project is in read only mode.                            |
| 403         | Forbidden             | You cannot delete space in project. Your license does not permit you to perform this action.     |
| 403         | Forbidden             | The space cannot be deleted in project. Project settings do not allow non-admins to delete items.    |
| 404         | Not Found             | The project with id: `projectId` cannot be found.                                                    |
| 404         | Not Found             | The space with id: `spaceId` cannot be found in project with id: `projectId`.                        |
| 409         | Conflict              | Unable to delete the space with id: `spaceId`. It has one or more associated Punch List item(s). |
| 500         | Internal Server Error | There was an internal server error while processing the request.                                 |

### Examples

<details>
 <summary><b>DELETE /projects/a03bd562-7e9d-4751-8a56-f36e435dcac1/spaces/2d439f19-6b77-4fd8-be30-0fab7b163dc6</b></summary> 

 Http 204 NoContent
 The space item has been deleted.
 </details>

 <details>
 <summary><b>DELETE /projects/a03bd562-7e9d-4751-8a56-f36e435dcac1/spaces/2d439f19-6b77-4fd8-be30-0fab7b163dc6</b></summary> 

 Http 400 Bad Request


 ```json
{
  "ErrorMessage": "Cannot process the request projectId isn't correct guid value."
}
```
 </details>

 <details>
 <summary><b>DELETE /projects/a03bd562-7e9d-4751-8a56-f36e435dcac1/spaces/2d439f19-6b77-4fd8-be30-0fab7b163dc6</b></summary> 

 Http 400 Bad Request


 ```json
{
  "ErrorMessage": "Cannot process the request spaceId isn't correct guid value."
}
```
 </details>

<details>
 <summary><b>DELETE /projects/a03bd562-7e9d-4751-8a56-f36e435dcac1/spaces/2d439f19-6b77-4fd8-be30-0fab7b163dc6</b></summary> 

 Http 401 Unauthorized


 ```json
{
  "ErrorMessage": "Required authentication information is either missing or invalid for the resource."
}
```
 </details>

 <details>
 <summary><b>DELETE /projects/a03bd562-7e9d-4751-8a56-f36e435dcac1/spaces/2d439f19-6b77-4fd8-be30-0fab7b163dc6</b></summary> 

 Http 403 Forbidden


 ```json
{
  "ErrorMessage": "Permission to project with id: projectId is denied."
}
```
 </details>

  <details>
 <summary><b>DELETE /projects/a03bd562-7e9d-4751-8a56-f36e435dcac1/spaces/2d439f19-6b77-4fd8-be30-0fab7b163dc6</b></summary> 

 Http 403 Forbidden


 ```json
{
  "ErrorMessage": "The space cannot be deleted in project. The project is in read only mode."
}
```
 </details>

   <details>
 <summary><b>DELETE /projects/a03bd562-7e9d-4751-8a56-f36e435dcac1/spaces/2d439f19-6b77-4fd8-be30-0fab7b163dc6</b></summary> 

 Http 403 Forbidden


 ```json
{
  "ErrorMessage": "You cannot delete space in project. Your license does not permit you to perform this action."
}
```
 </details>

<details>
 <summary><b>DELETE /projects/a03bd562-7e9d-4751-8a56-f36e435dcac1/spaces/2d439f19-6b77-4fd8-be30-0fab7b163dc6</b></summary> 

 Http 404 Not found


 ```json
{
  "ErrorMessage": "The project with id: projectId cannot be found."
}
```
 </details>

<details>
 <summary><b>DELETE /projects/a03bd562-7e9d-4751-8a56-f36e435dcac1/spaces/2d439f19-6b77-4fd8-be30-0fab7b163dc6</b></summary> 

 Http 404 Not found


 ```json
{
  "ErrorMessage": "The space with id: spaceId cannot be found in project with id: projectId."
}
```
 </details>

  Http 409 Conflict


 ```json
{
  "ErrorMessage": "Unable to delete the space with id: spaceId. It has one or more associated Punch List item(s)."
}
```
 </details>

 #### Licensing

- The user must have professional user role to use the endpoint.