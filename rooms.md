# POST /api/v1/areas/rooms

CRUD operations for rooms (sectors, zones, etc.). Areas (tables) belong to rooms.

**Auth:** `Authorization: Bearer <token>`

Sections: [GET](#get) | [UPDATE](#update)

---

## GET

Fetch all rooms or a filtered subset by ID.

### Request fields

| Field | Type | Required | Description |
|---|---|:---:|---|
| `ids` | `number[]` | no | IDs of rooms to fetch. Omit or send empty array to get all rooms. |

### Request examples

All rooms:

```json
{
  "action": "GET",
  "data": {}
}
```

Specific rooms by ID:

```json
{
  "action": "GET",
  "data": {
    "ids": [1, 2]
  }
}
```

### Response

`data.rooms` — array of room objects:

| Field | Type | Description |
|---|---|---|
| `id` | number | ID of the room |
| `name` | string | Name of the room |

### Response example

```json
{
  "success": true,
  "data": {
    "rooms": [
      {
        "id": 1,
        "name": "Bar"
      },
      {
        "id": 2,
        "name": "Terrace"
      }
    ]
  }
}
```

### Code examples

HTTPie — get all rooms:
```bash
http POST $BASE_URL/api/v1/areas/rooms \
  "Authorization:Bearer $TOKEN" \
  action=GET \
  data:='{}'
```

Kotlin:
```kotlin
println(papayaPost("/api/v1/areas/rooms", "GET"))
```

---

## UPDATE

Create or update a room.

- Request **without** `id` → creates a new room.
- Request **with** `id` → updates the existing room.

### Request fields

| Field | Type | Required | Description |
|---|---|:---:|---|
| `id` | number | no | ID of the room to update. Omit to create a new room. |
| `name` | string | no | Name of the room |

### Request examples

Create a new room:

```json
{
  "action": "UPDATE",
  "data": {
    "name": "new room"
  }
}
```

Update an existing room:

```json
{
  "action": "UPDATE",
  "data": {
    "id": 2,
    "name": "renamed room"
  }
}
```

### Response

Returns the created or updated room object in `data.rooms`.

### Response example

```json
{
  "success": true,
  "data": {
    "rooms": [
      {
        "id": 2,
        "name": "renamed room"
      }
    ]
  }
}
```

### Code examples

HTTPie — create room:
```bash
http POST $BASE_URL/api/v1/areas/rooms \
  "Authorization:Bearer $TOKEN" \
  action=UPDATE \
  data:='{"name":"new room"}'
```

HTTPie — update room:
```bash
http POST $BASE_URL/api/v1/areas/rooms \
  "Authorization:Bearer $TOKEN" \
  action=UPDATE \
  data:='{"id":2,"name":"renamed room"}'
```

Kotlin:
```kotlin
// Create room
println(papayaPost("/api/v1/areas/rooms", "UPDATE", """{"name":"new room"}"""))

// Update room
println(papayaPost("/api/v1/areas/rooms", "UPDATE", """{"id":2,"name":"renamed room"}"""))
```
