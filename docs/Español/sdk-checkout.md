---
title: SDK Checkout
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
# Referencia

```javascript
Hotelpay: {
  Checkout: {
    // Generales
    setApiKey: (key: string) => Promise<void>;
    setCurrency: (currency: "MXN" | "USD") => void;
    setTotal: (total: number) => void;
    setDetails: (data: Details) => void;
    setMode: (mode: "testing" | "production") => void;
    setLanguage: (lang: "es" | "en") => void;

    // Overlay
    onSuccess: (callback: () => void) => void;
    onCancel: (callback: () => void) => void;
    onFail: (callback: () => void) => void;
    pay: async () => Promise<void>;
    render: async (selector: string, onSubmit?: () => void) => Promise<void>;

    // Iframe
    on: (eventName: string, cb: Function) => void;
    binFocus: () => void;
    binBlur: () => void;
    cvvFocus: () => void;
    cvvBlur: () => void;
    continueCheckout: () => void;
    setAddress: (address: Address) => void;
    renderIframe: async (selector: string) => Promise<void>;
  }
}
```

<br />

<br />

# Ambientes

## Pruebas

Ambiente configurado para simular las condiciones de operación de una aplicación o sistema, permitiendo a los desarrolladores y testers verificar la funcionalidad, buscar errores, y asegurar la calidad antes de que el producto se despliegue en el entorno de producción. Este ambiente es crucial para identificar y solucionar problemas sin afectar el funcionamiento normal o la experiencia del usuario final.

```js JavaScript
Hotelpay.Checkout.setMode(Hotelpay.TESTING);
```

## Producción

Es el entorno final en el que una aplicación o sistema de software se ejecuta y es accesible para los usuarios finales. Es el entorno en vivo donde la aplicación se utiliza para realizar operaciones reales y cumple su propósito diseñado. Ya que cualquier error o fallo puede tener impactos directos en los procesos de negocio, la experiencia del usuario y, en algunos casos, puede conllevar riesgos financieros o de seguridad. Por ello, antes de que una aplicación se traslade al ambiente de producción, debe pasar por rigurosas pruebas en testing.

```js JavaScript
Hotelpay.Checkout.setMode(Hotelpay.PRODUCTION);
```

<br />

# Métodos Generales

<br />

## setApiKey

Esté método es utilizado para establecer la clave de API necesaria para autenticar las solicitudes que se hacen desde el SDK hacia el servicio.

```js JavaScript
Hotelpay.Checkout.setApiKey("pk_1237932718");
```

## setDetails

Esté método es utilizado para establecer cuáles son los detalles de la reservación, así como id interno del gestor de reservas, items, datos del cliente y fechas de reservación.

```js JavaScript
Hotelpay.Checkout.setDetails({
  /**
   * Opcional.
   * Enviar identificador interno del motor de reservas
   */
  referenceId: "my-internal-id",

  /* Requerido */
  reserve_data: {
    booking_number: "00001",
    checkin: "2024-12-01",
    checkout: "2024-12-31",
    guests_adults: 2,
    guests_children: 1
  },

  /* Requerido */
  items: [
    {
      description: "Habitación doble vista al mar",
      price: 56.79,
      quantity: 1,
    },
    {
      description: "Habitación sencilla",
      price: 34.99,
      quantity: 1,
    },
  ],

  /* Requerido */
  customer_info: {
    first_name: "Victor",
    last_name: "Alvarez",
    phone: "7777777777",
    email: "victor@hotelpay.com",
  },

  /* Opcional */
  card_holder: {
    name: "Victor Alvarez",
    phone: "7777777777",
    email: "victor@hotelpay.com",
  },
});
```

<br />

## setTotal

Esté método es utilizado para establecer el monto total de la compra, se permite 2 decimales.

> ❗️ Máximo 2 decimales

```js JavaScript
Hotelpay.Checkout.setTotal(444.44);
```

## setMode

Esté método es utilizado para establecer el ambiente en que el Checkout será ejecutado, puede ser ambiente de pruebas o productivo.

```js JavaScript
Hotelpay.Checkout.setMode(Hotelpay.PRODUCTION);

Hotelpay.Checkout.setMode(Hotelpay.TESTING);
```

## setCurrency

Esté método es utilizado para establecer la moneda para transaccionar.

```js JavaScript
Hotelpay.Checkout.setCurrency("MXN");

Hotelpay.Checkout.setCurrency("USD");
```

## setLanguage

Esté método es utilizado para establecer el idioma.<html xmlns="http://www.w3.org/1999/xhtml"><head /><body /></html>

```js JavaScript
Hotelpay.Checkout.setLanguage("es");

Hotelpay.Checkout.setLanguage("en");
```

# Overlay

## onSuccess

Callback que se ejecuta cuando la transacción ha sido efectuada exitosamente.

```js JavaScript
Hotelpay.Checkout.onSuccess(() => {
  console.log("Pagó exitoso");
});
```

## onCancel

Callback que se ejecuta cuando la transacción es cancelada.<html xmlns="http://www.w3.org/1999/xhtml"><head /><body /></html>

```js JavaScript
Hotelpay.Checkout.onCancel(() => {
  console.log("Checkout cancelado");
});
```

## onFail

Callback que se ejecuta cuando la transacción falla.

```js JavaScript
Hotelpay.Checkout.onFail(() => {
  console.log("Pagó no satisfactorio");
});
```

## pay

Método para abrir el overlay checkout.

```js JavaScript
Hotelpay.Checkout.pay();
```

## render

Método para renderizar el botón para pagar.

Este botón

```js JavaScript
const onSubmit = () => {
  /**
   * añadir comportamiento personalizado
   **/

  // Abrir overlay checkout
  Hotelpay.Checkout.pay();
};

Hotelpay.Checkout.render("#container", onSubmit);
```

# Iframe

## renderIframe

Método para renderizar el formulario versión iframe.

```js JavaScript
Hotelpay.Checkout.renderIframe("#container");
```

## setAddress

Método para enviar la información necesaria para la transacción, dirección y correo del usuario

```js JavaScript
const user = {
  email: "unknown@hotelpay.com",
};
const address = {
  country: "México",
  state: "Jalisco",
  city: "Guadalajara",
  street: "Unknown",
  zip: "45000",
};
Hotelpay.Checkout.setAddress(user, address);
```

## on(eventName)

Método para escuchar los eventos que suceden dentro del iframe.

```js Javascript
Hotelpay.Checkout.on("complete-form-card", (e) => console.log(e));

Hotelpay.Checkout.on("adding-card-start", (e) => console.log(e));

Hotelpay.Checkout.on("adding-card-end", (e) => console.log(e));

Hotelpay.Checkout.on("msi", (e) => console.log(e));

Hotelpay.Checkout.on("3ds-start", (e) => console.log(e));

Hotelpay.Checkout.on("3ds-end", (e) => console.log(e));

Hotelpay.Checkout.on("transaction-completed", (e) => console.log(e));

Hotelpay.Checkout.on("transaction-failed", (e) => console.log(e));

/**
 * Solo pago dividido
 **/

Hotelpay.Checkout.on("toggle-split", (e) => console.log(e));

Hotelpay.Checkout.on("adding-card-2-start", (e) => console.log(e));

Hotelpay.Checkout.on("adding-card-2-end", (e) => console.log(e));
```

Eventos:

* **complete-form-card**: esté evento se ejecuta cuando cambia el estatus entre valido e invalido de los datos de la tarjeta

Al iniciar el formulario se ejecuta como `is_valid = false` cuando se capturan todos los datos correctamente se ejecuta como `is_valid = true`. Si en algun momento los datos vuelven a cambiar es posible recibir el evento `is_valid = false` indicando que un dato fue modificado y debe ser corregido

```json
{
  is_valid: true | false
}
```

* **adding-card-start**: esté evento se ejecuta despues de `Hotelpay.Checkout.continueCheckout()` indique que el proceso de agregar tarjeta ha comenzado

* **adding-card-end**: esté evento se ejecuta despues que el proceso agregar tarjeta ha finalizado y devuelve como información si la tarjeta se puedo agregar correctamente

```json
{
  is_valid: true | false
}
```

* **msi**: esté evento se ejecuta despues de `adding-card-end` si la tarjeta fue agregada correctamente develve como información los plazos disponibles y los montos minimos para poder aplicarse

```json
{
  available: true | false,
  installments: {
    months: 3,
    minimum: 1000
  }[]
}
```

* **3ds-start**: esté evento se ejecuta cuando es necesario ejecutar un challenge de 3ds

* **3ds-end**: esté evento se ejecuta cuando termina el challenge de 3ds

* **transaction-completed**: esté evento se ejecuta cuando la transacción ha finalizado con resultado positivo

* **transaction-failed**: esté evento se ejecuta cuando la transacción ha finalizado con resultado negativo

Pago dividido

* **toggle-split**: esté evento se ejecuta cuando el usuario cambia de modo pago en un solo método o pago dividido

```json
{
  is_split: true | false
}
```

* **adding-card-2-start**: esté event se ejecuta despues de `Hotelpay.Checkout.continueCheckout()` indique que el proceso de agregar tarjeta 2 ha comenzado

* **adding-card-2-end**: esté event se ejecuta despues que el proceso agregar tarjeta ha finalizado y devuelve como información si la tarjeta 2 se puedo agregar correctamente

```json
{
  is_valid: true | false
}
```

## continueCheckout

Método para ejecutar la transacción despues de haber llenado la información del usuario

```js JavaScript
Hotelpay.Checkout.continueCheckout();
```