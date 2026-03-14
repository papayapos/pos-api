# POST /api/v1/transaction/discount

Apply a discount (or surcharge) to a single entry or to an entire transaction.

**Auth:** `Authorization: Bearer <token>`

Sections: [UPDATE](#update)

---

## UPDATE

Set the discount coefficient for an entry or transaction. A coefficient of `0.8` means the price becomes 80% of the original (20% discount). A value above `1` acts as a surcharge.

> Provide either `accountingEntryId` **or** `accountingTransactionId` — not both. If both are provided, only the entry discount is applied.

### Request fields

| Field | Type | Required | Description |
|---|---|:---:|---|
| `accountingEntryId` | string (UUID) | no | ID of the entry to discount |
| `accountingTransactionId` | string (UUID) | no | ID of the transaction to discount |
| `discountRate` | number | yes | Discount multiplier. Range 0–1 for a discount (e.g. `0.8` = 20% off). Values above 1 are a surcharge. |

### Request examples

Discount a single entry by 20%:

```json
{
  "action": "UPDATE",
  "data": {
    "accountingEntryId": "6b2b82c2-3423-4905-966a-d66811e0b88a",
    "discountRate": 0.8
  }
}
```

Discount an entire transaction by 20%:

```json
{
  "action": "UPDATE",
  "data": {
    "accountingTransactionId": "d18926c3-c4f1-4e1e-94a6-e47c2e5afa0d",
    "discountRate": 0.8
  }
}
```

### Response

- `data.adjustedAccountingEntry` — returned when an entry was discounted. See [EntryModel](transaction_objects.md).
- `data.adjustedAccountingTransaction` — returned when a transaction was discounted. See [TransactionModel](transaction_objects.md).

### Code examples

HTTPie — discount an entry:
```bash
http POST $BASE_URL/api/v1/transaction/discount \
  "Authorization:Bearer $TOKEN" \
  action=UPDATE \
  data:='{"accountingEntryId":"6b2b82c2-3423-4905-966a-d66811e0b88a","discountRate":0.8}'
```

HTTPie — discount a transaction:
```bash
http POST $BASE_URL/api/v1/transaction/discount \
  "Authorization:Bearer $TOKEN" \
  action=UPDATE \
  data:='{"accountingTransactionId":"d18926c3-c4f1-4e1e-94a6-e47c2e5afa0d","discountRate":0.8}'
```

Kotlin:
```kotlin
// Discount a single entry by 20%
val body = """
    {
        "action": "UPDATE",
        "data": {
            "accountingEntryId": "6b2b82c2-3423-4905-966a-d66811e0b88a",
            "discountRate": 0.8
        }
    }
""".trimIndent().toRequestBody("application/json".toMediaType())

OkHttpClient().newCall(
    Request.Builder()
        .url("$BASE_URL/api/v1/transaction/discount")
        .addHeader("Authorization", "Bearer $TOKEN")
        .post(body)
        .build()
).execute().body?.string()

// Discount entire transaction by 20%
val body2 = """
    {
        "action": "UPDATE",
        "data": {
            "accountingTransactionId": "d18926c3-c4f1-4e1e-94a6-e47c2e5afa0d",
            "discountRate": 0.8
        }
    }
""".trimIndent().toRequestBody("application/json".toMediaType())

OkHttpClient().newCall(
    Request.Builder()
        .url("$BASE_URL/api/v1/transaction/discount")
        .addHeader("Authorization", "Bearer $TOKEN")
        .post(body2)
        .build()
).execute().body?.string()
```

### Response examples

Entry discount response:

```json
{
  "success": true,
  "data": {
    "adjustedAccountingEntry": {
      "id": "6b2b82c2-3423-4905-966a-d66811e0b88a",
      "accountingTransactionId": "42d6b8b2-da39-4113-9b84-7d28c466184a",
      "title": "Croissant plnený šunkou a syrom",
      "accountingEntryState": "CREATED",
      "accountingEntryType": "SALE",
      "count": 2,
      "quantity": 1,
      "unit": "ks",
      "priceAmount": 2.50,
      "priceAdjustCoef": 0.8,
      "txAdjustCoef": 1,
      "priceCurrency": "EUR",
      "accountableItemId": "cbceb73a-25e7-4b83-a15d-c3cef5ef5f32",
      "createTime": "11.10.2022 14:50:17"
    }
  }
}
```

Transaction discount response:

```json
{
  "success": true,
  "data": {
    "adjustedAccountingTransaction": {
      "id": "d18926c3-c4f1-4e1e-94a6-e47c2e5afa0d",
      "type": "INVOICE",
      "area": {
        "id": 3,
        "numberOfSeats": 0
      },
      "createTime": "12.10.2022 11:03:30",
      "priceGross": 0,
      "accountingTransactionState": "OPENED",
      "transactionPriceAdjustmentCoefficient": 0.8
    }
  }
}
```
