# Type safe data sync
Sync data from 3rd party services safely using python _dataclass_ and _mypy_.
in this example, we will use Shopify order model to demonstrate our method.

## Steps
### Building the Order dataclass
Using a natural language model, you can convert this huge json payload received from Shopify's `order/create` webhook into an `Order` dataclass
```json
{
  "id": 820982911946154508,
  "admin_graphql_api_id": "gid:\/\/shopify\/Order\/820982911946154508",
  "app_id": null,
  "browser_ip": null,
  "buyer_accepts_marketing": true,
  "cancel_reason": "customer",
  "cancelled_at": "2021-12-31T19:00:00-05:00",
  "cart_token": null,
  "checkout_id": null,
  "checkout_token": null,
  "client_details": null,
  "closed_at": null,
  "confirmation_number": null,
  "confirmed": false,
  "contact_email": "jon@example.com",
  "created_at": "2021-12-31T19:00:00-05:00",
  "currency": "USD",
  "current_subtotal_price": "398.00",
  "current_subtotal_price_set": {
    "shop_money": {
      "amount": "398.00",
      "currency_code": "USD"
    },
    "presentment_money": {
      "amount": "398.00",
      "currency_code": "USD"
    }
  },
  "current_total_additional_fees_set": null,
  "current_total_discounts": "0.00",
  "current_total_discounts_set": {
    "shop_money": {
      "amount": "0.00",
      "currency_code": "USD"
    },
    "presentment_money": {
      "amount": "0.00",
      "currency_code": "USD"
    }
  },
  "current_total_duties_set": null,
  "current_total_price": "398.00",
  "current_total_price_set": {
    "shop_money": {
      "amount": "398.00",
      "currency_code": "USD"
    },
    "presentment_money": {
      "amount": "398.00",
      "currency_code": "USD"
    }
  },
  "current_total_tax": "0.00",
  "current_total_tax_set": {
    "shop_money": {
      "amount": "0.00",
      "currency_code": "USD"
    },
    "presentment_money": {
      "amount": "0.00",
      "currency_code": "USD"
    }
  },
  "customer_locale": "en",
  "device_id": null,
  "discount_codes": [

  ],
  "email": "jon@example.com",
  "estimated_taxes": false,
  "financial_status": "voided",
  "fulfillment_status": "pending",
  "landing_site": null,
  "landing_site_ref": null,
  "location_id": null,
  "merchant_of_record_app_id": null,
  "name": "#9999",
  "note": null,
  "note_attributes": [

  ],
  "number": 234,
  "order_number": 1234,
  "order_status_url": "https:\/\/jsmith.myshopify.com\/548380009\/orders\/123456abcd\/authenticate?key=abcdefg",
  "original_total_additional_fees_set": null,
  "original_total_duties_set": null,
  "payment_gateway_names": [
    "visa",
    "bogus"
  ],
  "phone": null,
  "po_number": null,
  "presentment_currency": "USD",
  "processed_at": null,
  "reference": null,
  "referring_site": null,
  "source_identifier": null,
  "source_name": "web",
  "source_url": null,
  "subtotal_price": "388.00",
  "subtotal_price_set": {
    "shop_money": {
      "amount": "388.00",
      "currency_code": "USD"
    },
    "presentment_money": {
      "amount": "388.00",
      "currency_code": "USD"
    }
  },
  "tags": "tag1, tag2",
  "tax_exempt": false,
  "tax_lines": [

  ],
  "taxes_included": false,
  "test": true,
  "token": "123456abcd",
  "total_discounts": "20.00",
  "total_discounts_set": {
    "shop_money": {
      "amount": "20.00",
      "currency_code": "USD"
    },
    "presentment_money": {
      "amount": "20.00",
      "currency_code": "USD"
    }
  },
  "total_line_items_price": "398.00",
  "total_line_items_price_set": {
    "shop_money": {
      "amount": "398.00",
      "currency_code": "USD"
    },
    "presentment_money": {
      "amount": "398.00",
      "currency_code": "USD"
    }
  },
  "total_outstanding": "398.00",
  "total_price": "388.00",
  "total_price_set": {
    "shop_money": {
      "amount": "388.00",
      "currency_code": "USD"
    },
    "presentment_money": {
      "amount": "388.00",
      "currency_code": "USD"
    }
  },
  "total_shipping_price_set": {
    "shop_money": {
      "amount": "10.00",
      "currency_code": "USD"
    },
    "presentment_money": {
      "amount": "10.00",
      "currency_code": "USD"
    }
  },
  "total_tax": "0.00",
  "total_tax_set": {
    "shop_money": {
      "amount": "0.00",
      "currency_code": "USD"
    },
    "presentment_money": {
      "amount": "0.00",
      "currency_code": "USD"
    }
  },
  "total_tip_received": "0.00",
  "total_weight": 0,
  "updated_at": "2021-12-31T19:00:00-05:00",
  "user_id": null,
  "billing_address": {
    "first_name": "Steve",
    "address1": "123 Shipping Street",
    "phone": "555-555-SHIP",
    "city": "Shippington",
    "zip": "40003",
    "province": "Kentucky",
    "country": "United States",
    "last_name": "Shipper",
    "address2": null,
    "company": "Shipping Company",
    "latitude": null,
    "longitude": null,
    "name": "Steve Shipper",
    "country_code": "US",
    "province_code": "KY"
  },
  "customer": {
    "id": 115310627314723954,
    "email": "john@example.com",
    "created_at": null,
    "updated_at": null,
    "first_name": "John",
    "last_name": "Smith",
    "state": "disabled",
    "note": null,
    "verified_email": true,
    "multipass_identifier": null,
    "tax_exempt": false,
    "phone": null,
    "email_marketing_consent": {
      "state": "not_subscribed",
      "opt_in_level": null,
      "consent_updated_at": null
    },
    "sms_marketing_consent": null,
    "tags": "",
    "currency": "USD",
    "tax_exemptions": [

    ],
    "admin_graphql_api_id": "gid:\/\/shopify\/Customer\/115310627314723954",
    "default_address": {
      "id": 715243470612851245,
      "customer_id": 115310627314723954,
      "first_name": null,
      "last_name": null,
      "company": null,
      "address1": "123 Elm St.",
      "address2": null,
      "city": "Ottawa",
      "province": "Ontario",
      "country": "Canada",
      "zip": "K2H7A8",
      "phone": "123-123-1234",
      "name": "",
      "province_code": "ON",
      "country_code": "CA",
      "country_name": "Canada",
      "default": true
    }
  },
  "discount_applications": [

  ],
  "fulfillments": [

  ],
  "line_items": [
    {
      "id": 866550311766439020,
      "admin_graphql_api_id": "gid:\/\/shopify\/LineItem\/866550311766439020",
      "attributed_staffs": [
        {
          "id": "gid:\/\/shopify\/StaffMember\/902541635",
          "quantity": 1
        }
      ],
      "current_quantity": 1,
      "fulfillable_quantity": 1,
      "fulfillment_service": "manual",
      "fulfillment_status": null,
      "gift_card": false,
      "grams": 567,
      "name": "IPod Nano - 8GB",
      "price": "199.00",
      "price_set": {
        "shop_money": {
          "amount": "199.00",
          "currency_code": "USD"
        },
        "presentment_money": {
          "amount": "199.00",
          "currency_code": "USD"
        }
      },
      "product_exists": true,
      "product_id": 632910392,
      "properties": [

      ],
      "quantity": 1,
      "requires_shipping": true,
      "sku": "IPOD2008PINK",
      "taxable": true,
      "title": "IPod Nano - 8GB",
      "total_discount": "0.00",
      "total_discount_set": {
        "shop_money": {
          "amount": "0.00",
          "currency_code": "USD"
        },
        "presentment_money": {
          "amount": "0.00",
          "currency_code": "USD"
        }
      },
      "variant_id": 808950810,
      "variant_inventory_management": "shopify",
      "variant_title": null,
      "vendor": null,
      "tax_lines": [

      ],
      "duties": [

      ],
      "discount_allocations": [

      ]
    },
    {
      "id": 141249953214522974,
      "admin_graphql_api_id": "gid:\/\/shopify\/LineItem\/141249953214522974",
      "attributed_staffs": [

      ],
      "current_quantity": 1,
      "fulfillable_quantity": 1,
      "fulfillment_service": "manual",
      "fulfillment_status": null,
      "gift_card": false,
      "grams": 567,
      "name": "IPod Nano - 8GB",
      "price": "199.00",
      "price_set": {
        "shop_money": {
          "amount": "199.00",
          "currency_code": "USD"
        },
        "presentment_money": {
          "amount": "199.00",
          "currency_code": "USD"
        }
      },
      "product_exists": true,
      "product_id": 632910392,
      "properties": [

      ],
      "quantity": 1,
      "requires_shipping": true,
      "sku": "IPOD2008PINK",
      "taxable": true,
      "title": "IPod Nano - 8GB",
      "total_discount": "0.00",
      "total_discount_set": {
        "shop_money": {
          "amount": "0.00",
          "currency_code": "USD"
        },
        "presentment_money": {
          "amount": "0.00",
          "currency_code": "USD"
        }
      },
      "variant_id": 808950810,
      "variant_inventory_management": "shopify",
      "variant_title": null,
      "vendor": null,
      "tax_lines": [

      ],
      "duties": [

      ],
      "discount_allocations": [

      ]
    }
  ],
  "payment_terms": null,
  "refunds": [

  ],
  "shipping_address": {
    "first_name": "Steve",
    "address1": "123 Shipping Street",
    "phone": "555-555-SHIP",
    "city": "Shippington",
    "zip": "40003",
    "province": "Kentucky",
    "country": "United States",
    "last_name": "Shipper",
    "address2": null,
    "company": "Shipping Company",
    "latitude": null,
    "longitude": null,
    "name": "Steve Shipper",
    "country_code": "US",
    "province_code": "KY"
  },
  "shipping_lines": [
    {
      "id": 271878346596884015,
      "carrier_identifier": null,
      "code": null,
      "discounted_price": "10.00",
      "discounted_price_set": {
        "shop_money": {
          "amount": "10.00",
          "currency_code": "USD"
        },
        "presentment_money": {
          "amount": "10.00",
          "currency_code": "USD"
        }
      },
      "phone": null,
      "price": "10.00",
      "price_set": {
        "shop_money": {
          "amount": "10.00",
          "currency_code": "USD"
        },
        "presentment_money": {
          "amount": "10.00",
          "currency_code": "USD"
        }
      },
      "requested_fulfillment_service_id": null,
      "source": "shopify",
      "title": "Generic Shipping",
      "tax_lines": [

      ],
      "discount_allocations": [

      ]
    }
  ]
}
```
Will be converted to this
```python
from dataclasses import dataclass, field
from typing import List, Optional, Dict

@dataclass
class Money:
    amount: str
    currency_code: str

@dataclass
class Address:
    first_name: str
    address1: str
    phone: str
    city: str
    zip: str
    province: str
    country: str
    last_name: str
    address2: Optional[str] = None
    company: Optional[str] = None
    latitude: Optional[float] = None
    longitude: Optional[float] = None
    name: Optional[str] = None
    country_code: Optional[str] = None
    province_code: Optional[str] = None

@dataclass
class LineItem:
    id: int
    admin_graphql_api_id: str
    attributed_staffs: List[Dict[str, int]]
    current_quantity: int
    fulfillable_quantity: int
    fulfillment_service: str
    fulfillment_status: Optional[str] = None
    gift_card: bool
    grams: int
    name: str
    price: str
    price_set: Money
    product_exists: bool
    product_id: int
    properties: List[str]
    quantity: int
    requires_shipping: bool
    sku: str
    taxable: bool
    title: str
    total_discount: str
    total_discount_set: Money
    variant_id: int
    variant_inventory_management: str
    variant_title: Optional[str] = None
    vendor: Optional[str] = None
    tax_lines: List[Dict[str, str]]
    duties: List[str]
    discount_allocations: List[str]

@dataclass
class ShippingLine:
    id: int
    carrier_identifier: Optional[str] = None
    code: Optional[str] = None
    discounted_price: str
    discounted_price_set: Money
    phone: Optional[str] = None
    price: str
    price_set: Money
    requested_fulfillment_service_id: Optional[str] = None
    source: str
    title: str
    tax_lines: List[str]
    discount_allocations: List[str]

@dataclass
class Order:
    id: int
    admin_graphql_api_id: str
    app_id: Optional[str] = None
    browser_ip: Optional[str] = None
    buyer_accepts_marketing: bool
    cancel_reason: str
    cancelled_at: str
    cart_token: Optional[str] = None
    checkout_id: Optional[str] = None
    checkout_token: Optional[str] = None
    client_details: Optional[str] = None
    closed_at: Optional[str] = None
    confirmation_number: Optional[str] = None
    confirmed: bool
    contact_email: str
    created_at: str
    currency: str
    current_subtotal_price: str
    current_subtotal_price_set: Money
    current_total_additional_fees_set: Optional[str] = None
    current_total_discounts: str
    current_total_discounts_set: Money
    current_total_duties_set: Optional[str] = None
    current_total_price: str
    current_total_price_set: Money
    current_total_tax: str
    current_total_tax_set: Money
    customer_locale: str
    device_id: Optional[str] = None
    discount_codes: List[str]
    email: str
    estimated_taxes: bool
    financial_status: str
    fulfillment_status: str
    landing_site: Optional[str] = None
    landing_site_ref: Optional[str] = None
    location_id: Optional[str] = None
    merchant_of_record_app_id: Optional[str] = None
    name: str
    note: Optional[str] = None
    note_attributes: List[str]
    number: int
    order_number: int
    order_status_url: str
    original_total_additional_fees_set: Optional[str] = None
    original_total_duties_set: Optional[str] = None
    payment_gateway_names: List[str]
    phone: Optional[str] = None
    po_number: Optional[str] = None
    presentment_currency: str
    processed_at: Optional[str] = None
    reference: Optional[str] = None
    referring_site: Optional[str] = None
    source_identifier: Optional[str] = None
    source_name: str
    source_url: Optional[str] = None
    subtotal_price: str
    subtotal_price_set: Money
    tags: str
    tax_exempt: bool
    tax_lines: List[str]
    taxes_included: bool
    test: bool
    token: str
    total_discounts: str
    total_discounts_set: Money
    total_line_items_price: str
    total_line_items_price_set: Money
    total_outstanding: str
    total_price: str
    total_price_set: Money
    total_shipping_price_set: Money
    total_tax: str
    total_tax_set: Money
    total_tip_received: str
    total_weight: int
    updated_at: str
    user_id: Optional[str] = None
    billing_address: Address
    customer: Dict[str, object]
    discount_applications: List[str]
    fulfillments: List[str]
    line_items: List[LineItem]
    payment_terms: Optional[str] = None
    refunds: List[str]
    shipping_address: Address
    shipping_lines: List[ShippingLine]
```
### Converter function
Converter function transforms the received json payload into an instance of the `Order` dataclass. This function ensures that nested data is mapped correclty without the need to go through each field manually.
```python
from typing import TypeVar

T = TypeVar('T')
def convert_to_dataclass(dataclass_type: T, data) -> T:
    return dataclass_type(
        **{
            field: (
                convert_to_dataclass(field.type, value)
                if isinstance(value, dict)
                else value
            )
            for field, value in data.items()
        }
    )
```
### Using the converter function
Let's a Django vies to handle the `order/create` webhook
```python
from django.http import HttpRequest, HttpResponse
from django.views.decorators.csrf import csrf_exempt
from django.views.decorators.http import require_POST

from .decorators import verify_hmac

@require_POST
@csrf_exempt
@verify_hmac
def orders_create(request: HttpRequest) -> HttpResponse:
    payload = json.loads(request.body)
    data = convert_to_dataclass(OrderDataModel, payload)

    # Transform Order dataclass to Django Order Model
    order = sync_order(data, save=False)
    order.save()

    return HttpResponse(status=200)
```
### Verify using mypy
We will use _mypy_ to make sure that typing is correct, and most importantly check if there's some `Optional` fields mapped into a `NOT_NULL` database column.
Just run the following command direclty.
```bash
$ mypy views.py
```
Or setup project wise mypy type checking.
