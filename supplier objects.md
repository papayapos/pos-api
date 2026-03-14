# Supplier Object Reference

Supplier objects are used in both the [supplier](supplier.md) endpoint and embedded in [stock change cards](card.md).

> Suppliers are informational only — they are not used in any calculations, only displayed in reports and on cards.

---

## Supplier

| Field | Type | Description |
|---|---|---|
| `id` | number | Unique supplier ID |
| `name` | string | Supplier name |
| `address` | string | Street address |
| `city` | string | City |
| `country` | string | Country |
| `contactPerson` | string | Name of the contact person |
| `phone` | string | Phone number |
| `email` | string | Email address |
| `ico` | string | Company registration number (IČO) |
| `dic` | string | Tax ID (DIČ) |
| `icDph` | string | VAT registration number (IČ DPH) |
| `registration` | boolean | Whether the supplier is registered |
| `registrationNumber` | string | Registration number |
| `registrationDate` | string | Registration date |
