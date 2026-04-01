# POST /api/v1/transaction/accountingTransaction

Open and cancel accounting transactions (orders/bills). A transaction represents a bill at a table or a standalone invoice.

**Auth:** `Authorization: Bearer <token>`

Sections: [GET](#get) | [UPDATE](#update) | [DELETE](#delete)

---

## GET

Fetch open transactions for a given area (table).

### Request fields

| Field | Type | Required | Description |
|---|---|:---:|---|
| `areaId` | number | yes | ID of the area (table) whose open transactions to fetch |

### Request example

```json
{
  "action": "GET",
  "data": {
    "areaId": 2
  }
}
```

### Response

`data.accountingTransactions` — array of open transaction objects:

| Field | Type | Description |
|---|---|---|
| `id` | string (UUID) | Unique transaction ID |
| `humanId` | string | Human-readable transaction number |
| `accountingTransactionState` | string | Transaction state (always `OPENED` in this response) |
| `type` | string | Transaction type: `RECEIPT` or `INVOICE` |
| `area` | object | Area (table) assigned to this transaction |
| `createTime` | string | Creation date/time |
| `createdByUserId` | number | ID of the user who created the transaction |
| `priceGross` | number | Total gross price of all entries on the bill |
| `transactionPriceAdjustmentCoefficient` | number | Price multiplier applied to all items (1 = no change, 0.8 = 20% discount) |
| `customerUuid` | string (UUID) | UUID of the associated member card (if any) |
| `memberCardId` | string | Physical card ID (if any) |
| `salePlaceId` | string (UUID) | UUID of the sale place |

### Code examples

HTTPie:
```bash
http POST $BASE_URL/api/v1/transaction/accountingTransaction \
  "Authorization:Bearer $TOKEN" \
  action=GET \
  data:='{"areaId":2}'
```

Kotlin:
```kotlin
val body = """
    {
        "action": "GET",
        "data": {
            "areaId": 2
        }
    }
""".trimIndent().toRequestBody("application/json".toMediaType())

OkHttpClient().newCall(
    Request.Builder()
        .url("$BASE_URL/api/v1/transaction/accountingTransaction")
        .addHeader("Authorization", "Bearer $TOKEN")
        .post(body)
        .build()
).execute().body?.string()
```

### Response examples

Opened transaction:

```json
{
  "success": true,
  "data": {
    "accountingTransactions": [
      {
        "id": "78c06986-3a94-4993-8b3b-09bbb73265da",
        "humanId": "100",
        "accountingTransactionState": "OPENED",
        "type": "RECEIPT",
        "area": {
          "id": 2,
          "numberOfSeats": 0
        },
        "createTime": "30.09.2022 17:07:48",
        "closeTime": null,
        "priceGross": 12.2,
        "priceNet": null,
        "priceVat": null,
        "transactionPriceAdjustmentCoefficient": 1,
        "salePlaceId": "a25805ba-dd76-4c08-8188-5e64bc2e1645",
        "customerUuid": "29cd0081-9e47-4cbf-b124-354f2d01fbec",
        "memberCardId": "123456",
        "footer": null
      }
    ]
  }
}
```

Closed transaction (after payment):

```json
{
  "success": true,
  "data": {
    "accountingTransactions": [
      {
        "id": "d9acb831-83be-4451-8b20-ba9d65f2e90d",
        "humanId": "101",
        "accountingTransactionState": "CLOSED",
        "type": "RECEIPT",
        "area": null,
        "createTime": 1775075755021,
        "closeTime": null,
        "priceGross": 1.00,
        "priceNet": null,
        "priceVat": null,
        "transactionPriceAdjustmentCoefficient": 1,
        "salePlaceId": "a25805ba-dd76-4c08-8188-5e64bc2e1645",
        "customerUuid": null,
        "memberCardId": null,
        "footer": null
      }
    ]
  }
}
```

---

## UPDATE

Open a new transaction (order/bill).

### Request fields

| Field | Type | Required | Description |
|---|---|:---:|---|
| `accountingTransactionType` | string | no | Transaction type: `RECEIPT` or `INVOICE`. See [transaction objects](transaction_objects.md). |
| `areaId` | number | no | ID of the table to associate with this transaction |
| `salePlaceId` | string (UUID) | no | ID of the sale place. If omitted, the first available sale place is used. |
| `memberCardId` | string | no | Physical card ID of the customer |
| `name` | string | no | Custom name for the transaction |
| `invoiceNumber` | string | no | Invoice number (for `INVOICE` type) |
| `footer` | string | no | Custom text printed at the bottom of the receipt |

### Request examples

Quick receipt at a table:

```json
{
  "action": "UPDATE",
  "data": {
    "accountingTransactionType": "RECEIPT",
    "areaId": 9
  }
}
```

Invoice at a specific sale place with a customer card:

```json
{
  "action": "UPDATE",
  "data": {
    "accountingTransactionType": "INVOICE",
    "invoiceNumber": "INV44331",
    "memberCardId": "12345",
    "name": "invoice",
    "salePlaceId": "a25805ba-dd76-4c08-8188-5e64bc2e1645",
    "footer": "Thank you for your visit!"
  }
}
```

### Code examples

HTTPie — quick receipt at a table:
```bash
http POST $BASE_URL/api/v1/transaction/accountingTransaction \
  "Authorization:Bearer $TOKEN" \
  action=UPDATE \
  data:='{"accountingTransactionType":"RECEIPT","areaId":9}'
```

HTTPie — invoice with customer card:
```bash
http POST $BASE_URL/api/v1/transaction/accountingTransaction \
  "Authorization:Bearer $TOKEN" \
  action=UPDATE \
  data:='{"accountingTransactionType":"INVOICE","invoiceNumber":"INV44331","memberCardId":"12345","salePlaceId":"a25805ba-dd76-4c08-8188-5e64bc2e1645"}'
```

Kotlin:
```kotlin
// Quick receipt at a table
val body = """
    {
        "action": "UPDATE",
        "data": {
            "accountingTransactionType": "RECEIPT",
            "areaId": 9
        }
    }
""".trimIndent().toRequestBody("application/json".toMediaType())

OkHttpClient().newCall(
    Request.Builder()
        .url("$BASE_URL/api/v1/transaction/accountingTransaction")
        .addHeader("Authorization", "Bearer $TOKEN")
        .post(body)
        .build()
).execute().body?.string()

// Invoice with customer card
val body2 = """
    {
        "action": "UPDATE",
        "data": {
            "accountingTransactionType": "INVOICE",
            "invoiceNumber": "INV44331",
            "memberCardId": "12345",
            "salePlaceId": "a25805ba-dd76-4c08-8188-5e64bc2e1645"
        }
    }
""".trimIndent().toRequestBody("application/json".toMediaType())

OkHttpClient().newCall(
    Request.Builder()
        .url("$BASE_URL/api/v1/transaction/accountingTransaction")
        .addHeader("Authorization", "Bearer $TOKEN")
        .post(body2)
        .build()
).execute().body?.string()
```

### Response

Returns the created transaction in `data.accountingTransactions` using the same format as GET.

### Response example

```json
{
  "success": true,
  "data": {
    "accountingTransactions": [
      {
        "id": "b93045af-3fd5-4bc5-92a4-91f346e1cc5e",
        "humanId": "103",
        "accountingTransactionState": "OPENED",
        "area": {
          "id": 9,
          "numberOfSeats": 4
        },
        "createTime": "30.09.2022 17:10:00",
        "createdByUserId": 1,
        "priceGross": 0,
        "transactionPriceAdjustmentCoefficient": 1,
        "salePlaceId": "a25805ba-dd76-4c08-8188-5e64bc2e1645"
      }
    ]
  }
}
```

---

## DELETE

Cancel a transaction. Cancelling a transaction also cancels all entries on it.

### Request fields

| Field | Type | Required | Description |
|---|---|:---:|---|
| `id` | string (UUID) | yes | ID of the transaction to cancel |

### Request example

```json
{
  "action": "DELETE",
  "data": {
    "id": "d703ca6d-c57d-408a-a449-36bdfd44a0eb"
  }
}
```

### Code examples

HTTPie:
```bash
http POST $BASE_URL/api/v1/transaction/accountingTransaction \
  "Authorization:Bearer $TOKEN" \
  action=DELETE \
  data:='{"id":"d703ca6d-c57d-408a-a449-36bdfd44a0eb"}'
```

Kotlin:
```kotlin
val body = """
    {
        "action": "DELETE",
        "data": {
            "id": "d703ca6d-c57d-408a-a449-36bdfd44a0eb"
        }
    }
""".trimIndent().toRequestBody("application/json".toMediaType())

OkHttpClient().newCall(
    Request.Builder()
        .url("$BASE_URL/api/v1/transaction/accountingTransaction")
        .addHeader("Authorization", "Bearer $TOKEN")
        .post(body)
        .build()
).execute().body?.string()
```

### Response

Returns the cancelled transaction in `data.accountingTransactions` using the same format as GET.
