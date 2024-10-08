---
title: Integrate Iframe
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
<Image align="center" className="border" src="https://files.readme.io/83dc0a1-iframe-flow.png" />

<br />

## Example

```js JavaScript
// Custom button
const btnReserve = document.querySelector("#btnReserve");

const initIframe = async () => {
  Hotelpay.Checkout.setApiKey("pk_1237932718");
  Hotelpay.Checkout.setMode(Hotelpay.TESTING);
  Hotelpay.Checkout.setTotal(444.44);
  Hotelpay.Checkout.setDetails({
      reserve_data: {
        booking_number: "1234",
        checkin: "2024-10-01",
        checkout: "2024-10-30",
      },
      customer_info: {
        first_name: "VICTOR",
        last_name: "ALVAREZ",
        email: "victor@admin.com",
        phone: "7777777777"
      },
      customer_address: {
        address: "123 Calle Principal",
        address2: "Apartamento 4",
        city: "Ciudad Central",
        state: "Estado Grande",
        country: "País Imaginario",
        country_code: "MX",
        zip: "12345",
      }
    });
  await Hotelpay.Checkout.renderIframe("#container");

  Hotelpay.Checkout.on("complete-form-card", (eventData) => {
    // enable / disable button to continue
    btnReserve.disabled = !e.is_valid;
  });
  Hotelpay.Checkout.on("adding-card-start", () => {
    btnReserve.disabled = true;
    // show "loading"
  });
  Hotelpay.Checkout.on("adding-card-end", () => {
    btnReserve.disabled = false;
    // hide "loading"
  });
  Hotelpay.Checkout.on("msi", (eventData) => {
    // no action needed
    console.log(eventData);
  });
  Hotelpay.Checkout.on("transaction-complete", () => {
    // close checkout
  });
  Hotelpay.Checkout.on("transaction-failed", (eventData) => {
    // show error message
    // hide "loading"
  });
};
```