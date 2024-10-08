---
title: Integrate Payment Link
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
The system allows generating customized payment links that include specific reservation information. These links can be used to redirect users to a preconfigured payment page.

## Payment Process Flow Diagram

The following diagram illustrates the complete flow of the payment process, from link generation to reservation confirmation:

<Image align="center" className="border" src="https://mermaid.ink/svg/pako:eNqNlE9v2zAMxb-KoFMLZNjdhwLZ2i0FksFL4JsvnE07RG3JlagUWdHvPsr_knQZOp9s86enxyeBr7qwJepEe3wOaAq8J6gdtLlR8kDB1qlMgVeZRzf87MAxFdSBYbXZxtoXa5_I1OrB1GTwb2qdRiqFY4vxi8yT-jx_plBfWZJeLEmdLdB7e8XBKnIry9jkZqhmn-7uNttEPRpiAkavHIr5AzDZEdlshVmnifqOBl3PdONWjbg7g7Ikbn6g8iqSjTLLIto7Q7q5qXU6qNyT7xo4npDKuvZS5cEwuhNQAsOZRirEDk3p-0JcrrohFol-4NKJG_M6M3TzQry3gVWQKBTFneRwJZDb8aQbVrvQt1GFZlo21GbpaPKHZapIlP1An5Cp0y2W5LBgr9hOlMq26xM5ndDSOTqIFPDMnYKbJVdT3y_4ay83Td1MTRXWVORaOOtjPrgYw8-ALlpdpo99XsUeXI2qRAZq_LvmoqH5qP9F9tKR_DpsLd5NqSoy0NDvazcNG4_qm4hg-T-hVkIGhx-EOlIfhjpxl6HOPazEe3Np-tKApJ4bvdAtSshUypR4jYVc8x5bzHUiryVWEBrOdW7eBIXAdnc0hU7YBVxoZ0O910kFksNCh07u7jRiJkQ6kymzGcZQP43e_gDTEXza" />

### Explanation of the diagram

1. **Booking initiation**: The user starts the booking process in the Booking Engine.
2. **Payment link generation**: The Booking Engine generates a unique payment link and provides it to the user.
3. **Access to the payment page**: The user accesses the payment page through the provided link.
4. **Payment data entry**: The user enters their payment details on the page.
5. **Payment processing**: The data is sent to the Payment Processor for processing.
6. **Payment result**:
   * If the payment is successful:\
     a. The user is redirected to the success URL.\
     b. A webhook is sent to the hotel to confirm the payment.\
     c. The Booking Engine queries the Payment Processor's API to verify the charge details.\
     d. The Booking Engine confirms and finalizes the reservation.
   * If the payment fails:\
     a. The user is redirected to the failure URL.\
     b. The Booking Engine handles the reservation failure.

This flow ensures that the payment is processed securely and that the reservation is confirmed only after a complete payment verification.

## Link Structure

The payment link has the following base structure:

```
https://{domain}/hotel/{slug}?params={encoded_parameters}
```

Where:

* `{domain}` is the domain of the payment service (see the "Environments" section below).
* `{slug}` is the unique identifier of the hotel.
* `{encoded_parameters}` are the booking parameters encoded in Base64.

## Environments

The system provides two environments for generating and processing payment links:

1. **Production Environment**
   * Base URL: `https://link-de-pago.hotelpay.net`
   * This is the environment for real transactions and should be used in production systems.

2. **Sandbox Environment (Testing)**
   * Base URL: `https://link-de-pago-partner.webflow.io`
   * This environment is for testing and development. Use it to test your integration without processing real payments.

Make sure to use the appropriate environment according to your development or production needs.

## Required Parameters

The following parameters can be included in the link:

| Parameter       | Type    | Description                                           |
| --------------- | ------- | ----------------------------------------------------- |
| referenceId     | string  | Booking reference identifier                          |
| booking\_number | string  | Reservation number                                    |
| checkin         | string  | Check-in date (format: YYYY-MM-DD)                    |
| checkout        | string  | Check-out date (format: YYYY-MM-DD)                   |
| guests          | integer | Number of guests                                      |
| rooms           | integer | Number of rooms                                       |
| total           | number  | Total reservation amount (calculated if not provided) |
| subtotal        | number  | Reservation subtotal (ignored if total is provided)   |
| taxes           | number  | Applicable taxes (ignored if total is provided)       |
| discounts       | number  | Applied discounts (ignored if total is provided)      |
| currency        | string  | Currency code (e.g., "MXN")                           |
| url\_success    | string  | Redirect URL for successful payment                   |
| url\_failure    | string  | Redirect URL for failed payment                       |

### Important notes about the parameters:

1. **Total vs. Subtotal, Taxes, and Discounts**:
   * If the `total` parameter is provided, the `subtotal`, `taxes`, and `discounts` parameters will be ignored.
   * If `total` is not provided, it will be automatically calculated by adding `subtotal` and `taxes`, and subtracting `discounts`.
   * It is recommended to provide either `total`, or the set of `subtotal`, `taxes`, and `discounts`.

2. **Default values**:
   * All numeric parameters (`guests`, `rooms`, `total`, `subtotal`, `taxes`, `discounts`) have 0 as their default value.
   * The `currency` parameter has "MXN" as its default value.

3. **Success and failure URLs**:
   * If `url_success` or `url_failure` are not provided, the payment link will not perform redirections after the payment process.
   * Instead, it will display a successful or failed payment message directly on the payment page.

4. **referenceId and booking\_number**:
   * `referenceId`: This parameter is used to provide a unique identifier for the reservation. This value will be returned in the webhook as the `reference` field.
   * `booking_number`: This parameter is used to provide the reservation number. This value will be returned in the webhook as the `booking_number` field.
   * These parameters are optional but highly recommended to facilitate tracking and reconciliation of transactions.

## Link Generation Process

1. Create a JSON object with the required parameters.
2. Convert the JSON object to a string.
3. Encode the JSON string in Base64.
4. URL-encode the Base64 result (replacing unsafe characters).
5. Construct the final URL with the hotel slug and encoded parameters.

## Implementation Example

```python
import json
import base64
import urllib.parse

def generate_payment_link(hotel_slug, reservation_data):
    # 1. Create a dictionary with the parameters
    params = {
        "checkin": reservation_data["checkin"],
        "checkout": reservation_data["checkout"],
        "guests": reservation_data["guests"],
        "rooms": reservation_data["rooms"],
        "subtotal": reservation_data["subtotal"],
        "taxes": reservation_data["taxes"],
        "discounts": reservation_data["discounts"],
        "total": reservation_data["total"],
        "currency": reservation_data["currency"],
        "url_success": reservation_data["url_success"],
        "url_failure": reservation_data["url_failure"]
    }

    # 2 y 3. Convert to JSON and encode in Base64
    params_json = json.dumps(params)
    params_base64 = base64.b64encode(params_json.encode('utf-8')).decode('utf-8')

    # 4. Encode for URL
    params_encoded = urllib.parse.quote(params_base64)

    # 5. Build the final URL
    base_url = "https://link-de-pago.hotelpay.net"
    payment_link = f"{base_url}/hotel/{hotel_slug}?params={params_encoded}"

    return payment_link

# Usage example
hotel_slug = "hotel-test"
reservation_data = {
    "checkin": "2024-07-15",
    "checkout": "2024-07-20",
    "guests": "2",
    "rooms": "1",
    "subtotal": "1000",
    "taxes": "150",
    "discounts": "50",
    "total": "1100",
    "currency": "MXN",
    "url_success": "https://mihotel.com/reserva-exitosa",
    "url_failure": "https://mihotel.com/reserva-fallida"
}

payment_link = generate_payment_link(hotel_slug, reservation_data)
print(payment_link)
```
```javascript Javascript
function generatePaymentLink(hotelSlug, reservationData) {
  // 1. Create a JSON object
  const params = {
    checkin: reservationData.checkin,
    checkout: reservationData.checkout,
    guests: reservationData.guests,
    rooms: reservationData.rooms,
    subtotal: reservationData.subtotal,
    taxes: reservationData.taxes,
    discounts: reservationData.discounts,
    total: reservationData.total,
    currency: reservationData.currency,
    url_success: reservationData.url_success,
    url_failure: reservationData.url_failure
  };

  // 2 y 3. Convert to JSON and encode in Base64
  const paramsBase64 = btoa(JSON.stringify(params));

  // 4. Encode for URL
  const paramsEncoded = encodeURIComponent(paramsBase64);

  // 5. Build the final URL
  const baseUrl = "https://link-de-pago.hotelpay.net";
  const paymentLink = `${baseUrl}/hotel/${hotelSlug}?params=${paramsEncoded}`;

  return paymentLink;
}

// Usage example
const hotelSlug = "hotel-test";
const reservationData = {
  checkin: "2024-07-15",
  checkout: "2024-07-20",
  guests: "2",
  rooms: "1",
  subtotal: "1000",
  taxes: "150",
  discounts: "50",
  total: "1100",
  currency: "MXN",
  url_success: "https://mihotel.com/reserva-exitosa",
  url_failure: "https://mihotel.com/reserva-fallida"
};

const paymentLink = generatePaymentLink(hotelSlug, reservationData);
console.log(paymentLink);
```

## Payment Confirmation Process

It is important to note that the booking engine should not confirm the payment based solely on the user being redirected to the success URL. The correct payment confirmation process is as follows:

1. **User Redirection**: When the payment is successful, the user is redirected to the provided success URL. This redirection is primarily for user experience and should not be considered as a definitive payment confirmation.

2. **Webhook Reception**: The payment system will send a webhook to the hotel (if configured) with the payment confirmation. This webhook contains detailed and reliable information about the payment status.

3. **Booking Confirmation**: Only after receiving the webhook should the booking engine confirm and finalize the reservation.

### Handling Failures

In case the payment fails, the user will be redirected to the failure URL. The booking engine should be prepared to handle this situation, possibly offering the user the option to try the payment again or cancel the reservation.

### Security Considerations

* Do not rely solely on the redirection to the success URL to confirm a payment.
* Implement webhook verification.
* Ensure all communications are made through secure connections (HTTPS).
* Validate the authenticity of received webhooks to prevent fraud.

## Webhooks

When configured, the system will send a webhook with detailed information about the transaction once the payment is completed. This information is crucial for updating the reservation status in your system.

### Test Configuration

For testing and development purposes, it is recommended to use the webhook.site service. This service provides a unique URL to which webhooks can be sent, allowing you to view and analyze responses in real-time.

### Webhook Response Structure

Below is an example of the webhook response, with sensitive data masked:

```json
{
  "uuid": "7f53a881-91f0-4088-9bca-08ee7960b1a0",
  "reference": "5aa72cdf-f98b-4391-b6af-45f615e9524f",
  "booking_number": null,
  "amount": 1000,
  "fee": 0,
  "currency": "MXN",
  "checkin": "05/07/2024",
  "checkout": "06/07/2024",
  "description": "Reserva de hotel",
  "customer_info": {
    "first_name": "Alex",
    "last_name": "G*****",
    "email": "pr****@re*****.com",
    "ip": "177.***.***.***",
    "phone": "33********",
    "user_agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/126.0.0.0 Safari/537.36 Edg/126.0.0.0",
    "browser_info": {
      "language": "en-US",
      "color_depth": "1",
      "java_enabled": true,
      "screen_width": 564,
      "accept_header": "*/*",
      "screen_height": 200,
      "java_script_enabled": true
    }
  },
  "items": [
    {
      "price": 1000,
      "quantity": 1,
      "description": "Reserva de hotel"
    }
  ],
  "payment_mode": "single",
  "order_status": "inactive",
  "payment_status": "paid",
  "created_at": "01/07/2024 21:16:38",
  "merchant": {
    "name": "Merchant Test",
    "slug": "merchant-test",
    "logo": "<img src=\"https://hotelpay-merchants.s3.us-east-2.amazonaws.com/logos/66673da2078ae\" alt=\"\" height=\"120\">",
    "billing_email": null,
    "reservation_email": "pa***@re*****.com",
    "reservation_email_bcc": null,
    "bg_color": "#d16363",
    "text_color": "#ba6a6a",
    "address": "Tiburon, -, -, Guadalajara, Jalisco, CP 32400",
    "address_phone": null,
    "contact_info": "<p>Llámanos al</p>",
    "pictures": []
  },
  "transactions": [
    {
      "priority": "primary",
      "kind": "card",
      "amount": 1000,
      "card_address": {
        "address": "Av ******** 1995",
        "city": "Guadalajara",
        "country": "México",
        "country_code": "MX",
        "zip": "123456",
        "address2": null,
        "state": "Jalisco"
      },
      "card": {
        "bin": "400000",
        "type": "credit",
        "brand": "visa",
        "bank_name": "Intl Hdqtrs-Center Owned",
        "last_4_digits": "0051",
        "country_code": "US",
        "url_brand": "https://testing.hotelpay.net/storage/img/brands/flats/visa.svg"
      },
      "card_info": {
        "holder_name": "Alex",
        "expiration": "12/2030"
      },
      "installment": "1",
      "monthly": "$1,000.00 MXN",
      "status": "capture",
      "descriptor": "HOTELPAY"
    }
  ],
  "transaction_template": [
    {
      "BRAND": "visa",
      "CARD_ICON": "https://testing.hotelpay.net/storage/img/brands/flats/visa.svg",
      "TYPE": "credit",
      "LAST_4_DIGITS": "0051",
      "HOLDER_NAME": "Alex",
      "MERCHANT_DESCRIPTOR": "HOTELPAY",
      "INSTALLMENT": "1",
      "MONTHLY": "$1,000.00 MXN"
    }
  ],
  "holder_name_diff": true,
  "payment_adds": {
    "date": "01/07/2024 21:16",
    "amount": "$1,000.00 MXN"
  }
}
```

### Important Fields

* `uuid`: Unique identifier of the transaction.
* `reference`: Corresponds to the `referenceId` provided in the payment link parameters.
* `booking_number`: Corresponds to the `booking_number` provided in the payment link parameters.
* `amount`: Total amount of the transaction.
* `currency`: Currency of the transaction.
* `checkin` and `checkout`: Check-in and check-out dates.
* `payment_status`: Payment status (in this case, "paid").
* `transactions`: Transaction details, including information about the card used.

## Additional Notes

* All parameter values must be passed as strings.
* Dates should be in "YYYY-MM-DD" format.
* The currency code must be a valid string (for example, "MXN" for Mexican pesos).
* Success and failure URLs must be valid and complete URLs, including the protocol (http\:// or https\://).
* It is the developer's responsibility to ensure that the calculations of subtotal, taxes, discounts, and total are correct before generating the link.

## Security

* Do not include additional sensitive information in the parameters, as these can be decoded.
* Consider implementing additional security measures, such as digital signatures or one-time tokens, to prevent link manipulation.
* Ensure that the success and failure URLs (url\_success and url\_failure) are valid and secure URLs within your domain.

## Support:

If you have questions about the webhook configuration or operation, you can request support from the Hotelpay support team via email at [soporte@hotelpay.com](mailto:soporte@hotelpay.com).

We hope this information is helpful to you.