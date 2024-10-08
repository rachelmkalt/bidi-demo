---
title: Integrar Webhook
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
El sistema enviará un webhook con información detallada sobre la transacción una vez que se complete el pago. Esta información es crucial para actualizar el estado de la reserva en su sistema.

A continuación se explicaran unos ejemplos de integración para pruebas o en su efecto para conectar con una plataforma como pipedream

# Estructura JSON Webhook

Este documento proporciona una descripción detallada del JSON de un evento webhook.

## Ejemplo JSON

```json Estructura
{
  "uuid": "string",
  "reference": "string",
  "booking_number": "string | null",
  "amount": "number",
  "fee": "number",
  "currency": "string",
  "checkin": "string (format: DD/MM/YYYY)",
  "checkout": "string (format: DD/MM/YYYY)",
  "description": "string",
  "customer_info": {
    "first_name": "string",
    "last_name": "string",
    "email": "string",
    "ip": "string",
    "phone": "string",
    "user_agent": "string",
    "browser_info": {
      "language": "string",
      "color_depth": "string",
      "java_enabled": "boolean",
      "screen_width": "number",
      "accept_header": "string",
      "screen_height": "number",
      "java_script_enabled": "boolean"
    }
  },
  "items": [
    {
      "price": "number",
      "quantity": "number",
      "description": "string"
    }
  ],
  "payment_mode": "string",
  "order_status": "string",
  "payment_status": "string",
  "created_at": "string (format: DD/MM/YYYY HH:MM:SS)",
  "merchant": {
    "name": "string",
    "slug": "string",
    "logo": "string (HTML)",
    "billing_email": "string | null",
    "reservation_email": "string",
    "reservation_email_bcc": "string | null",
    "bg_color": "string (hex color)",
    "text_color": "string (hex color)",
    "address": "string",
    "address_phone": "string | null",
    "contact_info": "string (HTML)",
    "pictures": "array"
  },
  "transactions": [
    {
      "priority": "string",
      "kind": "string",
      "amount": "number",
      "card_address": {
        "address": "string",
        "city": "string",
        "country": "string",
        "country_code": "string",
        "zip": "string",
        "address2": "string | null",
        "state": "string"
      },
      "card": {
        "bin": "string",
        "type": "string",
        "brand": "string",
        "bank_name": "string",
        "last_4_digits": "string",
        "country_code": "string",
        "url_brand": "string (URL)"
      },
      "card_info": {
        "holder_name": "string",
        "expiration": "string (format: MM/YYYY)"
      },
      "installment": "string",
      "monthly": "string",
      "descriptor": "string"
    }
  ],
  "transaction_template": [
    {
      "BRAND": "string",
      "CARD_ICON": "string (URL)",
      "TYPE": "string",
      "LAST_4_DIGITS": "string",
      "HOLDER_NAME": "string",
      "MERCHANT_DESCRIPTOR": "string",
      "INSTALLMENT": "string",
      "MONTHLY": "string"
    }
  ],
  "holder_name_diff": "boolean",
  "payment_adds": {
    "date": "string (format: DD/MM/YYYY HH:MM)",
    "amount": "string"
  }
}
```
```json Ejemplo
{
  "uuid": "f5b07c64-a98a-4b9e-8fbc-9cef25cc7659",
  "reference": "f80f1431-1d94-44d0-ab30-b3a6b7d19d4e",
  "booking_number": null,
  "amount": 1000,
  "fee": 0,
  "currency": "MXN",
  "checkin": "05/07/2024",
  "checkout": "06/07/2024",
  "description": "Reserva de hotel",
  "customer_info": {
    "first_name": "Alex",
    "last_name": "Galindo",
    "email": "pruebas@retrypay.com",
    "ip": "177.249.170.25",
    "phone": "3311224455",
    "user_agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/126.0.0.0 Safari/537.36 Edg/126.0.0.0",
    "browser_info": {
      "language": "en-US",
      "color_depth": "1",
      "java_enabled": true,
      "screen_width": 564,
      "accept_header": "*/",
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
  "created_at": "01/07/2024 20:56:04",
  "merchant": {
    "name": "Merchant Test",
    "slug": "merchant-test",
    "logo": "<img src=\"https://hotelpay-merchants.s3.us-east-2.amazonaws.com/logos/66673da2078ae\" alt=\"\" height=\"120\">",
    "billing_email": null,
    "reservation_email": "pagos@retrypay.com",
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
        "address": "Av Hidalgo 1995",
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
    "date": "01/07/2024 20:56",
    "amount": "$1,000.00 MXN"
  }
}
```

## Estructura del JSON

***

| Campo                 | Descripción                                                 | Tipo de Dato  | Ejemplo                                                     |
| --------------------- | ----------------------------------------------------------- | ------------- | ----------------------------------------------------------- |
| uuid                  | Identificador único universal para la reserva.              | String        | f5b07c64-a98a-4b9e-8fbc-9cef25cc7659                        |
| reference             | Referencia interna de la reserva.                           | String        | f80f1431-1d94-44d0-ab30-b3a6b7d19d4e                        |
| booking\_number       | Número de reserva (no disponible en este caso).             | String        | null                                                        |
| amount                | Monto total de la reserva en la moneda especificada.        | Number        | 1000                                                        |
| fee                   | Comisión aplicada a la transacción.                         | Number        | 0                                                           |
| currency              | Moneda utilizada (Pesos Mexicanos).                         | String        | MXN                                                         |
| checkin               | Fecha de entrada al hotel.                                  | String        | 05/07/2024                                                  |
| checkout              | Fecha de salida del hotel.                                  | String        | 06/07/2024                                                  |
| description           | Descripción de la reserva.                                  | String        | Reserva de hotel                                            |
| customer\_info        | Información del cliente                                     | json          | [Información del Cliente](#información-del-cliente)         |
| items                 | Detalles de las habitaciones                                | array of json | Detalles de las habitaciones )                              |
| payment\_mode         | Modalidad de pago (único en este caso).                     | String        | single                                                      |
| order\_status         | Estado de la orden (inactiva).                              | String        | inactive                                                    |
| payment\_status       | Estados del pago (pagado).                                  | String        | [Estados del pago](#estados-del-pago)                       |
| created\_at           | Fecha y hora de creación de la reserva.                     | String        | 01/07/2024 20:56:04                                         |
| merchant              | Información del merchant                                    | json          | [Información del Merchant](#información-del-merchant)       |
| transactions          | Transacciones                                               | array of json | [Transacciones](#transacciones)                             |
| transaction\_template | Plantilla de la Transacción ( Sendgrid)                     | array of json | [Plantilla de la Transacción](#plantilla-de-la-transacción) |
| holder\_name\_diff    | Indica si el nombre del titular de la tarjeta es diferente. | Boolean       | true                                                        |
| payment\_adds         | Información Adicional                                       | json          | [Información Adicional](#información-adicional)             |

### Información del Cliente

| Campo         | Descripción                                                | Tipo de Dato | Ejemplo                                                                                                                               |
| ------------- | ---------------------------------------------------------- | ------------ | ------------------------------------------------------------------------------------------------------------------------------------- |
| first\_name   | Nombre del cliente.                                        | String       | Alex                                                                                                                                  |
| last\_name    | Apellido del cliente.                                      | String       | Galindo                                                                                                                               |
| email         | Correo electrónico del cliente.                            | String       | correo....                                                                                                                            |
| ip            | Dirección IP del cliente.                                  | String       | 177.229.160.25                                                                                                                        |
| phone         | Número de teléfono del cliente.                            | String       | 3311224455                                                                                                                            |
| user\_agent   | Información del navegador y sistema operativo del cliente. | String       | Mozilla/5.0 (Macintosh; Intel Mac OS X 10\_15\_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/126.0.0.0 Safari/537.36 Edg/126.0.0.0 |
| browser\_info | Información del Navegador                                  | json         | [Información del Navegador del Cliente](#información-del-navegador-del-cliente)                                                       |

### Información del Navegador del Cliente

<Table>
  <thead>
    <tr>
      <th>
        Campo
      </th>

      <th>
        Descripción
      </th>

      <th>
        Tipo de Dato
      </th>

      <th>
        Ejemplo
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        language
      </td>

      <td>
        Idioma del navegador.
      </td>

      <td>
        String
      </td>

      <td>
        en-US
      </td>
    </tr>

    <tr>
      <td>
        color_depth
      </td>

      <td>
        Profundidad de color de la pantalla.
      </td>

      <td>
        String
      </td>

      <td>
        1
      </td>
    </tr>

    <tr>
      <td>
        java_enabled
      </td>

      <td>
        Indica si Java está habilitado.
      </td>

      <td>
        Boolean
      </td>

      <td>
        true
      </td>
    </tr>

    <tr>
      <td>
        screen_width
      </td>

      <td>
        Ancho de la pantalla en píxeles.
      </td>

      <td>
        Number
      </td>

      <td>
        564
      </td>
    </tr>

    <tr>
      <td>
        accept_header
      </td>

      <td>
        Encabezado de aceptación del navegador.
      </td>

      <td>
        String
      </td>

      <td>
        \*

        /
      </td>
    </tr>

    <tr>
      <td>
        screen_height
      </td>

      <td>
        Altura de la pantalla en píxeles.
      </td>

      <td>
        Number
      </td>

      <td>
        200
      </td>
    </tr>

    <tr>
      <td>
        java_script_enabled
      </td>

      <td>
        Indica si JavaScript está habilitado.
      </td>

      <td>
        Boolean
      </td>

      <td>
        true
      </td>
    </tr>
  </tbody>
</Table>

### Detalles de las habitaciones

| Campo       | Descripción               | Tipo de Dato | Ejemplo          |
| ----------- | ------------------------- | ------------ | ---------------- |
| price       | Precio del artículo.      | Number       | 1000             |
| quantity    | Cantidad de artículos.    | Number       | 1                |
| description | Descripción del artículo. | String       | Reserva de hotel |

### Información del Merchant

<Table>
  <thead>
    <tr>
      <th>
        Campo
      </th>

      <th>
        Descripción
      </th>

      <th>
        Tipo de Dato
      </th>

      <th>
        Ejemplo
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        name
      </td>

      <td>
        Nombre del comerciante.
      </td>

      <td>
        String
      </td>

      <td>
        Merchant Test
      </td>
    </tr>

    <tr>
      <td>
        slug
      </td>

      <td>
        Identificador único del comerciante.
      </td>

      <td>
        String
      </td>

      <td>
        merchant-test
      </td>
    </tr>

    <tr>
      <td>
        logo
      </td>

      <td>
        Logo del comerciante en formato HTML.
      </td>

      <td>
        String
      </td>

      <td>
        <img src="&#x3C;https://hotelpay-merchants.s3.us-east-2.amazonaws.com/logos/66673da2078ae>" alt="" height="120" />
      </td>
    </tr>

    <tr>
      <td>
        billing_email
      </td>

      <td>
        Correo electrónico de facturación (puede no estar disponible).
      </td>

      <td>
        String
      </td>

      <td>
        correo...
      </td>
    </tr>

    <tr>
      <td>
        reservation_email
      </td>

      <td>
        Correo electrónico para reservas.
      </td>

      <td>
        String
      </td>

      <td>
        correo...
      </td>
    </tr>

    <tr>
      <td>
        reservation_email_bcc
      </td>

      <td>
        Correo electrónico en copia oculta para reservas (puede no estar disponible).
      </td>

      <td>
        String
      </td>

      <td>
        correos...
      </td>
    </tr>

    <tr>
      <td>
        bg_color
      </td>

      <td>
        Color de fondo.
      </td>

      <td>
        String
      </td>

      <td>
        \# d16363
      </td>
    </tr>

    <tr>
      <td>
        text_color
      </td>

      <td>
        Color del texto.
      </td>

      <td>
        String
      </td>

      <td>
        \# ba6a6a
      </td>
    </tr>

    <tr>
      <td>
        address
      </td>

      <td>
        Dirección del comerciante.
      </td>

      <td>
        String
      </td>

      <td>
        Tiburon, -, -, Guadalajara, Jalisco, CP 32400
      </td>
    </tr>

    <tr>
      <td>
        address_phone
      </td>

      <td>
        Teléfono de la dirección (no disponible).
      </td>

      <td>
        String
      </td>

      <td>
        null
      </td>
    </tr>

    <tr>
      <td>
        contact_info
      </td>

      <td>
        Información de contacto en formato HTML.
      </td>

      <td>
        String
      </td>

      <td>
        <p />

        Llámanos al

        <html xmlns="http://www.w3.org/1999/xhtml"><head /><body /></html>
      </td>
    </tr>

    <tr>
      <td>
        pictures
      </td>

      <td>
        Imágenes asociadas (ninguna disponible).
      </td>

      <td>
        Array
      </td>

      <td>
        \[

        ]
      </td>
    </tr>
  </tbody>
</Table>

### Transacciones

| Campo         | Descripción                                    | Tipo de Dato | Ejemplo                                                                         |
| ------------- | ---------------------------------------------- | ------------ | ------------------------------------------------------------------------------- |
| priority      | Prioridad de la transacción.                   | String       | primary                                                                         |
| kind          | Tipo de transacción (tarjeta).                 | String       | card                                                                            |
| amount        | Monto de la transacción.                       | Number       | 1000                                                                            |
| card\_address | Dirección de la Tarjeta                        | json         | [Dirección de la Tarjeta](#dirección-de-la-tarjeta)                             |
| card          | Información de la Tarjeta                      | json         | [Información de la Tarjeta](#información-de-la-tarjeta)                         |
| card\_info    | Información del Titular de la Tarjeta          | json         | [Información del Titular de la Tarjeta](#información-del-titular-de-la-tarjeta) |
| installment   | Número de cuotas.                              | String       | 1                                                                               |
| monthly       | Pago mensual.                                  | String       | $1,000.00 MXN                                                                   |
| descriptor    | Descripción del comerciante en la transacción. | String       | HOTELPAY                                                                        |

### Dirección de la Tarjeta

| Campo         | Descripción                                 | Tipo de Dato | Ejemplo         |
| ------------- | ------------------------------------------- | ------------ | --------------- |
| address       | Dirección de la tarjeta.                    | String       | Av Hidalgo 1995 |
| city          | Ciudad de la tarjeta.                       | String       | Guadalajara     |
| country       | País de la tarjeta.                         | String       | México          |
| country\_code | Código del país de la tarjeta.              | String       | MX              |
| zip           | Código postal de la tarjeta.                | String       | 123456          |
| address2      | Segunda línea de dirección (no disponible). | Null         | null            |
| state         | Estado de la tarjeta.                       | String       | Jalisco         |

### Información de la Tarjeta

<Table>
  <thead>
    <tr>
      <th>
        Campo
      </th>

      <th>
        Descripción
      </th>

      <th>
        Tipo de Dato
      </th>

      <th>
        Ejemplo
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        bin
      </td>

      <td>
        Bin de la tarjeta.
      </td>

      <td>
        String
      </td>

      <td>
        400000
      </td>
    </tr>

    <tr>
      <td>
        type
      </td>

      <td>
        Tipo de tarjeta (crédito).
      </td>

      <td>
        String
      </td>

      <td>
        credit
      </td>
    </tr>

    <tr>
      <td>
        brand
      </td>

      <td>
        Marca de la tarjeta.
      </td>

      <td>
        String
      </td>

      <td>
        visa
      </td>
    </tr>

    <tr>
      <td>
        bank_name
      </td>

      <td>
        Nombre del banco.
      </td>

      <td>
        String
      </td>

      <td>
        \`

        Intl Hdqtrs-C
      </td>
    </tr>
  </tbody>
</Table>

### Información del Titular de la Tarjeta

| Campo        | Descripción                        | Tipo de Dato | Ejemplo |
| ------------ | ---------------------------------- | ------------ | ------- |
| holder\_name | Nombre del titular de la tarjeta.  | String       | Alex    |
| expiration   | Fecha de expiración de la tarjeta. | String       | 12/2030 |

### Plantilla de la Transacción

| Campo                | Descripción                       | Tipo de Dato | Ejemplo                                                                                                                          |
| -------------------- | --------------------------------- | ------------ | -------------------------------------------------------------------------------------------------------------------------------- |
| BRAND                | Marca de la tarjeta.              | String       | visa                                                                                                                             |
| CARD\_ICON           | Icono de la tarjeta.              | String       | [https://testing.hotelpay.net/storage/img/brands/flats/visa.svg](https://testing.hotelpay.net/storage/img/brands/flats/visa.svg) |
| TYPE                 | Tipo de tarjeta.                  | String       | credit                                                                                                                           |
| LAST\_4\_DIGITS      | Últimos 4 dígitos de la tarjeta.  | String       | 0051                                                                                                                             |
| HOLDER\_NAME         | Nombre del titular de la tarjeta. | String       | Alex                                                                                                                             |
| MERCHANT\_DESCRIPTOR | Descripción del comerciante.      | String       | UNLIMIT                                                                                                                          |
| INSTALLMENT          | Número de cuotas.                 | String       | 1                                                                                                                                |
| MONTHLY              | Pago mensual.                     | String       | $1,000.00 MXN                                                                                                                    |

### Información Adicional

| Campo                | Descripción     | Tipo de Dato | Ejemplo          |
| -------------------- | --------------- | ------------ | ---------------- |
| payment\_adds.date   | Fecha del pago. | String       | 01/07/2024 20:56 |
| payment\_adds.amount | Monto del pago. | String       | $1,000.00 MXN    |

### Estados del pago

| Valor      | Descripción                                                  |
| ---------- | ------------------------------------------------------------ |
| `pending`  | La orden de pago está pendiente de ser procesada.            |
| `process`  | La orden de pago está en proceso de ser completada.          |
| `paid`     | La orden de pago ha sido completada y confirmada.            |
| `canceled` | La orden de pago ha sido cancelada y no se completará.       |
| `error`    | Ha ocurrido un error durante el proceso de la orden de pago. |

Estas Tablas vinculadas permite una presentación estructurada y clara de la información contenida en el JSON, facilitando su comprensión y uso.

## Notas Adicionales

* La estructura del JSON es flexible y puede ser extendida según las necesidades específicas del sistema y los requisitos del negocio.

Este JSON proporciona una base sólida para el intercambio de información entre sistemas de reserva y pago, facilitando la automatización y la eficiencia en el proceso de gestión de reservas y pagos para alojamientos.

## Webhook.site

Webhook.site es una herramienta que te permite generar URLs únicas para recibir y observar peticiones HTTP, incluyendo JSONs enviados por webhooks. Es útil para pruebas y depuración.

#### Pasos para usar Webhook.site

1. **Generar una URL de Webhook**:

   * Ve a [Webhook.site](https://webhook.site/).
   * Se generará automáticamente una URL única para ti.

     <Image alt="Ejemplo de la URL" align="center" src="https://files.readme.io/1fa69c2-image.png">
       Ejemplo de la URL
     </Image>

2. **Configurar la url del Webhook**: &#x20;

   * Ve al backend de hotelpay en el apartado Hoteles.
   * Indica la URL que actuará como el endpoint que recibirá el JSON y de ser necesario indica un token.

     ![](https://files.readme.io/2a66e14-image.png)

3. **Observar los Datos Recibidos**:

   * Regresa a la página de Webhook.site.
   * Verás las solicitudes HTTP recibidas en tiempo real, incluyendo el JSON que se ha enviado.

   <Image alt="Ejemplo de una petición webhook de una reserva" align="center" src="https://files.readme.io/039c16f-image.png">
     Ejemplo de una petición webhook de una reserva
   </Image>

## Pipedream.com

Pipedream es una plataforma de integración que permite automatizar flujos de trabajo conectando diferentes servicios y aplicaciones. Puedes usar Pipedream para recibir webhooks y procesar los datos.

#### Pasos para usar Pipedream.com

1. **Crear una Cuenta y un Nuevo Workflow**:
   * Regístrate o inicia sesión en [Pipedream](https://pipedream.com/).
   * Crea un nuevo Workflow.

2. **Configurar el Trigger del Webhook**:

   * Selecciona "HTTP / Webhook" como trigger.

     <Image alt="Debemos seleccionar new HTTP" align="center" src="https://files.readme.io/9c88cce-image.png">
       Debemos seleccionar new HTTP
     </Image>
   * Pipedream generará una URL única que actuará como tu endpoint para recibir webhooks.

     <Image alt="Copiamos la URL" align="center" src="https://files.readme.io/4c3ec87-image.png">
       Copiamos la URL
     </Image>

3. **Configurar el Emisor del Webhook**:
   * Utiliza la URL generada en el servicio que enviará el webhook.
   * Ve al backend de hotelpay en el apartado Hoteles.
   * Indica la URL que actuará como el endpoint que recibirá el JSON y de ser necesario indica un token.\
     ![](https://files.readme.io/2a66e14-image.png)

4. **Añadir Pasos para Procesar el JSON**:

   * Una vez configurado el trigger, añade un paso para procesar los datos. Puedes usar Node.js, Python o cualquier otro lenguaje soportado por Pipedream, en este caso se utilizara Node js

     <Image alt="Seleccionamos Node para este ejemplo." align="center" src="https://files.readme.io/95b3435-image.png">
       Seleccionamos Node para este ejemplo.
     </Image>

     <Image alt="Seleccionamos la prime opción" align="center" src="https://files.readme.io/319de77-image.png">
       Seleccionamos la primera opción
     </Image>
   * Copiamos el código y lo pegamos dentro del apartado código en Pipedream

   ```javascript
   export default defineComponent({
     async run({ steps, $ }) {
       const data = steps.trigger.event.body;

       console.log('Datos recibidos:', data);

       const { uuid, reference, amount, currency, customer_info, items, transactions } = data;

       console.log(`Reserva UUID: ${uuid}`);
       console.log(`Referencia: ${reference}`);
       console.log(`Monto: ${amount} ${currency}`);
       console.log(`Cliente: ${customer_info.first_name} ${customer_info.last_name}`);
       console.log(`Artículos:`);
       items.forEach(item => {
         console.log(`- Descripción: ${item.description}, Precio: ${item.price}, Cantidad: ${item.quantity}`);
       });
       console.log(`Transacciones:`);
       transactions.forEach(transaction => {
         console.log(`- Monto: ${transaction.amount}, Estado: ${transaction.status}`);
       });

       return data;
     }
   });
   ```

5. **Deploy y Probar**:
   * Guarda y despliega el workflow\..

6. **Observar y Procesar los Datos**:

   * En el dashboard de Pipedream, podrás ver la ejecución de cada workflow, incluyendo los datos recibidos y procesados.

     <Image alt="Ejemplo de cada peticion webhook en un proceso implementado" align="center" src="https://files.readme.io/0cc921b-image.png">
       Ejemplo de cada petición webhook en un proceso implementado
     </Image>
   * Pipedream también te permite integrar fácilmente con otros servicios y realizar acciones adicionales, como enviar correos electrónicos, almacenar datos en bases de datos, etc.

### Consideraciones Adicionales

* **Seguridad**: Asegúrate de que solo fuentes confiables envíen datos a tu webhook. Pipedream permite configurar autenticación y otros mecanismos de seguridad.
* **Errores y Reintentos**: Maneja adecuadamente los errores y considera implementar un sistema de reintento en caso de fallos.
* **Logs y Monitorización**: Tanto Webhook.site como Pipedream proporcionan herramientas para monitorizar y registrar las solicitudes recibidas, lo cual es útil para depuración y análisis.

Usando estas herramientas, puedes recibir, observar y procesar los datos enviados a través del webhook de hotelpay.