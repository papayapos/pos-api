# Storehouse Object Reference

Shared object definitions used across inventory endpoints.

---

## Stock Item Group

| Field | Type | Description |
|---|---|---|
| `id` | number | Group ID |
| `name` | string | Group name |
| `type` | string | Group type: `ALCOHOL` or `OTHER` |
| `items` | Stock Item[] | Items belonging to this group |

---

## Stock Item

| Field | Type | Description |
|---|---|---|
| `id` | string (UUID) | Item ID |
| `externalId` | string | External system identifier |
| `title` | string | Item name |
| `productCode` | string | Product code |
| `measuringUnit` | string | Primary measuring unit (e.g. `kg`, `l`, `piece`) |
| `secondaryMeasuringUnit` | string | Secondary measuring unit |
| `unitCoefficient` | number | Conversion factor from primary to secondary unit (e.g. `1000` for kg → g) |
| `minimalStockAmount` | number | Minimum stock threshold, used in reports |
| `priceFixed` | number | Fixed price set by user |
| `priceNetAverage` | number | Average net purchase price, calculated from stock change cards. Read-only. |
| `amount` | number | Current stock amount. Only present in [inventory](inventory.md) GET responses with `withAmounts: true`. |
| `vatRate` | number | VAT rate in % |

---

## Inventory

| Field | Type | Description |
|---|---|---|
| `id` | number | Inventory ID |
| `name` | string | Inventory name |
| `description` | string | Inventory description |
| `groups` | Stock Item Group[] | Current state of the inventory. Only populated in GET responses with `withAmounts: true`. Ignored in UPDATE and DELETE requests. |

---

## Stock Change Card

| Field | Type | Description |
|---|---|---|
| `id` | number | Card ID |
| `inventoryId` | number | ID of the inventory this card belongs to |
| `createTime` | string | Creation time. Format: `dd.MM.yyyy HH:mm:ss` |
| `placedTime` | string | Placement time. Format: `dd.MM.yyyy HH:mm:ss` |
| `type` | string | Card type: `RECEIPT_CARD` (goods received) or `ISSUE_CARD` (goods issued) |
| `accType` | number | Accounting type: `1` = operating costs, `2` = sale, `3` = refund of operating costs, `4` = refund of sale, `5` = stock taking receipt, `6` = stock taking invoice |
| `documentType` | number | Document type: `1` = fiscal receipt, `2` = invoice, `3` = other |
| `documentId` | string | ID of an associated document (e.g. accounting transaction ID) |
| `supplier` | Supplier | Supplier associated with this card |
| `orderNumber` | string | Order number |
| `deliveryNote` | string | Delivery note reference |
| `note` | string | Free-text note |
| `items` | Stock Change Card Item[] | Items affected by this card |

---

## Stock Change Card Item

| Field | Type | Description |
|---|---|---|
| `id` | number | Card item ID |
| `stockItemId` | string (UUID) | ID of the referenced stock item |
| `stockItemTitle` | string | Name of the stock item (for display) |
| `measuringUnit` | string | Unit of measurement |
| `amount` | number | Amount added (RECEIPT_CARD) or removed (ISSUE_CARD) |
| `priceNet` | number | Net price per unit |
| `vatRate` | number | VAT rate in % |
| `stockChangeCardId` | number | ID of the card this item belongs to |
| `entryId` | string (UUID) | ID of the associated accounting entry (if any) |

---

## Supplier

| Field | Type | Description |
|---|---|---|
| `id` | number | Supplier ID |
| `name` | string | Supplier name |
| `address` | string | Street address |
| `city` | string | City |
| `country` | string | Country |
| `contactPerson` | string | Name of contact person |
| `phone` | string | Phone number |
| `email` | string | Email address |
| `ico` | string | Company registration number (IČO) |
| `dic` | string | Tax ID (DIČ) |
| `icDph` | string | VAT registration number (IČ DPH) |
| `registration` | boolean | Whether the supplier is registered |
| `registrationNumber` | string | Registration number |
| `registrationDate` | string | Registration date |

---

## Recipe

| Field | Type | Description |
|---|---|---|
| `accountableItemId` | string (UUID) | UUID of the catalog item this recipe is assigned to |
| `items` | Recipe Item[] | Ingredients used in this recipe |

## Recipe Item

| Field | Type | Description |
|---|---|---|
| `stockItem` | Stock Item | The stock item used as an ingredient |
| `amountUsed` | number | Amount of this ingredient consumed per serving |
| `unit` | number | Unit used: `1` = primary unit, `2` = secondary unit |
