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
