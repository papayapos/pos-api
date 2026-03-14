# POST /api/v1/memberCard

CRUD operations for member/loyalty cards.

**Auth:** `Authorization: Bearer <token>`

Sections: [GET](#get) | [UPDATE](#update)

---

## GET

Fetch member cards.

- Sending empty `data` returns all non-deleted member cards.
- Sending `uuid` returns a single card (including deleted ones).

### Request fields

| Field | Type | Required | Description |
|---|---|:---:|---|
| `uuid` | string (UUID) | no | UUID of the member card to fetch. Omit to get all active cards. |

### Request examples

All active member cards:

```json
{
  "action": "GET",
  "data": {}
}
```

Single member card (including if deleted):

```json
{
  "action": "GET",
  "data": {
    "uuid": "ac9aef30-9c11-4f53-b59e-f2af98c3e13f"
  }
}
```

### Response

`data.memberCards` — array of member card objects:

| Field | Type | Description |
|---|---|---|
| `uuid` | string (UUID) | UUID of the member card |
| `firstName` | string | Customer's first name |
| `lastName` | string | Customer's last name |
| `email` | string | Customer's email address |
| `cardId` | string | Physical card ID (scanned by the Android app via NFC) |
| `discount` | number | Discount percentage applied to this customer's orders |
| `deleted` | boolean | `true` if the card has been soft-deleted. Returned only when queried by UUID. |

### Code examples

HTTPie — get all cards:
```bash
http POST $BASE_URL/api/v1/memberCard \
  "Authorization:Bearer $TOKEN" \
  action=GET \
  data:='{}'
```

HTTPie — get single card:
```bash
http POST $BASE_URL/api/v1/memberCard \
  "Authorization:Bearer $TOKEN" \
  action=GET \
  data:='{"uuid":"ac9aef30-9c11-4f53-b59e-f2af98c3e13f"}'
```

Kotlin:
```kotlin
// Get all active member cards
val body = """
    {
        "action": "GET",
        "data": {}
    }
""".trimIndent().toRequestBody("application/json".toMediaType())

OkHttpClient().newCall(
    Request.Builder()
        .url("$BASE_URL/api/v1/memberCard")
        .addHeader("Authorization", "Bearer $TOKEN")
        .post(body)
        .build()
).execute().body?.string()

// Get single card by UUID
val body2 = """
    {
        "action": "GET",
        "data": {
            "uuid": "ac9aef30-9c11-4f53-b59e-f2af98c3e13f"
        }
    }
""".trimIndent().toRequestBody("application/json".toMediaType())

OkHttpClient().newCall(
    Request.Builder()
        .url("$BASE_URL/api/v1/memberCard")
        .addHeader("Authorization", "Bearer $TOKEN")
        .post(body2)
        .build()
).execute().body?.string()
```

### Response example

```json
{
  "success": true,
  "data": {
    "memberCards": [
      {
        "uuid": "29cd0081-9e47-4cbf-b124-354f2d01fbec",
        "firstName": "John",
        "lastName": "Doe",
        "email": "john.doe@email.com",
        "cardId": "123",
        "discount": 5,
        "deleted": false
      }
    ]
  }
}
```

---

## UPDATE

Create or update a member card.

- Request **without** `uuid` → creates a new card with a random UUID. `firstName` and `lastName` are required.
- Request **with** `uuid`:
  - Card exists → updated using non-null values from the request.
  - Card does not exist → created with the provided UUID. `firstName` and `lastName` are required.
- To soft-delete a card, send `"deleted": true`.
- To restore a deleted card, send `"deleted": false`.

### Request fields

| Field | Type | Required | Description |
|---|---|:---:|---|
| `uuid` | string (UUID) | no | UUID of the card. Omit to auto-generate. |
| `firstName` | string | yes (create) | Customer's first name. Required when creating. |
| `lastName` | string | yes (create) | Customer's last name. Required when creating. |
| `email` | string | no | Customer's email address |
| `cardId` | string | no | Physical card ID (scanned by the Android app via NFC) |
| `discount` | number | no | Discount percentage (e.g. `5` for 5%) |
| `deleted` | boolean | no | Set to `true` to soft-delete, `false` to restore |

### Request examples

Create a new member card:

```json
{
  "action": "UPDATE",
  "data": {
    "firstName": "John",
    "lastName": "Doe",
    "cardId": "123",
    "discount": 5
  }
}
```

Soft-delete an existing card:

```json
{
  "action": "UPDATE",
  "data": {
    "uuid": "ac9aef30-9c11-4f53-b59e-f2af98c3e13f",
    "deleted": true
  }
}
```

### Code examples

HTTPie — create card:
```bash
http POST $BASE_URL/api/v1/memberCard \
  "Authorization:Bearer $TOKEN" \
  action=UPDATE \
  data:='{"firstName":"John","lastName":"Doe","cardId":"123","discount":5}'
```

HTTPie — soft-delete card:
```bash
http POST $BASE_URL/api/v1/memberCard \
  "Authorization:Bearer $TOKEN" \
  action=UPDATE \
  data:='{"uuid":"ac9aef30-9c11-4f53-b59e-f2af98c3e13f","deleted":true}'
```

Kotlin:
```kotlin
// Create new member card
val body = """
    {
        "action": "UPDATE",
        "data": {
            "firstName": "John",
            "lastName": "Doe",
            "cardId": "123",
            "discount": 5
        }
    }
""".trimIndent().toRequestBody("application/json".toMediaType())

OkHttpClient().newCall(
    Request.Builder()
        .url("$BASE_URL/api/v1/memberCard")
        .addHeader("Authorization", "Bearer $TOKEN")
        .post(body)
        .build()
).execute().body?.string()

// Soft-delete card
val body2 = """
    {
        "action": "UPDATE",
        "data": {
            "uuid": "ac9aef30-9c11-4f53-b59e-f2af98c3e13f",
            "deleted": true
        }
    }
""".trimIndent().toRequestBody("application/json".toMediaType())

OkHttpClient().newCall(
    Request.Builder()
        .url("$BASE_URL/api/v1/memberCard")
        .addHeader("Authorization", "Bearer $TOKEN")
        .post(body2)
        .build()
).execute().body?.string()
```

### Response

`data` — UUID string of the created or updated member card.

### Response example

```json
{
  "success": true,
  "data": "ac9aef30-9c11-4f53-b59e-f2af98c3e13f"
}
```
