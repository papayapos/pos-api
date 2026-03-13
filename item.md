# POST /api/v1/inventory/item

CRUD operations for stock items. Items are organized into groups.

Items can be used to directly track product quantities, or as components in recipes assigned to menu items. For tracking current amounts per inventory, use the [inventory](inventory.md) endpoint. To add or subtract stock, use the [card](card.md) endpoint.

**Auth:** `Authorization: Bearer <token>`

Sections: [GET](#get) | [UPDATE](#update) | [DELETE](#delete)

---

## GET

Fetch stock items, optionally filtered by item IDs, product codes, or group IDs.

- Sending empty `data` returns all non-deleted items.
- Multiple filter fields are combined as a union — items matching any filter are returned once.

### Request fields

| Field | Type | Required | Description |
|---|---|:---:|---|
| `itemIds` | `string[]` (UUID) | no | Return items matching these IDs |
| `productCodes` | `string[]` | no | Return items matching these product codes |
| `groupIds` | `number[]` | no | Return all items in these groups |

### Request examples

All items:

```json
{
  "action": "GET",
  "data": {}
}
```

Filter by item IDs:

```json
{
  "action": "GET",
  "data": {
    "itemIds": ["27fd9916-d6ee-43d7-a3a4-0946f41b3c0f"]
  }
}
```

Filter by product codes:

```json
{
  "action": "GET",
  "data": {
    "productCodes": ["123", "5555"]
  }
}
```

Filter by group IDs:

```json
{
  "action": "GET",
  "data": {
    "groupIds": [1, 2]
  }
}
```

Combined filter (union of all matches):

```json
{
  "action": "GET",
  "data": {
    "itemIds": [
      "27fd9916-d6ee-43d7-a3a4-0946f41b3c0f",
      "27fd9916-aaaa-43d7-a3a4-0946f41b3c0f"
    ],
    "productCodes": ["123", "5555"],
    "groupIds": [1, 2]
  }
}
```

### Response

`data.groups` — array of stock item group objects, each containing their items. See [storehouse objects](storehouse%20objects.md) for full field definitions.

> `amount` is not included in this response. Use [/api/v1/inventory](inventory.md) to get current stock amounts.

### Response example

```json
{
  "success": true,
  "data": {
    "groups": [
      {
        "id": 1,
        "name": "Fruit",
        "type": "OTHER",
        "items": [
          {
            "id": "27fd9916-d6ee-43d7-a3a4-0946f41b3c0f",
            "externalId": "ext-123",
            "title": "Apple",
            "productCode": "12345",
            "measuringUnit": "kg",
            "secondaryMeasuringUnit": "g",
            "unitCoefficient": 1000,
            "minimalStockAmount": 3,
            "priceFixed": 5,
            "priceNetAverage": 2.5,
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

Create or update items and groups. Both can be done simultaneously in one request.

- Group/item **without** `id` → created with a generated ID.
- Group/item **with** `id`:
  - Does not exist → created using the provided ID.
  - Already exists → updated.
- You can mix items with and without IDs in one request.
- To move an item to a different group, send an update with the item nested under the desired group.
- An item always belongs to exactly one group.

> `amount` and `priceNetAverage` cannot be set here — they are calculated from stock cards. See [card](card.md).
>
> When updating an existing item, all its fields should be provided.
>
> The Alcohol group name cannot be changed.

### Request fields

| Field | Type | Required | Description |
|---|---|:---:|---|
| `groups` | object[] | yes | Stock item groups containing items to create or update |

**Group object:**

| Field | Type | Required | Description |
|---|---|:---:|---|
| `id` | number | no | Group ID. Omit to auto-generate. |
| `name` | string | no | Name of the group |
| `type` | string | no | Group type: `ALCOHOL` or `OTHER` |
| `items` | object[] | no | Items belonging to this group |

**Item object:**

| Field | Type | Required | Description |
|---|---|:---:|---|
| `id` | string (UUID) | no | Item ID. Omit to auto-generate. |
| `externalId` | string | no | External system identifier |
| `title` | string | no | Name of the item |
| `productCode` | string | no | Product code |
| `measuringUnit` | string | no | Primary measuring unit (e.g. `kg`, `l`, `piece`) |
| `secondaryMeasuringUnit` | string | no | Secondary measuring unit |
| `unitCoefficient` | number | no | Conversion factor between primary and secondary unit (e.g. 1000 for kg→g) |
| `minimalStockAmount` | number | no | Minimum stock threshold, used in reports |
| `priceFixed` | number | no | Fixed price set by user |
| `vatRate` | number | no | VAT rate in % |

### Request example

```json
{
  "action": "UPDATE",
  "data": {
    "groups": [
      {
        "id": 1,
        "name": "Fruit",
        "type": "OTHER",
        "items": [
          {
            "id": "27fd9916-d6ee-43d7-a3a4-0946f41b3c0f",
            "externalId": "ext-123",
            "title": "Apple",
            "priceFixed": 5,
            "vatRate": 20,
            "measuringUnit": "kg",
            "secondaryMeasuringUnit": "g",
            "unitCoefficient": 1000,
            "productCode": "12345",
            "minimalStockAmount": 3
          }
        ]
      }
    ]
  }
}
```

### Response

Returns the created or updated groups with their items in `data.groups`.

> `amount` is not included. Use [/api/v1/inventory](inventory.md) for stock amounts.

### Response example

```json
{
  "success": true,
  "data": {
    "groups": [
      {
        "id": 1,
        "name": "Fruit",
        "type": "OTHER",
        "items": [
          {
            "id": "27fd9916-d6ee-43d7-a3a4-0946f41b3c0f",
            "externalId": "ext-123",
            "title": "Apple",
            "productCode": "12345",
            "measuringUnit": "kg",
            "secondaryMeasuringUnit": "g",
            "unitCoefficient": 1000,
            "minimalStockAmount": 3,
            "priceFixed": 5,
            "priceNetAverage": 0,
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

Delete individual items or entire groups (including all their items).

- `itemIds` → deletes matching items.
- `groupIds` → deletes matching groups and all their items.
- Both can be combined in one request.

### Request fields

| Field | Type | Required | Description |
|---|---|:---:|---|
| `itemIds` | `string[]` (UUID) | no | IDs of items to delete |
| `groupIds` | `number[]` | no | IDs of groups to delete (along with all their items) |

At least one of `itemIds` or `groupIds` must be provided.

### Request examples

Delete specific items:

```json
{
  "action": "DELETE",
  "data": {
    "itemIds": [
      "27fd9916-d6ee-43d7-a3a4-0946f41b3c0f",
      "27fd9916-aaaa-43d7-a3a4-0946f41b3c0f"
    ]
  }
}
```

Delete a group and all its items:

```json
{
  "action": "DELETE",
  "data": {
    "groupIds": [1, 2]
  }
}
```

Delete both items and groups:

```json
{
  "action": "DELETE",
  "data": {
    "itemIds": ["27fd9916-d6ee-43d7-a3a4-0946f41b3c0f"],
    "groupIds": [2]
  }
}
```

### Response

Returns deleted groups with their items in `data.groups`. Deleted items are always returned inside their group, even if the group itself was not deleted.

If `success` is `false`, no items were deleted.

> `amount` is not included. Use [/api/v1/inventory](inventory.md) for stock amounts.

### Response example

```json
{
  "success": true,
  "data": {
    "groups": [
      {
        "id": 1,
        "name": "Fruit",
        "items": [
          {
            "id": "27fd9916-d6ee-43d7-a3a4-0946f41b3c0f",
            "title": "Apple",
            "productCode": "12345",
            "measuringUnit": "kg",
            "secondaryMeasuringUnit": "g",
            "unitCoefficient": 1000,
            "minimalStockAmount": 3,
            "priceFixed": 5,
            "priceNetAverage": 0,
            "vatRate": 20
          }
        ]
      }
    ]
  }
}
```
