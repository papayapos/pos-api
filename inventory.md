# POST /api/v1/inventory

CRUD operations for inventories (storehouses). Each inventory independently tracks stock amounts for items. Amounts are calculated from [cards](card.md), starting at zero.

**Auth:** `Authorization: Bearer <token>`

Sections: [GET](#get) | [UPDATE](#update) | [DELETE](#delete)

---

## GET

Fetch inventories. Optionally include current item amounts.

- Sending empty `data` returns all inventories without item amounts.
- To get item amounts, set `withAmounts: true` and specify which inventories to query in the `inventories` array.
- You can filter items within each inventory using `itemIds`.
- Non-existent inventory or item IDs are silently ignored.

> To update amounts or average price of items, use the [card](card.md) endpoint. To manage item data, use the [item](item.md) endpoint.

### Request fields

| Field | Type | Required | Description |
|---|---|:---:|---|
| `inventories` | object[] | no | List of inventory queries. See fields below. |
| `withAmounts` | boolean | no | If `true`, includes items and their current amounts in the response. Requires `inventories` to be set. |

**Inventory query object:**

| Field | Type | Required | Description |
|---|---|:---:|---|
| `inventoryId` | number | yes | ID of the inventory to fetch |
| `itemIds` | `string[]` (UUID) | no | Filter returned items to these IDs only |

### Request examples

All inventories (no amounts):

```json
{
  "action": "GET",
  "data": {}
}
```

Specific inventory with all item amounts:

```json
{
  "action": "GET",
  "data": {
    "inventories": [
      {
        "inventoryId": 1
      }
    ],
    "withAmounts": true
  }
}
```

Specific inventory with filtered items:

```json
{
  "action": "GET",
  "data": {
    "inventories": [
      {
        "inventoryId": 1,
        "itemIds": [
          "08850701-060c-4381-9805-185f8f846138",
          "a0aa6b7d-9513-416b-938b-3e4bfb5a47d5"
        ]
      }
    ],
    "withAmounts": true
  }
}
```

### Response

`data.inventories` — array of inventory objects. See [storehouse objects](storehouse%20objects.md) for full field definitions.

| Field | Type | Description |
|---|---|---|
| `inventories` | object[] | Inventories matching the request |

### Response example

```json
{
  "success": true,
  "data": {
    "inventories": [
      {
        "id": 1,
        "name": "Main inventory",
        "description": "Description of main inventory",
        "groups": [
          {
            "id": 1,
            "name": "Fruit",
            "items": [
              {
                "id": "27fd9916-d6ee-43d7-a3a4-0946f41b3c0f",
                "title": "Apple",
                "amount": 42.5,
                "measuringUnit": "kg"
              }
            ]
          }
        ]
      }
    ]
  }
}
```

---

## UPDATE

Create or update inventories.

- Request **without** `id` → creates a new inventory with a generated ID.
- Request **with** `id`:
  - If the inventory exists → updates it.
  - If it does not exist → creates it using the provided ID.
- Updating a deleted inventory restores it along with its cards.
- The `groups` field is ignored in UPDATE requests.

### Request fields

| Field | Type | Required | Description |
|---|---|:---:|---|
| `inventories` | object[] | yes | Inventories to create or update |

**Inventory object:**

| Field | Type | Required | Description |
|---|---|:---:|---|
| `id` | number | no | ID of the inventory. Omit to auto-generate. |
| `name` | string | no | Name of the inventory |
| `description` | string | no | Description of the inventory |

### Request example

```json
{
  "action": "UPDATE",
  "data": {
    "inventories": [
      {
        "id": 1,
        "name": "Main inventory",
        "description": "Primary storehouse for all ingredients"
      }
    ]
  }
}
```

### Response

Returns the created or updated inventory objects in `data.inventories`.

### Response example

```json
{
  "success": true,
  "data": {
    "inventories": [
      {
        "id": 1,
        "name": "Main inventory",
        "description": "Primary storehouse for all ingredients"
      }
    ]
  }
}
```

---

## DELETE

Delete inventories by their IDs.

- Non-existent IDs are silently ignored.
- Deleted inventories can be restored by sending an UPDATE request with the same ID.
- Inventory with `id: 1` (the default inventory) cannot be deleted.

### Request fields

| Field | Type | Required | Description |
|---|---|:---:|---|
| `ids` | `number[]` | yes | IDs of inventories to delete |

### Request example

```json
{
  "action": "DELETE",
  "data": {
    "ids": [2, 3, 4]
  }
}
```

### Response

Returns the deleted inventory objects in `data.inventories`. The `groups` field is not included in the delete response.

### Response example

```json
{
  "success": true,
  "data": {
    "inventories": [
      {
        "id": 2,
        "name": "Fruit",
        "description": "Fruit and other related vegetables"
      },
      {
        "id": 3,
        "name": "Water"
      }
    ]
  }
}
```

### Errors

| Code | Message | Cause |
|---|---|---|
| `4000` | Default Inventory cannot be deleted | Attempted to delete inventory with `id: 1` |
