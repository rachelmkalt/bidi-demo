---
title: Integrar Link de Pago (v1)
excerpt: >-
  Los links de pago ofrecidos por Hotelpay son seguros, sencillos y certificados
  por PCI.
deprecated: true
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
Los links de pago son una forma segura y sencilla de que los huéspedes completen su pago sin necesidad de exponer los datos de su tarjeta. Hotelpay te ofrece esta solución certificada por PCI para que puedas brindar una experiencia de pago óptima a tus clientes.

## ¿Cómo funcionan los links de pago?

### Cuentas activas en Hotelpay:

Para ofrecer links de pago, es necesario tener cuentas activas en la plataforma de Hotelpay. Cada hotel creado tendrá una URL única, como por ejemplo:

[https://link-de-pago.hotelpay.net/hotel/obsidiana](https://link-de-pago.hotelpay.net/hotel/obsidiana)

### Creación de múltiples links de pago

Si necesitas crear varios links de pago para un mismo proceso, puedes hacerlo mediante un archivo CSV. Este archivo debe contener la siguiente información:

[Link de Pago Partner - Hoteles.csv](Links%20de%20Pago%20bb9f300c9ca54269888b7e924044e22f/Link_de_Pago_Partner_-_Hoteles.csv)

<Table>
  <thead>
    <tr>
      <th>
        Hotel
      </th>

      <th>
        Slug
      </th>

      <th>
        Logo
      </th>

      <th>
        Color Fondo
      </th>

      <th>
        Color Texto
      </th>

      <th>
        Dirección
      </th>

      <th>
        Teléfono
      </th>

      <th>
        Aviso de Privacidad
      </th>

      <th>
        Términos y Condiciones
      </th>

      <th>
        Webhook
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        Hotel Obsidiana
      </td>

      <td>
        hotel-obsidiana
      </td>

      <td>
        https

        \:

        //url/logo
      </td>

      <td>
        gray
      </td>

      <td>
        white
      </td>

      <td>
        Av Obsidiana
      </td>

      <td>
        <p />

        ¿Necesitas ayuda?

        <html xmlns="http://www.w3.org/1999/xhtml"><head /><body /></html>

        <p />

        ‍

        <html xmlns="http://www.w3.org/1999/xhtml"><head /><body /></html>

        <p />

        Llámanos al +52 33 11224455

        <html xmlns="http://www.w3.org/1999/xhtml"><head /><body /></html>
      </td>

      <td>
        https

        \:

        //url/aviso-de-privacidad
      </td>

      <td>
        https

        \:

        //url/terminos-y-condiciones
      </td>

      <td>
        https

        \:

        //url/webhook
      </td>
    </tr>
  </tbody>
</Table>

## Pantalla de link de pago:

Los links de pago creados son inicialmente "pantallas vacías" que solo muestran la información del hotel.

* **Hotel:** Nombre del hotel asociado al link de pago, logo, dirección
* **Vínculos:** A el aviso de privacidad y a los términos y condiciones
* **Botón de pago:** Botón que acciona el proceso del pago

<Image align="center" className="border" width="360px" src="https://files.readme.io/8d4aff9-Untitled.png" />

## Envío de información de pago al link de pago

### Objeto JSON:

Para enviar la información del pago al link de pago, se debe generar un objeto JSON serializado con la siguiente estructura:

```jsx JSON
{
    checkin: "2024-10-01",
    checkout: "2024-10-05",
    guests: 2,
    rooms: 1,
    subtotal: 1000,
    taxes: 100,
    discounts: 50,
    total: 1050,
    currency: 'MXN'
}
```

### Serialización del objeto JSON:

El objeto JSON se serializa utilizando la función `JSON.stringify()` y la función `btoa()` para codificarlo en Base64. Esto genera una cadena como la siguiente:

```jsx Text
eyJjaGVja2luIjoiMjAyMi0xMC0wMSIsImNoZWNrb3V0IjoiMjAyMi0xMC0wNSIsImd1ZXN0cyI6Miwicm9vbXMiOjEsInN1YnRvdGFsIjoxMDAwLCJ0YXhlcyI6MTAwLCJkaXNjb3VudHMiOjUwLCJ0b3RhbCI6MTA1MH0
```

### Integración en la URL del link de pago:

La cadena codificada se agrega a la URL del link de pago como parámetro `params`. La URL final tendrá la siguiente forma:

```jsx Text
https://link-de-pago.hotelpay.net/hotel/viaggio-resort-mazatlan?params=eyJjaGVja2luIjoiMjAyMi0xMC0wMSIsImNoZWNrb3V0IjoiMjAyMi0xMC0wNSIsImd1ZXN0cyI6Miwicm9vbXMiOjEsInN1YnRvdGFsIjoxMDAwLCJ0YXhlcyI6MTAwLCJkaXNjb3VudHMiOjUwLCJ0b3RhbCI6MTA1MH0
```

### Link de pago funcional:

Al acceder a la URL completa, se genera un link de pago funcional que el huésped puede utilizar para completar la transacción. El link de pago mostrará la información del hotel, la descripción del pago y el importe total a pagar.

<Image align="center" className="border" width="360px" src="https://files.readme.io/d1f0ec2-Untitled_1.png" />

## Notificación de cobro autorizado (Webhook)

Una vez finalizado el flujo de pago con un cobro autorizado, la plataforma de Hotelpay accionará un webhook. Este webhook es una notificación automática que se envía a una URL específica configurada por el hotel.

Es importante que cada hotel tenga un webhook único. Esto garantiza que la información de la transacción se envíe a la URL correcta.

### Configuración del webhook:

La configuración del webhook se realiza en la plataforma de Hotelpay. Para ello, se debe proporcionar la URL donde se desea recibir la información de la transacción.

## Soporte:

Si tiene dudas sobre la configuración o el funcionamiento del webhook, puede solicitar apoyo al equipo de soporte de Hotelpay a través del correo electrónico [soporte@hotelpay.com](mailto:soporte@hotelpay.com).

Esperamos que esta información te sea útil.