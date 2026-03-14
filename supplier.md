# POST /api/v1/supplier

CRUD operations for suppliers. Suppliers are informational and are used when creating inventory cards to record where goods came from. Supplier data is copied into each card at creation time, so deleting a supplier does not affect existing cards.

**Auth:** `Authorization: Bearer <token>`

Sections: [GET](#get) | [UPDATE](#update) | [DELETE](#delete)

---

## GET

Fetch all suppliers or filter by IDs.

- Sending empty `data` returns all suppliers.
- Non-existent IDs are silently ignored.

### Request fields

| Field | Type | Required | Description |
|---|---|:---:|---|
| `ids` | `number[]` | no | IDs of suppliers to fetch. Omit to get all suppliers. |

### Request examples

All suppliers:

```json
{
  "action": "GET",
  "data": {}
}
```

Specific suppliers by ID:

```json
{
  "action": "GET",
  "data": {
    "ids": [1]
  }
}
```

### Response

`data.suppliers` — array of supplier objects. See [supplier objects](supplier%20objects.md) for field definitions.

### Code examples

HTTPie — get all suppliers:
```bash
http POST $BASE_URL/api/v1/supplier \
  "Authorization:Bearer $TOKEN" \
  action=GET \
  data:='{}'
```

HTTPie — get by IDs:
```bash
http POST $BASE_URL/api/v1/supplier \
  "Authorization:Bearer $TOKEN" \
  action=GET \
  data:='{"ids":[1,2]}'
```

Kotlin:
```kotlin
// Get all suppliers
val body = """
    {
        "action": "GET",
        "data": {}
    }
""".trimIndent().toRequestBody("application/json".toMediaType())

OkHttpClient().newCall(
    Request.Builder()
        .url("$BASE_URL/api/v1/supplier")
        .addHeader("Authorization", "Bearer $TOKEN")
        .post(body)
        .build()
).execute().body?.string()

// Get by IDs
val body2 = """
    {
        "action": "GET",
        "data": {
            "ids": [1, 2]
        }
    }
""".trimIndent().toRequestBody("application/json".toMediaType())

OkHttpClient().newCall(
    Request.Builder()
        .url("$BASE_URL/api/v1/supplier")
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
    "suppliers": [
      {
        "id": 1,
        "name": "Silvia",
        "address": "Street no. 1",
        "city": "Bratislava",
        "country": "Slovakia",
        "contactPerson": "Elena",
        "email": "abc@cde.efg",
        "phone": "1234567890",
        "ico": "44444444",
        "icDph": "12345",
        "registration": true,
        "registrationDate": "12.05.2022 00:00:00",
        "registrationNumber": "55"
      }
    ]
  }
}
```

---

## UPDATE

Create or update suppliers. Both can be done in one request.

- Supplier **without** `id` → created with a generated ID.
- Supplier **with** `id`:
  - Already exists → all its fields are overwritten with the provided data.
  - Does not exist → created using the provided ID.

> When updating, all fields are overwritten. Even if you want to change only one field, send all data.

### Request fields

| Field | Type | Required | Description |
|---|---|:---:|---|
| `suppliers` | object[] | yes | Suppliers to create or update |

**Supplier object:**

| Field | Type | Required | Description |
|---|---|:---:|---|
| `id` | number | no | Supplier ID. Omit to auto-generate. |
| `name` | string | no | Supplier name |
| `address` | string | no | Street address |
| `city` | string | no | City |
| `country` | string | no | Country |
| `contactPerson` | string | no | Name of contact person |
| `phone` | string | no | Phone number |
| `email` | string | no | Email address |
| `ico` | string | no | Company registration number (IČO) |
| `dic` | string | no | Tax ID (DIČ) |
| `icDph` | string | no | VAT registration number (IČ DPH) |
| `registration` | boolean | no | Whether the supplier is registered |
| `registrationNumber` | string | no | Registration number |
| `registrationDate` | string | no | Registration date |

### Request example

```json
{
  "action": "UPDATE",
  "data": {
    "suppliers": [
      {
        "id": 1,
        "name": "Silvia",
        "address": "Street no. 1",
        "city": "Bratislava",
        "country": "Slovakia",
        "contactPerson": "Elena",
        "email": "abc@cde.efg",
        "phone": "1234567890",
        "ico": "44444444",
        "icDph": "12345",
        "registration": true,
        "registrationDate": "12.05.2022 00:00:00",
        "registrationNumber": "55"
      }
    ]
  }
}
```

### Code examples

HTTPie:
```bash
http POST $BASE_URL/api/v1/supplier \
  "Authorization:Bearer $TOKEN" \
  action=UPDATE \
  data:='{"suppliers":[{"id":1,"name":"Silvia","city":"Bratislava","country":"Slovakia","email":"abc@cde.efg"}]}'
```

Kotlin:
```kotlin
val body = """
    {
        "action": "UPDATE",
        "data": {
            "suppliers": [{
                "id": 1,
                "name": "Silvia",
                "city": "Bratislava",
                "country": "Slovakia",
                "email": "abc@cde.efg"
            }]
        }
    }
""".trimIndent().toRequestBody("application/json".toMediaType())

OkHttpClient().newCall(
    Request.Builder()
        .url("$BASE_URL/api/v1/supplier")
        .addHeader("Authorization", "Bearer $TOKEN")
        .post(body)
        .build()
).execute().body?.string()
```

### Response

Returns the created or updated suppliers in `data.suppliers`.

### Response example

```json
{
  "success": true,
  "data": {
    "suppliers": [
      {
        "id": 1,
        "name": "Silvia",
        "address": "Street no. 1",
        "city": "Bratislava",
        "country": "Slovakia",
        "contactPerson": "Julia",
        "email": "abc@cde.efg",
        "phone": "1234567890",
        "ico": "44444444",
        "icDph": "12345",
        "registration": true,
        "registrationDate": "12.05.2022 00:00:00",
        "registrationNumber": "55"
      }
    ]
  }
}
```

---

## DELETE

Delete suppliers by their IDs. Non-existent IDs are silently ignored. Deleting a supplier does not affect inventory cards that reference it.

### Request fields

| Field | Type | Required | Description |
|---|---|:---:|---|
| `ids` | `number[]` | yes | IDs of suppliers to delete |

### Request example

```json
{
  "action": "DELETE",
  "data": {
    "ids": [1, 2, 3, 4]
  }
}
```

### Code examples

HTTPie:
```bash
http POST $BASE_URL/api/v1/supplier \
  "Authorization:Bearer $TOKEN" \
  action=DELETE \
  data:='{"ids":[1,2]}'
```

Kotlin:
```kotlin
val body = """
    {
        "action": "DELETE",
        "data": {
            "ids": [1, 2]
        }
    }
""".trimIndent().toRequestBody("application/json".toMediaType())

OkHttpClient().newCall(
    Request.Builder()
        .url("$BASE_URL/api/v1/supplier")
        .addHeader("Authorization", "Bearer $TOKEN")
        .post(body)
        .build()
).execute().body?.string()
```

### Response

Returns the deleted suppliers in `data.suppliers`. If `success` is `false`, no suppliers were deleted.

### Response example

```json
{
  "success": true,
  "data": {
    "suppliers": [
      {
        "id": 1,
        "name": "Silvia",
        "address": "Street no. 1",
        "city": "Bratislava",
        "country": "Slovakia",
        "contactPerson": "Julia",
        "email": "abc@cde.efg",
        "phone": "1234567890",
        "ico": "44444444",
        "icDph": "12345",
        "registration": true,
        "registrationDate": "12.05.2022 00:00:00",
        "registrationNumber": "55"
      }
    ]
  }
}
```
