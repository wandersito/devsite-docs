----[mlb]----
# Integra Checkout Transparente para pagos con tarjetas
------------
----[mla, mlm, mpe, mco, mlu, mlc]----
# Integra Checkout API para pagos con tarjetas
------------

[TXTSNIPPET][/guides/snippets/test-integration/receiving-payment-by-card]

## ¿Cómo funciona?

![API-integration-flowchart](/images/api/api-integration-flowchart-cardform-es.png)

> Si quieres realizar un flujo personalizado del pago, te compartimos todos los [métodos disponibles para integrar de forma avanzada](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/es/guides/online-payments/checkout-api/receiving-payment-by-card-core-methods-v2).

----[mlb]----
Al usar nuestro Checkout Transparente de Mercado Pago, es importante tener en cuenta dos instancias: la de la captura de datos y la del envío de confirmación del pago.
------------
----[mla, mlm, mpe, mco, mlu, mlc]----
Al usar nuestro Checkout API de Mercado Pago, es importante tener en cuenta dos instancias: la de la captura de datos y la del envío de confirmación del pago.
------------

1. Primero, necesitas un frontend para que recolecte los datos de la tarjeta y que genere un token de seguridad con la información para poder crear el pago.
2. Segundo, un backend que tome el token generado y los datos del pago, como por ejemplo monto e ítem, pueda confirmar y efectuar el pago.

Tanto para el frontend como para el backend, recomendamos utilizar [nuestras librerías](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/es/guides/online-payments/checkout-api/previous-requirements#bookmark_utiliza_nuestras_librerías_siempre) para poder recolectar los datos sensibles de tus usuarios de manera segura.

> NOTE
>
> Nota
>
> Puedes obtener más información en las [Referencias de API](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/es/reference/cards/_customers_customer_id_cards/post).

<br>

> CLIENT_SIDE
>
> h2
>
> Captura los datos de la tarjeta

[TXTSNIPPET][/guides/snippets/test-integration/secure-fields-card]

<br>
<span></span>

> SERVER_SIDE
>
> h2
>
> Envía el pago a Mercado Pago

Para continuar el proceso de pago hacia Mercado Pago, es necesario que tu backend sepa recibir la información del formulario con el token generado y los datos completados.

Según el ejemplo dado, tu backend debería disponibilizar un endpoint `/process_payment`, para recibir allí todos los datos luego de realizar la acción submit.

Ya estando en tu backend con toda la información recolectada, es momento de enviar la solicitud a Mercado Pago a través de nuestras APIs. Los campos mínimos requeridos a enviar son: `token`, `transaction_amount`, `installments`, `payment_method_id` y el `payer.email`.

Ten en cuenta que para que este paso funcione es necesario que configures tu [clave privada]([FAKER][CREDENTIALS][URL]) y que para interactuar con nuestras APIs recomendamos utilizar la [SDK oficial de Mercado Pago](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/es/guides/online-payments/checkout-api/previous-requirements#bookmark__instala_la_sdk_de_mercado_pago).

[[[
```php
===
Encuentra el estado del pago en el campo _status_.
===
<?php
   require_once 'vendor/autoload.php';
 
   MercadoPago\SDK::setAccessToken("YOUR_ACCESS_TOKEN");
 
   $payment = new MercadoPago\Payment();
   $payment->transaction_amount = (float)$_POST['transactionAmount'];
   $payment->token = $_POST['token'];
   $payment->description = $_POST['description'];
   $payment->installments = (int)$_POST['installments'];
   $payment->payment_method_id = $_POST['paymentMethodId'];
   $payment->issuer_id = (int)$_POST['issuer'];
 
   $payer = new MercadoPago\Payer();
   $payer->email = $_POST['email'];
   $payer->identification = array(----[mla, mlb, mlu, mlc, mpe, mco]----
       "type" => $_POST['identificationType'],------------
       "number" => $_POST['identificationNumber']
   );
   $payment->payer = $payer;
 
   $payment->save();
 
   $response = array(
       'status' => $payment->status,
       'status_detail' => $payment->status_detail,
       'id' => $payment->id
   );
   echo json_encode($response);
 
?>
```
```node
===
Encuentra el estado del pago en el campo _status_.
===
 
var mercadopago = require('mercadopago');
mercadopago.configurations.setAccessToken("YOUR_ACCESS_TOKEN");
 
mercadopago.payment.save(req.body)
  .then(function(response) {
    const { status, status_detail, id } = response.body;
    res.status(response.status).json({ status, status_detail, id });
  })
  .catch(function(error) {
    console.error(error);
  });
```
```java
===
Encuentra el estado del pago en el campo _status_.
===
 
MercadoPago.SDK.setAccessToken("YOUR_ACCESS_TOKEN");
 
Payment payment = new Payment();
payment.setTransactionAmount(Float.valueOf(request.getParameter("transactionAmount")))
      .setToken(request.getParameter("token"))
      .setDescription(request.getParameter("description"))
      .setInstallments(Integer.valueOf(request.getParameter("installments")))
      .setPaymentMethodId(request.getParameter("paymentMethodId"));
 
Identification identification = new Identification();----[mla, mlb, mlu, mlc, mpe, mco]----
identification.setType(request.getParameter("identificationType"))
             .setNumber(request.getParameter("identificationNumber"));------------ ----[mlm]----
identification.setNumber(request.getParameter("identificationNumber"));------------
 
Payer payer = new Payer();
payer.setEmail(request.getParameter("email"))
    .setIdentification(identification);
   
payment.setPayer(payer);
 
payment.save();
 
System.out.println(payment.getStatus());
 
```
```ruby
===
Encuentra el estado del pago en el campo _status_.
===
require 'mercadopago'
sdk = Mercadopago::SDK.new('YOUR_ACCESS_TOKEN')
 
payment_data = {
 transaction_amount: params[:transactionAmount].to_f,
 token: params[:token],
 description: params[:description],
 installments: params[:installments].to_i,
 payment_method_id: params[:paymentMethodId],
 payer: {
   email: params[:email],
   identification: {----[mla, mlb, mlu, mlc, mpe, mco]----
     type: params[:identificationType],------------
     number: params[:identificationNumber]
   }
 }
}
 
payment_response = sdk.payment.create(payment_data)
payment = payment_response[:response]
 
puts payment
 
```
```csharp
===
Encuentra el estado del pago en el campo _status_.
===
using System;
using MercadoPago.Client.Common;
using MercadoPago.Client.Payment;
using MercadoPago.Config;
using MercadoPago.Resource.Payment;
 
MercadoPagoConfig.AccessToken = "YOUR_ACCESS_TOKEN";
 
var paymentRequest = new PaymentCreateRequest
{
   TransactionAmount = decimal.Parse(Request["transactionAmount"]),
   Token = Request["token"],
   Description = Request["description"],
   Installments = int.Parse(Request["installments"]),
   PaymentMethodId = Request["paymentMethodId"],
   Payer = new PaymentPayerRequest
   {
       Email = Request["email"],
       Identification = new IdentificationRequest
       {----[mla, mlb, mlu, mlc, mpe, mco]----
           Type = Request["identificationType"],------------
           Number = Request["identificationNumber"],
       },
   },
};
 
var client = new PaymentClient();
Payment payment = await client.CreateAsync(paymentRequest);
 
Console.WriteLine(payment.Status);
 
```
```python
===
Encuentra el estado del pago en el campo _status_.
===
import mercadopago
sdk = mercadopago.SDK("ACCESS_TOKEN")
 
payment_data = {
   "transaction_amount": float(request.POST.get("transaction_amount")),
   "token": request.POST.get("token"),
   "description": request.POST.get("description"),
   "installments": int(request.POST.get("installments")),
   "payment_method_id": request.POST.get("payment_method_id"),
   "payer": {
       "email": request.POST.get("email"),
       "identification": {----[mla, mlb, mlu, mlc, mpe, mco]----
           "type": request.POST.get("type"), ------------
           "number": request.POST.get("number")
       }
   }
}
 
payment_response = sdk.payment().create(payment_data)
payment = payment_response["response"]
 
print(payment)
```
```curl
===
Encuentra el estado del pago en el campo _status_.
===
 
curl -X POST \
   -H 'accept: application/json' \
   -H 'content-type: application/json' \
   -H 'Authorization: Bearer YOUR_ACCESS_TOKEN' \
   'https://api.mercadopago.com/v1/payments' \
   -d '{
         "transaction_amount": 100,
         "token": "ff8080814c11e237014c1ff593b57b4d",
         "description": "Blue shirt",
         "installments": 1,
         "payment_method_id": "visa",
         "issuer_id": 310,
         "payer": {
           "email": "test@test.com"
         }
   }'
 
```
]]]

#### Respuesta

```json
{
   "status": "approved",
   "status_detail": "accredited",
   "id": 3055677,
   "date_approved": "2019-02-23T00:01:10.000-04:00",
   "payer": {
       ...
   },
   "payment_method_id": "visa",
   "payment_type_id": "credit_card",
   "refunds": [],
   ...
}
```

> Conoce todos los campos disponibles para realizar un pago completo en las [Referencias de API](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/es/reference/payments/_payments/post).

## Mensajes de respuestas

[TXTSNIPPET][/guides/snippets/test-integration/api-response-handling]

## Recibe notificaciones de pago

[TXTSNIPPET][/guides/snippets/test-integration/api-payment-notifications]

## Ejemplos descargables

----[mlb]----
> GIT
>
> Checkout Transparente
>
> Te dejamos [ejemplos completos de integración](https://github.com/mercadopago/card-payment-sample/tree/feature/secure-fields) para que puedas descargar al instante.
------------
----[mla, mlm, mpe, mco, mlu, mlc]----
> GIT
>
> Checkout API
>
> Te dejamos [ejemplos completos de integración](https://github.com/mercadopago/card-payment-sample/tree/feature/secure-fields) para que puedas descargar al instante.
------------

<span></span>

> GIT
>
> Formulario de pago
>
> Si quieres implementar tu servidor con alguna otra tecnología, te dejamos un [ejemplo completo del formulario de pago](https://drive.google.com/file/d/12gSCPLfZCE36iKFbM4BTUwf6lnM7lVEL/view?usp=sharing) en GitHub para que puedas descargar.

---
### Próximos pasos

> LEFT_BUTTON_REQUIRED_ES
>
> Prueba tu integración
>
> Revisa que esté todo bien en tu integración con los usuarios de prueba.
>
> [Prueba tu integración](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/es/guides/online-payments/checkout-api/testing)

----[mlc]----
> RIGHT_BUTTON_RECOMMENDED_ES
>
> Referencias de API
>
> Encuentra toda la información necesaria para interactuar con nuestras APIs.
>
> [Referencias de API](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/es/reference)
------------

----[mla, mlb, mlm, mlu, mpe, mco]----
> RIGHT_BUTTON_RECOMMENDED_ES
>
> Integra otros medios de pago
>
> Conoce todas las opciones de pago disponibles y cómo ofrecerlas.
>
> [Integra otros medios de pago](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/es/guides/online-payments/checkout-api/other-payment-ways)
------------
