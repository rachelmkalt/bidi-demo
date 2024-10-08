---
title: Integrar Link de Pago
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
El sistema permite generar enlaces de pago personalizados que incluyen información específica de la reserva. Estos enlaces pueden ser utilizados para redirigir a los usuarios a una página de pago preconfigurada.

## Diagrama de flujo del proceso de pago

El siguiente diagrama ilustra el flujo completo del proceso de pago, desde la generación del enlace hasta la confirmación de la reserva:

<Image align="center" className="border" src="https://mermaid.ink/svg/pako:eNqNlE1u2zAQha8y4CoBUnSvRYAgLWoDUiCo0E6bCTWS2VCkQlJp0yCH6bKLrnoEX6xD_diJbRj1yiY_vpn3OOaLkLYmkQhPjwMZSZ8Utg67ygB_UAbroAT0UPoBnbLTeo8uKKl6NAGyIm5nNpI1QUGe3BP6YzDNI5gq8xC5HFsLHyHf_mqVwWXl-FQ-nsqdleSxnmpE8kSBVSRXNpCuzLRbfri-zooE1oYZBDf1Nu1lBW-meQJfyJBDIKNRUpTvd42MTJnE8r11UllzmitnqRspiTcQNEK_d7YH03wSzAbygYs21nWDjsGeFlyblptmEQzWn5DKmflsnrZ_FoTz4NJTWp0iE2Y6X-g5SSA9asGFVwaUCRyBlGr713AVDcN025fzGOgw3Rf9UFxkltzJxkbvbFCNkgjb35HZE4vjgmrlVBvDKYs0Wjkkl8tKNbUI7wI8KbraWf9O9xtrH-BCWtMo1-FiZLR4uT833mdM4dYaP-iAcJOvp8xqCqg1-dG-tPfu0GVs7e0cnDsw1okHbueG4Bka9qLVz3E23g0iaT__HRoWVPXZeCPyX-kegGfCPSB3vWdo6Ns4KCMR0aPWTS2uREfsUNX8iLzE5UqEDXVUiYS_1tQgB12JyrwyikOwX5-NFElwA10JZ4d2IxIu4PnX0PMQLy_QbpWt8duSTc_U-Fq9_gOEeoY0" />

### Explicación del diagrama

1. **Inicio de la reserva**: El usuario inicia el proceso de reserva en el Motor de Reservas.
2. **Generación del enlace de pago**: El Motor de Reservas genera un enlace de pago único y lo proporciona al usuario.
3. **Acceso a la página de pago**: El usuario accede a la página de pago a través del enlace proporcionado.
4. **Ingreso de datos de pago**: El usuario ingresa sus datos de pago en la página.
5. **Procesamiento del pago**: Los datos se envían al Procesador de Pagos para su procesamiento.
6. **Resultado del pago**:
   * Si el pago es exitoso:\
     a. El usuario es redirigido a la URL de éxito.\
     b. Se envía un webhook al hotel para confirmar el pago.\
     c. El Motor de Reservas consulta la API del Procesador de Pagos para verificar los detalles del cobro.\
     d. El Motor de Reservas confirma y finaliza la reserva.
   * Si el pago falla:\
     a. El usuario es redirigido a la URL de fallo.\
     b. El Motor de Reservas maneja el fallo de la reserva.

Este flujo asegura que el pago se procese de manera segura y que la reserva se confirme solo después de una verificación completa del pago.

## Estructura del enlace

El enlace de pago tiene la siguiente estructura base:

```
https://{dominio}/hotel/{slug}?params={parametros_codificados}
```

Donde:

* `{dominio}` es el dominio del servicio de pago (ver la sección "Entornos" a continuación).
* `{slug}` es el identificador único del hotel.
* `{parametros_codificados}` son los parámetros de la reserva codificados en Base64.

## Entornos

El sistema proporciona dos entornos para la generación y procesamiento de enlaces de pago:

1. **Entorno de Producción**
   * URL base: `https://link-de-pago.hotelpay.net`
   * Este es el entorno para transacciones reales y debe ser utilizado en sistemas en producción.

2. **Entorno Sandbox (Pruebas)**
   * URL base: `https://link-de-pago-partner.webflow.io`
   * Este entorno es para pruebas y desarrollo. Utilícelo para probar su integración sin procesar pagos reales.

Asegúrese de usar el entorno apropiado según sus necesidades de desarrollo o producción.

## Parámetros requeridos

Los siguientes parámetros pueden ser incluidos en el enlace:

| Parámetro       | Tipo    | Descripción                                               |
| --------------- | ------- | --------------------------------------------------------- |
| referenceId     | string  | Identificador de referencia de la reserva                 |
| booking\_number | string  | Número de reserva                                         |
| checkin         | string  | Fecha de entrada (formato: YYYY-MM-DD)                    |
| checkout        | string  | Fecha de salida (formato: YYYY-MM-DD)                     |
| guests          | integer | Número de huéspedes                                       |
| rooms           | integer | Número de habitaciones                                    |
| total           | number  | Monto total de la reserva (calculado si no se ingresa)    |
| subtotal        | number  | Subtotal de la reserva (ignorado si se proporciona total) |
| taxes           | number  | Impuestos aplicables (ignorado si se proporciona total)   |
| discounts       | number  | Descuentos aplicados (ignorado si se proporciona total)   |
| currency        | string  | Código de moneda (ej. "MXN")                              |
| url\_success    | string  | URL de redirección en caso de pago exitoso                |
| url\_failure    | string  | URL de redirección en caso de fallo en el pago            |

### Notas importantes sobre los parámetros:

1. **Total vs. Subtotal, Taxes, y Discounts**:
   * Si se proporciona el parámetro `total`, los parámetros `subtotal`, `taxes`, y `discounts` serán ignorados.
   * Si no se proporciona `total`, se calculará automáticamente sumando `subtotal` y `taxes`, y restando `discounts`.
   * Es recomendable proporcionar o bien `total`, o bien el conjunto de `subtotal`, `taxes`, y `discounts`.

2. **Valores por defecto**:
   * Todos los parámetros numéricos (`guests`, `rooms`, `total`, `subtotal`, `taxes`, `discounts`) tienen 0 como valor por defecto.
   * El parámetro `currency` tiene "MXN" como valor por defecto.

3. **URLs de éxito y fallo**:
   * Si no se proporcionan `url_success` o `url_failure`, el link de pago no realizará redirecciones después del proceso de pago.
   * En su lugar, mostrará un mensaje de pago exitoso o fallido directamente en la página de pago.

4. **referenceId y booking\_number**:
   * `referenceId`: Este parámetro se utiliza para proporcionar un identificador único de la reserva. Este valor se devolverá en el webhook como el campo `reference`.
   * `booking_number`: Este parámetro se utiliza para proporcionar el número de reserva. Este valor se devolverá en el webhook como el campo `booking_number`.
   * Estos parámetros son opcionales pero altamente recomendados para facilitar el seguimiento y la reconciliación de las transacciones.

## Proceso de generación del enlace

1. Crear un objeto JSON con los parámetros requeridos.
2. Convertir el objeto JSON a una cadena.
3. Codificar la cadena JSON en Base64.
4. Codificar el resultado Base64 para URL (reemplazando caracteres no seguros).
5. Construir la URL final con el slug del hotel y los parámetros codificados.

## Ejemplo de implementación

```python
import json
import base64
import urllib.parse

def generate_payment_link(hotel_slug, reservation_data):
    # 1. Crear diccionario con los parámetros
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

    # 2 y 3. Convertir a JSON y codificar en Base64
    params_json = json.dumps(params)
    params_base64 = base64.b64encode(params_json.encode('utf-8')).decode('utf-8')

    # 4. Codificar para URL
    params_encoded = urllib.parse.quote(params_base64)

    # 5. Construir la URL final
    base_url = "https://link-de-pago.hotelpay.net"
    payment_link = f"{base_url}/hotel/{hotel_slug}?params={params_encoded}"

    return payment_link

# Ejemplo de uso
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
  // 1. Crear objeto JSON
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

  // 2 y 3. Convertir a JSON y codificar en Base64
  const paramsBase64 = btoa(JSON.stringify(params));

  // 4. Codificar para URL
  const paramsEncoded = encodeURIComponent(paramsBase64);

  // 5. Construir la URL final
  const baseUrl = "https://link-de-pago.hotelpay.net";
  const paymentLink = `${baseUrl}/hotel/${hotelSlug}?params=${paramsEncoded}`;

  return paymentLink;
}

// Ejemplo de uso
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

## Proceso de confirmación del pago

Es importante destacar que el motor de reservas no debe confirmar el pago basándose únicamente en la redirección del usuario a la URL de éxito. El proceso correcto de confirmación del pago es el siguiente:

1. **Redirección del usuario**: Cuando el pago es exitoso, el usuario es redirigido a la URL de éxito proporcionada. Esta redirección es principalmente para la experiencia del usuario y no debe considerarse como una confirmación definitiva del pago.

2. **Recepción del webhook**: El sistema de pago enviará un webhook al hotel (si está configurado) con la confirmación del pago. Este webhook contiene información detallada y confiable sobre el estado del pago.

3. **Confirmación de la reserva**: Solo después de recibir el webhook el motor de reservas debe confirmar y finalizar la reserva.

### Manejo de fallos

En caso de que el pago falle, el usuario será redirigido a la URL de fallo. El motor de reservas debe estar preparado para manejar esta situación, posiblemente ofreciendo al usuario la opción de intentar el pago nuevamente o cancelar la reserva.

### Consideraciones de seguridad

* No confíe únicamente en la redirección a la URL de éxito para confirmar un pago.
* Implemente la verificación del webhook.
* Asegúrese de que todas las comunicaciones se realicen a través de conexiones seguras (HTTPS).
* Valide la autenticidad de los webhooks recibidos para prevenir fraudes.

## Webhooks

Cuando se configura, el sistema enviará un webhook con información detallada sobre la transacción una vez que se complete el pago. Esta información es crucial para actualizar el estado de la reserva en su sistema.

### Configuración de pruebas

Para propósitos de prueba y desarrollo, se recomienda utilizar el servicio webhook.site. Este servicio proporciona una URL única a la que se pueden enviar los webhooks, permitiéndole ver y analizar las respuestas en tiempo real.

### Estructura de la respuesta del webhook

A continuación se muestra un ejemplo de la respuesta del webhook, con datos sensibles enmascarados:

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

### Campos importantes

* `uuid`: Identificador único de la transacción.
* `reference`: Corresponde al `referenceId` proporcionado en los parámetros del enlace de pago.
* `booking_number`: Corresponde al `booking_number` proporcionado en los parámetros del enlace de pago.
* `amount`: Monto total de la transacción.
* `currency`: Moneda de la transacción.
* `checkin` y `checkout`: Fechas de entrada y salida.
* `payment_status`: Estado del pago (en este caso, "paid").
* `transactions`: Detalles de la transacción, incluyendo información de la tarjeta utilizada.

## Notas adicionales

* Todos los valores de los parámetros deben ser pasados como cadenas (strings).
* Las fechas deben estar en formato "YYYY-MM-DD".
* El código de moneda debe ser un string válido (por ejemplo, "MXN" para pesos mexicanos).
* Las URL de éxito y fallo deben ser URLs válidas y completas, incluyendo el protocolo (http\:// o https\://).
* Es responsabilidad del desarrollador asegurar que los cálculos de subtotal, impuestos, descuentos y total sean correctos antes de generar el enlace.

## Seguridad

* No incluya información sensible adicional en los parámetros, ya que estos pueden ser decodificados.
* Considere implementar medidas de seguridad adicionales, como firmas digitales o tokens de un solo uso, para prevenir la manipulación de los enlaces.
* Asegúrese de que las URL de éxito y fallo (url\_success y url\_failure) sean URLs válidas y seguras dentro de su dominio.

## Soporte:

Si tiene dudas sobre la configuración o el funcionamiento del webhook, puede solicitar apoyo al equipo de soporte de Hotelpay a través del correo electrónico [soporte@hotelpay.com](mailto:soporte@hotelpay.com).

Esperamos que esta información te sea útil.