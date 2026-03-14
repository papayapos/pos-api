# POST /api/v1/transaction/accountingEntry

Add, fetch, and cancel entries (line items) on a transaction/bill.

**Auth:** `Authorization: Bearer <token>`

Sections: [GET](#get) | [UPDATE](#update) | [DELETE](#delete)

---

## GET

Fetch all entries on a given transaction.

### Request fields

| Field | Type | Required | Description |
|---|---|:---:|---|
| `accountingTransactionId` | string (UUID) | yes | ID of the transaction whose entries to fetch |

### Request example

```json
{
  "action": "GET",
  "data": {
    "accountingTransactionId": "a7e92c6d-8db5-4753-8613-de50a6b710ea"
  }
}
```

### Response

`data.accountingEntries` — array of accounting entry objects:

| Field | Type | Description |
|---|---|---|
| `id` | string (UUID) | Entry ID |
| `accountingTransactionId` | string (UUID) | ID of the parent transaction |
| `accountableItemId` | string (UUID) | ID of the catalog item being sold |
| `title` | string | Item name as shown on the receipt |
| `accountingEntryState` | string | Entry state (e.g. `CREATED`, `CANCELED`) |
| `accountingEntryType` | string | Entry type (e.g. `SALE`, `REFUND`). See [transaction objects](transaction_objects.md). |
| `count` | number | Number of items (e.g. `2` croissants) |
| `quantity` | number | Quantity per item in the item's unit (informational) |
| `unit` | string | Unit of measurement (informational) |
| `priceAmount` | number | Original unit price |
| `priceAdjustCoef` | number | Entry-level discount multiplier (1 = no change, 0.8 = 20% off) |
| `txAdjustCoef` | number | Transaction-level discount multiplier |
| `adjustedPrice` | number | Final price after applying adjustment coefficients |
| `priceCurrency` | string | Currency code. See [transaction objects](transaction_objects.md). |
| `vatRateId` | number | Internal VAT rate ID |
| `vatRateValue` | number | VAT rate in % |
| `createTime` | string | Date/time the entry was created |

### Code examples

HTTPie:
```bash
http POST $BASE_URL/api/v1/transaction/accountingEntry \
  "Authorization:Bearer $TOKEN" \
  action=GET \
  data:='{"accountingTransactionId":"a7e92c6d-8db5-4753-8613-de50a6b710ea"}'
```

Kotlin:
```kotlin
val body = """
    {
        "action": "GET",
        "data": {
            "accountingTransactionId": "a7e92c6d-8db5-4753-8613-de50a6b710ea"
        }
    }
""".trimIndent().toRequestBody("application/json".toMediaType())

OkHttpClient().newCall(
    Request.Builder()
        .url("$BASE_URL/api/v1/transaction/accountingEntry")
        .addHeader("Authorization", "Bearer $TOKEN")
        .post(body)
        .build()
).execute().body?.string()
```

### Response example

```json
{
  "success": true,
  "data": {
    "accountingEntries": [
      {
        "id": "ac67a376-5c51-4b71-a6a9-2425f42bdff1",
        "accountingTransactionId": "a7e92c6d-8db5-4753-8613-de50a6b710ea",
        "accountableItemId": "b6aeb536-5829-41f1-ab86-aba8f69d614b",
        "title": "Green tea",
        "accountingEntryState": "CREATED",
        "accountingEntryType": "SALE",
        "count": 1,
        "quantity": 0.5,
        "unit": "l",
        "priceAmount": 20,
        "priceAdjustCoef": 1,
        "txAdjustCoef": 1,
        "adjustedPrice": 3.5,
        "priceCurrency": "EUR",
        "vatRateId": 1,
        "vatRateValue": 20,
        "createTime": "30.09.2022 17:08:03"
      }
    ]
  }
}
```

---

## UPDATE

Add an item to an open transaction.

### Request fields

| Field | Type | Required | Description |
|---|---|:---:|---|
| `accountingTransactionId` | string (UUID) | yes | ID of the transaction to add the item to |
| `accountableItemId` | string (UUID) | yes | ID of the catalog item to add |
| `count` | number | yes | Number of items to add |
| `price` | number | yes | Price per item |
| `state` | string | yes | Must be `CREATED` |
| `type` | string | yes | Must be `SALE` |

### Request example

```json
{
  "action": "UPDATE",
  "data": {
    "accountingTransactionId": "a7e92c6d-8db5-4753-8613-de50a6b710ea",
    "accountableItemId": "a7e92c6d-8db5-4753-8613-de50a6b71ttt",
    "count": 2,
    "price": 2.50,
    "state": "CREATED",
    "type": "SALE"
  }
}
```

### Code examples

HTTPie:
```bash
http POST $BASE_URL/api/v1/transaction/accountingEntry \
  "Authorization:Bearer $TOKEN" \
  action=UPDATE \
  data:='{"accountingTransactionId":"a7e92c6d-8db5-4753-8613-de50a6b710ea","accountableItemId":"b6aeb536-5829-41f1-ab86-aba8f69d614b","count":2,"price":3.50,"state":"CREATED","type":"SALE"}'
```

Kotlin:
```kotlin
val body = """
    {
        "action": "UPDATE",
        "data": {
            "accountingTransactionId": "a7e92c6d-8db5-4753-8613-de50a6b710ea",
            "accountableItemId": "b6aeb536-5829-41f1-ab86-aba8f69d614b",
            "count": 2,
            "price": 3.50,
            "state": "CREATED",
            "type": "SALE"
        }
    }
""".trimIndent().toRequestBody("application/json".toMediaType())

OkHttpClient().newCall(
    Request.Builder()
        .url("$BASE_URL/api/v1/transaction/accountingEntry")
        .addHeader("Authorization", "Bearer $TOKEN")
        .post(body)
        .build()
).execute().body?.string()
```

### Response

Returns the created entry in `data.accountingEntries` using the same format as GET.

---

## DELETE

Cancel an accounting entry. The entry is marked as `CANCELED` and is no longer included in the bill total.

### Request fields

| Field | Type | Required | Description |
|---|---|:---:|---|
| `id` | string (UUID) | yes | ID of the entry to cancel |

### Request example

```json
{
  "action": "DELETE",
  "data": {
    "id": "868d73c9-3b06-4642-a861-1b0d28d0de9d"
  }
}
```

### Code examples

HTTPie:
```bash
http POST $BASE_URL/api/v1/transaction/accountingEntry \
  "Authorization:Bearer $TOKEN" \
  action=DELETE \
  data:='{"id":"868d73c9-3b06-4642-a861-1b0d28d0de9d"}'
```

Kotlin:
```kotlin
val body = """
    {
        "action": "DELETE",
        "data": {
            "id": "868d73c9-3b06-4642-a861-1b0d28d0de9d"
        }
    }
""".trimIndent().toRequestBody("application/json".toMediaType())

OkHttpClient().newCall(
    Request.Builder()
        .url("$BASE_URL/api/v1/transaction/accountingEntry")
        .addHeader("Authorization", "Bearer $TOKEN")
        .post(body)
        .build()
).execute().body?.string()
```

### Response

Returns the cancelled entry in `data.accountingEntries` using the same format as GET.
