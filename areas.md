# POST /api/v1/areas

CRUD operations for areas (tables).

**Auth:** `Authorization: Bearer <token>`

Sections: [GET](#get) | [UPDATE](#update)

---

## GET

Fetch all areas or a filtered subset by ID.

### Request fields

| Field | Type | Required | Description |
|---|---|:---:|---|
| `ids` | `number[]` | no | IDs of areas to fetch. Omit or send empty array to get all areas. |

### Request examples

All areas:

```json
{
  "action": "GET",
  "data": {}
}
```

Specific areas by ID:

```json
{
  "action": "GET",
  "data": {
    "ids": [3, 4, 5]
  }
}
```

### Response

`data.areas` — array of area objects:

| Field | Type | Description |
|---|---|---|
| `id` | number | ID of the area (table) |
| `name` | string | Name of the area |
| `numberOfSeats` | number | Number of seats at this area |
| `roomId` | number | ID of the room this area belongs to |

### Response example

```json
{
  "success": true,
  "data": {
    "areas": [
      {
        "id": 3,
        "name": "INT 01",
        "numberOfSeats": 0,
        "roomId": 1
      },
      {
        "id": 4,
        "name": "INT 02",
        "numberOfSeats": 0,
        "roomId": 1
      },
      {
        "id": 6,
        "name": "TER 01",
        "numberOfSeats": 0,
        "roomId": 2
      }
    ]
  }
}
```

### Code examples

HTTPie — get all areas:
```bash
http POST $BASE_URL/api/v1/areas \
  "Authorization:Bearer $TOKEN" \
  action=GET \
  data:='{}'
```

HTTPie — get specific areas:
```bash
http POST $BASE_URL/api/v1/areas \
  "Authorization:Bearer $TOKEN" \
  action=GET \
  data:='{"ids":[3,4,5]}'
```

Kotlin:
```kotlin
// Get all areas
println(papayaPost("/api/v1/areas", "GET"))

// Get specific areas
println(papayaPost("/api/v1/areas", "GET", """{"ids":[3,4,5]}"""))
```

---

## UPDATE

Create or update an area.

- Request **without** `id` → creates a new area (`roomId` required).
- Request **with** `id` → updates the existing area. `roomId` is optional; the area stays in its current room if not provided.

### Request fields

| Field | Type | Required | Description |
|---|---|:---:|---|
| `id` | number | no | ID of the area to update. Omit to create a new area. |
| `name` | string | no | Name of the area |
| `numberOfSeats` | number | no | Number of seats at this area |
| `roomId` | number | yes (create) | ID of the room this area belongs to. Required when creating. |

### Request examples

Create a new area:

```json
{
  "action": "UPDATE",
  "data": {
    "name": "new table",
    "roomId": 1
  }
}
```

Update an existing area:

```json
{
  "action": "UPDATE",
  "data": {
    "id": 4,
    "name": "renamed table",
    "numberOfSeats": 2
  }
}
```

### Response

Returns the created or updated area object in `data.areas`.

### Response example

```json
{
  "success": true,
  "data": {
    "areas": [
      {
        "id": 4,
        "name": "renamed table",
        "numberOfSeats": 2,
        "roomId": 1
      }
    ]
  }
}
```

### Code examples

HTTPie — create new area:
```bash
http POST $BASE_URL/api/v1/areas \
  "Authorization:Bearer $TOKEN" \
  action=UPDATE \
  data:='{"name":"new table","roomId":1}'
```

HTTPie — update existing area:
```bash
http POST $BASE_URL/api/v1/areas \
  "Authorization:Bearer $TOKEN" \
  action=UPDATE \
  data:='{"id":4,"name":"renamed table","numberOfSeats":2}'
```

Kotlin:
```kotlin
// Create new area
println(papayaPost("/api/v1/areas", "UPDATE", """{"name":"new table","roomId":1}"""))

// Update existing area
println(papayaPost("/api/v1/areas", "UPDATE", """{"id":4,"name":"renamed table","numberOfSeats":2}"""))
```
