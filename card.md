# POST /api/v1/inventory/card

CRUD operations for stock change cards. Cards record the movement of items in and out of inventories, including supplier details, document type, prices, and more. Each card contains one or more card items describing the actual stock changes.

**Auth:** `Authorization: Bearer <token>`

Sections: [GET](#get) | [UPDATE](#update) | [DELETE](#delete)

---

## GET

Fetch stock change cards by IDs or by date range.

- Option 1: provide `ids` to get specific cards.
- Option 2: provide `from` (and optionally `to`) for a date range, with optional filters.
- `ids` and `from`/`to` cannot be combined. If both are present, `ids` takes priority.
- There is no "get all" option — the number of cards grows unboundedly over time.
- Non-existent IDs are silently ignored.

### Request fields

**Option 1 — by IDs:**

| Field | Type | Required | Description |
|---|---|:---:|---|
| `ids` | `number[]` | yes | IDs of cards to fetch |

**Option 2 — by date range:**

| Field | Type | Required | Description |
|---|---|:---:|---|
| `from` | string | yes | Start of date range. Format: `dd.MM.yyyy HH:mm:ss` |
| `to` | string | no | End of date range. Format: `dd.MM.yyyy HH:mm:ss` |
| `inventoryId` | number | no | Filter to cards from this inventory only |
| `documentType` | number | no | Filter by document type: `1` = fiscal receipt, `2` = invoice, `3` = other |
| `supplierName` | string | no | Filter by supplier name |

### Request examples

By IDs:

```json
{
  "action": "GET",
  "data": {
    "ids": [1, 2, 3, 4]
  }
}
```

By date range:

```json
{
  "action": "GET",
  "data": {
    "from": "11.03.2020 01:01:03",
    "to": "11.03.2022 01:01:03"
  }
}
```

By date range with filters:

```json
{
  "action": "GET",
  "data": {
    "from": "11.03.2020 01:01:03",
    "to": "11.03.2022 01:01:03",
    "inventoryId": 1,
    "documentType": 1,
    "supplierName": "Silvia"
  }
}
```

### Response

`data.cards` — array of stock change card objects. See [storehouse objects](storehouse%20objects.md) for full field definitions.

### Code examples

HTTPie — get by IDs:
```bash
http POST $BASE_URL/api/v1/inventory/card \
  "Authorization:Bearer $TOKEN" \
  action=GET \
  data:='{"ids":[1,2,3]}'
```

HTTPie — get by date range:
```bash
http POST $BASE_URL/api/v1/inventory/card \
  "Authorization:Bearer $TOKEN" \
  action=GET \
  data:='{"from":"01.01.2024 00:00:00","to":"31.01.2024 23:59:59"}'
```

Kotlin:
```kotlin
// Get by IDs
println(papayaPost("/api/v1/inventory/card", "GET", """{"ids":[1,2,3]}"""))

// Get by date range
println(papayaPost("/api/v1/inventory/card", "GET",
    """{"from":"01.01.2024 00:00:00","to":"31.01.2024 23:59:59"}"""))
```

### Response example

```json
{
  "success": true,
  "data": {
    "cards": [
      {
        "id": 1,
        "inventoryId": 1,
        "createTime": "11.03.2021 11:04:30",
        "placedTime": "11.03.2021 11:04:29",
        "type": "RECEIPT_CARD",
        "documentType": 1,
        "supplier": {
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
        },
        "items": [
          {
            "id": 1,
            "stockItemId": "27fd9916-d6ee-43d7-a3a4-0946f41b3c0f",
            "stockItemTitle": "Apple",
            "measuringUnit": "piece",
            "amount": 10,
            "priceNet": 2,
            "vatRate": 20
          }
        ]
      }
    ]
  }
}
```

---

## UPDATE

Create or update stock change cards.

**Card creation/update rules:**
- Card **without** `id` → created with a generated ID.
- Card **with** `id`:
  - ID does not exist → created using the provided ID.
  - ID exists → updated.

**Supplier rules:**
- Supplier **with** `id` → loads stored supplier data. Any additional fields provided override only the stored data for this card (the stored supplier record is not changed).
- Supplier **without** `id` → provided data is stored directly in the card only. This does not create a new supplier record.

**Card item rules:**
- When creating a new card, item `id`s are ignored — new IDs are always generated.
- When updating an existing card:
  - Item `id` exists and belongs to this card → item is overwritten.
  - Item `id` exists but belongs to a different card → a new ID is generated (items cannot be moved between cards).
- All previous card items are replaced on update. You can safely omit item `id`s and let the API manage them.

> If you need to store `id`s of created card items, capture them from the response — they are not predictable.

### Request fields

| Field | Type | Required | Description |
|---|---|:---:|---|
| `cards` | object[] | yes | Stock change cards to create or update |

**Card object:**

| Field | Type | Required | Description |
|---|---|:---:|---|
| `id` | number | no | Card ID. Omit to auto-generate. |
| `inventoryId` | number | yes | ID of the inventory this card belongs to |
| `createTime` | string | yes | Creation time. Format: `dd.MM.yyyy HH:mm:ss` |
| `placedTime` | string | yes | Placement time (use same value as `createTime` for now). Format: `dd.MM.yyyy HH:mm:ss` |
| `type` | string | yes | Card type: `RECEIPT_CARD` (goods added) or `ISSUE_CARD` (goods removed) |
| `documentType` | number | no | `1` = fiscal receipt, `2` = invoice, `3` = other |
| `documentId` | string | no | ID of an associated document (e.g. accounting transaction ID) |
| `supplier` | object | no | Supplier information. See [supplier objects](supplier%20objects.md). |
| `orderNumber` | string | no | Order number |
| `deliveryNote` | string | no | Delivery note reference |
| `note` | string | no | Free-text note |
| `items` | object[] | yes | Stock items being received or issued |

**Card item object:**

| Field | Type | Required | Description |
|---|---|:---:|---|
| `id` | number | no | Item ID. Ignored when creating a card; managed by API during updates. |
| `stockItemId` | string (UUID) | yes | ID of the existing stock item (from [/inventory/item](item.md)) |
| `stockItemTitle` | string | no | Name of the item (for display purposes) |
| `amount` | number | yes | Amount added or removed |
| `measuringUnit` | string | yes | Unit of measurement |
| `priceNet` | number | no | Net price per unit |
| `vatRate` | number | no | VAT rate in % |

### Request example

```json
{
  "action": "UPDATE",
  "data": {
    "cards": [
      {
        "id": 1,
        "inventoryId": 1,
        "createTime": "17.02.2022 14:04:03",
        "placedTime": "17.02.2022 14:04:03",
        "type": "RECEIPT_CARD",
        "documentType": 1,
        "documentId": "1234",
        "supplier": {
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
        },
        "orderNumber": "12",
        "deliveryNote": "DN-0042",
        "note": "Received in good condition",
        "items": [
          {
            "stockItemId": "27fd9916-d6ee-43d7-a3a4-0946f41b3c0f",
            "stockItemTitle": "Apple",
            "amount": 100,
            "measuringUnit": "ks",
            "priceNet": 0.5,
            "vatRate": 20
          }
        ]
      }
    ]
  }
}
```

### Code examples

HTTPie:
```bash
http POST $BASE_URL/api/v1/inventory/card \
  "Authorization:Bearer $TOKEN" \
  action=UPDATE \
  data:='{"cards":[{"inventoryId":1,"createTime":"17.02.2022 14:04:03","placedTime":"17.02.2022 14:04:03","type":"RECEIPT_CARD","items":[{"stockItemId":"27fd9916-d6ee-43d7-a3a4-0946f41b3c0f","amount":100,"measuringUnit":"ks","priceNet":0.5,"vatRate":20}]}]}'
```

Kotlin:
```kotlin
println(papayaPost("/api/v1/inventory/card", "UPDATE", """
    {"cards":[{
        "inventoryId":1,
        "createTime":"17.02.2022 14:04:03",
        "placedTime":"17.02.2022 14:04:03",
        "type":"RECEIPT_CARD",
        "items":[{
            "stockItemId":"27fd9916-d6ee-43d7-a3a4-0946f41b3c0f",
            "amount":100,
            "measuringUnit":"ks",
            "priceNet":0.5,
            "vatRate":20
        }]
    }]}""".trimIndent()))
```

### Response

Returns the created or updated cards with their items in `data.cards`. Capture item IDs from this response if you need them for future updates.

### Response example

```json
{
  "success": true,
  "data": {
    "cards": [
      {
        "id": 1,
        "inventoryId": 1,
        "createTime": "17.02.2022 14:04:03",
        "placedTime": "17.02.2022 14:04:03",
        "type": "RECEIPT_CARD",
        "documentType": 1,
        "documentId": "1234",
        "supplier": {
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
        },
        "orderNumber": "12",
        "deliveryNote": "DN-0042",
        "note": "Received in good condition",
        "items": [
          {
            "id": 1,
            "stockItemId": "27fd9916-d6ee-43d7-a3a4-0946f41b3c0f",
            "stockItemTitle": "Apple",
            "amount": 100,
            "measuringUnit": "ks",
            "priceNet": 0.5,
            "vatRate": 20
          }
        ]
      }
    ]
  }
}
```

---

## DELETE

Delete stock change cards by their IDs. Card items are deleted along with their card. Deleting a card does not affect supplier records stored via the [supplier](supplier.md) endpoint.

### Request fields

| Field | Type | Required | Description |
|---|---|:---:|---|
| `ids` | `number[]` | yes | IDs of cards to delete |

### Request example

```json
{
  "action": "DELETE",
  "data": {
    "ids": [1, 2, 3]
  }
}
```

### Code examples

HTTPie:
```bash
http POST $BASE_URL/api/v1/inventory/card \
  "Authorization:Bearer $TOKEN" \
  action=DELETE \
  data:='{"ids":[1,2,3]}'
```

Kotlin:
```kotlin
println(papayaPost("/api/v1/inventory/card", "DELETE", """{"ids":[1,2,3]}"""))
```

### Response

Returns the deleted cards in `data.cards`. If `success` is `false`, no cards were deleted.

### Response example

```json
{
  "success": true,
  "data": {
    "cards": [
      {
        "id": 1,
        "inventoryId": 1,
        "createTime": "17.02.2022 14:04:03",
        "type": "RECEIPT_CARD",
        "items": [
          {
            "id": 1,
            "stockItemId": "27fd9916-d6ee-43d7-a3a4-0946f41b3c0f",
            "stockItemTitle": "Apple",
            "amount": 100,
            "measuringUnit": "ks",
            "priceNet": 0.5,
            "vatRate": 20
          }
        ]
      }
    ]
  }
}
```
