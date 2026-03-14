# POST /api/v1/transaction/accountingEntry/date

Fetch accounting entries (sold items) within a date range. Useful for sales reporting and export.

**Auth:** `Authorization: Bearer <token>`

Sections: [GET](#get)

---

## GET

Fetch all accounting entries created between two timestamps.

### Request fields

| Field | Type | Required | Description |
|---|---|:---:|---|
| `fromDate` | number | yes | Start of range as a UNIX timestamp in milliseconds |
| `toDate` | number | yes | End of range as a UNIX timestamp in milliseconds |

### Request example

```json
{
  "action": "GET",
  "data": {
    "fromDate": 1713873874129,
    "toDate": 1713873934000
  }
}
```

### Response

`data` — array of entry report objects (not wrapped in a named key):

| Field | Type | Description |
|---|---|---|
| `id` | string (UUID) | Entry ID |
| `txUuid` | string (UUID) | ID of the parent transaction |
| `menuId` | string (UUID) | ID of the menu/catalog item |
| `menuTitle` | string | Name of the menu category |
| `title` | string | Name of the item as shown on the receipt |
| `plu` | string | PLU code of the item |
| `count` | number | Number of items sold |
| `basePrice` | number | Base unit price |
| `unitPrice` | number | Price per unit (after any modifiers) |
| `priceAdjustCoef` | number | Price adjustment coefficient (1 = no change) |
| `totalPrice` | number | Total price excluding VAT |
| `totalWithVat` | number | Total price including VAT and adjustments |
| `vatRate` | number | VAT rate in % |
| `type` | string | Entry type: `SALE`, `REFUND`, or `VOUCHER` |
| `createDate` | number | Creation time as UNIX timestamp in milliseconds |
| `user` | string | Name of the user who created the entry |
| `salePlaceName` | string | Name of the sale place |
| `revenueCenterName` | string | Name of the revenue center |
| `note` | string | Entry note |

### Code examples

HTTPie:
```bash
http POST $BASE_URL/api/v1/transaction/accountingEntry/date \
  "Authorization:Bearer $TOKEN" \
  action=GET \
  data:='{"fromDate":1713873874129,"toDate":1713873934000}'
```

Kotlin:
```kotlin
val body = """
    {
        "action": "GET",
        "data": {
            "fromDate": 1713873874129,
            "toDate": 1713873934000
        }
    }
""".trimIndent().toRequestBody("application/json".toMediaType())

OkHttpClient().newCall(
    Request.Builder()
        .url("$BASE_URL/api/v1/transaction/accountingEntry/date")
        .addHeader("Authorization", "Bearer $TOKEN")
        .post(body)
        .build()
).execute().body?.string()
```

### Response example

```json
{
  "success": true,
  "data": [
    {
      "id": "f9c7ecca-552a-4f27-8fec-d78beaa34285",
      "txUuid": "82046ac6-69ef-46f1-8616-8bf832c8a146",
      "menuId": "00000000-0000-0000-0000-000000000007",
      "menuTitle": "Beer",
      "title": "Zlatý bažant radler - lemon",
      "plu": "0719",
      "count": 1,
      "basePrice": 1.40,
      "unitPrice": 1.40,
      "priceAdjustCoef": 1,
      "totalPrice": 1.1667,
      "totalWithVat": 1.40,
      "vatRate": 20,
      "type": "SALE",
      "createDate": 1713873874129,
      "user": "Administrator",
      "salePlaceName": "Main Register"
    }
  ]
}
```
