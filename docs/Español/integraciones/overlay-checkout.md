---
title: Integrar Overlay Checkout
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
<Image align="center" className="border" src="https://files.readme.io/c833be4-es_flow.png" />

## Archivo JS + CSS

```html
<script src="https://js.hotelpay.net/sdk/latest/index.js" type="text/javascript"></script>
```

```html
<link href="https://js.hotelpay.net/sdk/latest/index.css" rel="stylesheet" />
```

## Ejemplo por defecto

Usando el botón por defecto, y eventos por defecto

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <link href="https://js.hotelpay.net/sdk/latest/index.css" rel="stylesheet" />
  </head>
  <body>
    <div style="padding: 16px">
      <div id="container" style="width: 400px"></div>
    </div>

    <script src=https://js.hotelpay.net/sdk/latest/index.js type="text/javascript"></script>
    <script type="text/javascript">
      Hotelpay.Checkout.setMode(Hotelpay.TESTING);
      Hotelpay.Checkout.setApiKey("pk_...");
      Hotelpay.Checkout.setDetails({
        referenceId: "my-internal-id",
        reserve_data: {
          booking_number: "00001",
          checkin: "2024-12-01",
          checkout: "2024-12-31",
          guests_adults: 2,
          guests_children: 1
        },
        items: [
          {
            "description": "Habitación doble vista al mar",
            "price": 56.79,
            "quantity": 1,
          },
          {
            "description": "Habitación sencilla",
            "price": 34.99,
            "quantity": 1,
          }
        ],
        customer_info: {
          first_name: "test",
          last_name: "test",
          phone: "6692788388",
          email: "test@hotelpay.com",
          ip: "1.1.1.1",
        },
        customer_address: {
          address: "123 Calle Principal",
          address2: "Apartamento 4",
          city: "Ciudad Central",
          state: "Estado Grande",
          country: "País Imaginario",
          country_code: "MX",
          zip: "12345",
        },
      });
      Hotelpay.Checkout.onSuccess(() => {
        console.log("Pagó exitoso")
      });
      Hotelpay.Checkout.setTotal(444.44);
      Hotelpay.Checkout.render("#container");
    </script>
  </body>
</html>
```

## Ejemplo flujo personalizado

Si necesitas personalizar el botón, o añadir el proceso de pago a otro pago ya existente

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <link href="https://js.hotelpay.net/sdk/latest/index.css" rel="stylesheet" />
  </head>
  <body>
    <div style="padding: 16px">
      <button id="my-button">Pagar</button>
    </div>

    <script src="https://js.hotelpay.net/sdk/latest/index.js" type="text/javascript"></script>
    <script type="text/javascript">
      Hotelpay.Checkout.setMode(Hotelpay.TESTING);
      Hotelpay.Checkout.setApiKey("pk_...");
      Hotelpay.Checkout.setTotal(444.44);
      // optional
      Hotelpay.Checkout.setDetails({
        referenceId: "my-internal-id",
        reserve_data: {
          booking_number: "00001",
          checkin: "2024-12-01",
          checkout: "2024-12-31"
        },
        items: [
          {
            "description": "Habitación doble vista al mar",
            "price": 444.44,
            "quantity": 1
          }
        ],
        customer_info: {
          first_name: "test",
          last_name: "test",
          phone: "6692788388",
          email: "test@hotelpay.com",
          ip: "1.1.1.1",
        },
        customer_address: {
          address: "123 Calle Principal",
          address2: "Apartamento 4",
          city: "Ciudad Central",
          state: "Estado Grande",
          country: "País Imaginario",
          country_code: "MX",
          zip: "12345",
        },
      });
      Hotelpay.Checkout.onSuccess(() => {
        console.log("Pagó exitoso");
      });

      const button = document.querySelector("#my-button");
      button.onclick = function () {
        Hotelpay.Checkout.pay();
      };
    </script>
  </body>
</html>
```