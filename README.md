**Endpoint Summary**




| Endpoint                                                               | Description                                     |
|------------------------------------------------------------------------|-------------------------------------------------|
| [/api/v1/areas](areas.md#)                                             | CRUD operations for areas (tables)              |
| [/api/v1/transaction/accountingTransaction](accountingTransaction.md#) | CRUD operations for accounting transaction      |
| [/api/v1/transaction/accountingEntry](inventory.md#)                   | CRUD operations for accounting entry            |
| [/api/v1/inventory](inventory.md#)                                     | CRUD operations for inventories                 |
| [/api/v1/inventory/card](card.md#)                                     | Receive or issue the stock with inventory cards |
| [/api/v1/inventory/item](item.md#)                                     | Work with stock items                           |
| /api/v1/inventory/recipe                                               | Create and assign recipes to menu items         |
| /api/v1/inventory/movement (todo define)                               | Read inventory movements not grouped in cards   |
| [/api/v1/supplier](supplier.md#)                                       | CRUD for supplier                               |
| /api/v1/order                                                          | Create order to POS                             |
| /api/v1/                                                               |                                                 |
| [/api/v1/transaction/payment](payment.md#)                             | Pay for a transaction                           |
| [/api/v1/transaction/discount](discount.md#)                           | Discount a transaction                          |




**Sending request**

### HTTP request headers

- **Content-Type**: application/json
- **Accept**: application/json

***REQUEST DESCRIPTION***

- header must contain access token generated in Papaya POS administration account

```
Authorization: Bearer <token>
```

- body contains data in a form of JSON

```
{}
```

> Currently only way to send request is to use Http POST request on correct endpoint with body containing `action` and `data`.

**Request**

All requests must contain `action` and `request`. Currently there are 3 defined actions for get, create or update, and delete.

In case you want to send empty `data`, you need to put in empty json. In case when `data` is missing in request, API will respond with error and request will be ignored.

**Actions**

| action | description |
| :---   | :---          |
| GET | Fetch data, may use data in request to narrow results |
| UPDATE | Create or update with data provided in request |
| DELETE | Delete objects using data provided in request |

**Example**

You can find examples for each endpoint and action in wiki. This is example of empty `data` with GET `action`.

```json

{
  "action": "GET",
  "data": {}
}
```

Errors from storehouse api, all storehouse errors are from range 4000-4999

| error  |  Description   |
| :---        |  :---     |
| 4000: Default storehouse cannot be deleted | When attempting to delete default storehouse. |

**General Error messages**

| error  | Description   |
| :---        |    ----:   |
| 1: Empty request not allowed | Endpoint received empty request and it does not allow empty requests |
| 2: Field value required: field_name | Returned when request is missing required field. field_name will be replaced by name of field that was missing |
| 3: This endpoint or method is not supported | This endpoint does not exist |
| 4: Action in request is not supported| When you provide wrong *action* in api request |
| 5: Method is not supported, use POST requests with action defined in body | If you send GET or DELETE requests on endpoint |
| 6: Bad request, check documentation for the correct form of requests | Request has something missing or is not correct or
