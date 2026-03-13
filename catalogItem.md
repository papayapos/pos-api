# POST /api/v1/catalog/item

Read catalog items (pricelist). Items are organized into categories.

**Auth:** `Authorization: Bearer <token>`

Sections: [GET](#get)

---

## GET

Fetch catalog items, optionally filtered by item UUIDs or category UUIDs.

- Sending empty `data` returns all categories with all their items.
- `item` / `items` filter by item UUID(s).
- `category` / `categories` filter by category UUID(s).
- Filters are combined as a union — matching items from any filter are returned.

### Request fields

| Field | Type | Required | Description |
|---|---|:---:|---|
| `item` | string (UUID) | no | Single item UUID to fetch |
| `items` | `string[]` (UUID) | no | Multiple item UUIDs to fetch |
| `category` | string (UUID) | no | Fetch all items in this category |
| `categories` | `string[]` (UUID) | no | Fetch all items in these categories |

### Request examples

All catalog items:

```json
{
  "action": "GET",
  "data": {}
}
```

Items from specific categories:

```json
{
  "action": "GET",
  "data": {
    "categories": [
      "5de57c08-6b12-4de5-8d6f-edccaf82cd2d",
      "e33a3bf6-d6c2-43c9-a4e5-ad1fae30fd2f"
    ]
  }
}
```

Specific items by UUID:

```json
{
  "action": "GET",
  "data": {
    "items": [
      "360ded1b-8dcb-410e-9f0e-0aa80bc1720f",
      "7e8a6ea0-2832-4c15-b686-2b9a80a7b573"
    ]
  }
}
```

### Response

`data.categories` — array of category objects, each containing their items:

**Category fields:**

| Field | Type | Description |
|---|---|---|
| `id` | string (UUID) | UUID of the category |
| `title` | string | Name of the category |
| `position` | number | Display order position |
| `color` | string | Color identifier |
| `items` | object[] | Items belonging to this category |

**Item fields:**

| Field | Type | Description |
|---|---|---|
| `id` | string (UUID) | UUID of the item |
| `title` | string | Name of the item |
| `price` | number | Price of the item |
| `vatRate` | number | VAT rate in % |
| `color` | string | Color identifier |
| `isFavorite` | boolean | Whether the item is marked as favorite |
| `isHidden` | boolean | Whether the item is hidden from the menu |
| `isOutOfStock` | boolean | Whether the item is marked as out of stock |
| `position` | number | Display order position within the category |

### Response example

```json
{
  "success": true,
  "data": {
    "categories": [
      {
        "id": "5de57c08-6b12-4de5-8d6f-edccaf82cd2d",
        "title": "Drinks",
        "position": 0,
        "color": "RED_2",
        "items": [
          {
            "id": "360ded1b-8dcb-410e-9f0e-0aa80bc1720f",
            "title": "Lemonade",
            "price": 123,
            "vatRate": 20,
            "color": "VIOLET_1",
            "isFavorite": false,
            "isHidden": false,
            "isOutOfStock": false,
            "position": 0
          },
          {
            "id": "7e8a6ea0-2832-4c15-b686-2b9a80a7b573",
            "title": "Orange Juice",
            "price": 12,
            "vatRate": 20,
            "color": "ORANGE_1",
            "isFavorite": false,
            "isHidden": false,
            "isOutOfStock": false,
            "position": 1
          }
        ]
      }
    ]
  }
}
```
