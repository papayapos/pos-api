# Papaya POS API

## Setup

Replace `BASE_URL` and `TOKEN` in all examples with your actual values.

### HTTPie

Install: https://httpie.io/docs/cli/installation

```bash
export BASE_URL=https://your.papayapos.domain
export TOKEN=your_access_token
```

### Kotlin (OkHttp)

Add to `build.gradle.kts`:

```kotlin
implementation("com.squareup.okhttp3:okhttp:4.12.0")
```

Imports used in all examples:

```kotlin
import okhttp3.MediaType.Companion.toMediaType
import okhttp3.OkHttpClient
import okhttp3.Request
import okhttp3.RequestBody.Companion.toRequestBody
```

---

## Endpoint Summary

| Endpoint | Description |
|---|---|
| [POST /api/v1/areas](areas.md) | CRUD operations for areas (tables) |
| [POST /api/v1/areas/rooms](rooms.md) | CRUD operations for rooms (room, sector, etc.) |
| [POST /api/v1/salePlace](salePlace.md) | Read sale places (cashiers) |
| [POST /api/v1/catalog/item](catalogItem.md) | Read catalog items (pricelist) |
| [POST /api/v1/inventory](inventory.md) | CRUD operations for inventories |
| [POST /api/v1/inventory/card](card.md) | Receive or issue stock with inventory cards |
| [POST /api/v1/inventory/item](item.md) | CRUD operations for stock items |
| [POST /api/v1/supplier](supplier.md) | CRUD operations for suppliers |
| [POST /api/v1/memberCard](memberCard.md) | CRUD operations for member/loyalty cards |
| [POST /api/v1/transaction/accountingTransaction](accountingTransaction.md) | Open and cancel orders/bills |
| [POST /api/v1/transaction/accountingEntry](accountingEntry.md) | Add or cancel items on a bill |
| [POST /api/v1/transaction/accountingEntry/date](accountingEntryByDate.md) | Read entries by date range |
| [POST /api/v1/transaction/payment](payment.md) | Pay for a transaction |
| [POST /api/v1/transaction/discount](discount.md) | Apply a discount to a transaction or entry |

---

## Sending Requests

**All requests use HTTP POST.**

### Headers

| Header | Value |
|---|---|
| `Content-Type` | `application/json` |
| `Accept` | `application/json` |
| `Authorization` | `Bearer <token>` |

The token is generated in the Papaya POS administration account.

### Request Body

Every request body must contain `action` and `data`:

```json
{
  "action": "GET",
  "data": {}
}
```

`data` must always be present. If you have no filter parameters, send an empty object `{}`. Requests with a missing `data` field will return an error.

### Actions

| Action | Description |
|---|---|
| `GET` | Fetch data. Use `data` fields to filter results. |
| `UPDATE` | Create or update using data provided in `data`. |
| `DELETE` | Delete objects identified by fields in `data`. |

---

## Response Format

All responses follow this structure:

```json
{
  "success": true,
  "data": { ... }
}
```

On failure:

```json
{
  "success": false,
  "error": "2: Field value required: itemIds"
}
```

| Field | Type | Description |
|---|---|---|
| `success` | boolean | `true` if the request was processed successfully |
| `data` | object | Present only when `success` is `true`. Contains the response payload. |
| `error` | string | Present only when `success` is `false`. Contains an error code and message. |

---

## Error Codes

### General Errors

| Code | Message | Cause |
|---|---|---|
| `1` | Empty request not allowed | Endpoint received empty request |
| `2` | Field value required: `<field_name>` | A required field is missing from `data` |
| `3` | This endpoint or method is not supported | Endpoint does not exist |
| `4` | Action in request is not supported | `action` value is not valid |
| `5` | Method is not supported, use POST requests with action defined in body | Request used GET or DELETE HTTP method instead of POST |
| `6` | Bad request, check documentation for the correct form of requests | Request is malformed or missing required structure |

### Storehouse Errors (4000–4999)

| Code | Message | Cause |
|---|---|---|
| `4000` | Default storehouse cannot be deleted | Attempted to delete the default inventory |
