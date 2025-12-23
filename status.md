**Endpoint: /api/v1/status**

Responses to API requests that need to be reflected on android devices (for exaplme operations with accounting transactions) contain 
`operationId` field (UUID). This can be used to check, if they have been processed completely.

### GET

Checks if an operation is properly replicated into android devices.

**Request data**

ID of operation, taken from another API response

| field name  |     type      | Description     |
| :---------- | :-----------: | :-------------- |
| operationId | STRING (UUID) | id of operation |

**Request Example**

```json
{
  "action":"GET",
  "data": {
    "operationId":"e9d1a320-14e3-4fd6-ac58-a37da1e32139"
  }
}
```



**Response**

| field name              |     type      | Description                                                  |
| :---------------------- | :-----------: | :----------------------------------------------------------- |
| completed               |    Boolean    | true if all efects of the operation have been reflected into activated devices |



**Response Example**

```json
{
    "data": {
       "completed": true
    },
    "success": true
}
```
