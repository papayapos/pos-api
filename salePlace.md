# POST /api/v1/salePlace

Read sale places (cashier/register positions).

**Auth:** `Authorization: Bearer <token>`

Sections: [GET](#get)

---

## GET

Fetch all sale places or a single one by UUID.

### Request fields

| Field | Type | Required | Description |
|---|---|:---:|---|
| `uuid` | string (UUID) | no | UUID of the sale place to fetch. Omit to get all sale places. |

### Request examples

All sale places:

```json
{
  "action": "GET",
  "data": {}
}
```

Single sale place:

```json
{
  "action": "GET",
  "data": {
    "uuid": "a25805ba-dd76-4c08-8188-5e64bc2e1645"
  }
}
```

### Response

`data.salePlaces` — array of sale place objects:

| Field | Type | Description |
|---|---|---|
| `uuid` | string (UUID) | UUID of the sale place |
| `name` | string | Name of the sale place |

### Response example

```json
{
  "success": true,
  "data": {
    "salePlaces": [
      {
        "uuid": "a25805ba-dd76-4c08-8188-5e64bc2e1645",
        "name": "Sale place"
      }
    ]
  }
}
```

### Code examples

HTTPie — get all sale places:
```bash
http POST $BASE_URL/api/v1/salePlace \
  "Authorization:Bearer $TOKEN" \
  action=GET \
  data:='{}'
```

HTTPie — get one sale place:
```bash
http POST $BASE_URL/api/v1/salePlace \
  "Authorization:Bearer $TOKEN" \
  action=GET \
  data:='{"uuid":"a25805ba-dd76-4c08-8188-5e64bc2e1645"}'
```

Kotlin:
```kotlin
// Get all sale places
val body = """
    {
        "action": "GET",
        "data": {}
    }
""".trimIndent().toRequestBody("application/json".toMediaType())

OkHttpClient().newCall(
    Request.Builder()
        .url("$BASE_URL/api/v1/salePlace")
        .addHeader("Authorization", "Bearer $TOKEN")
        .post(body)
        .build()
).execute().body?.string()

// Get one sale place
val body2 = """
    {
        "action": "GET",
        "data": {
            "uuid": "a25805ba-dd76-4c08-8188-5e64bc2e1645"
        }
    }
""".trimIndent().toRequestBody("application/json".toMediaType())

OkHttpClient().newCall(
    Request.Builder()
        .url("$BASE_URL/api/v1/salePlace")
        .addHeader("Authorization", "Bearer $TOKEN")
        .post(body2)
        .build()
).execute().body?.string()
```
