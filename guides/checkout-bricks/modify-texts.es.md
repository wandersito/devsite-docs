> CLIENT_SIDE
>
> h1
>
> Modificar textos

| Brick  | Card Payment Brick  |
| --- | --- |
| Momento de personalización  | Al renderizar el brick  |
| Propiedad  | customization.visual.texts.{cardNumber, cardExpirationDate, cardSecurityCode, cardholderName, cardholderIdentification, cardholderEmail, formTitle, emailSectionTitle, installmentsSectionTitle, selectInstallments, formSubmit}  |
| Atributo  | label, placeholder  |
| Tipo  | String  |
| Observaciones  | Al enviar texto vacío, la pantalla presentará el texto definido por el layout predeterminado. Por otro lado, al enviar un texto personalizado, reemplazará el texto predeterminado. Para comprobar cuáles son los textos por defecto, consulta la sección Layout del brick deseado.  Si los textos personalizados son más grandes que el espacio disponible, el texto mostrado se interrumpirá hasta el tamaño máximo permitido y el excedente será reemplazado por el símbolo "...".  |

```javascript
const settings = {
    ...,
    customization: {
        visual: {
            texts: {
                formTitle: 'string',
                installmentsSectionTitle: 'string',
                ...,
            },
        }
    },
}
```

> PREV_STEP_CARD_ES
>
> Ocultar títulos y banderas
>
> Además, si lo deseas, puedes ocultar los títulos de la UI y las banderas admitidas en Card Payment Brick.
>
> [Ocultar títulos y banderas](/developers/es/docs/checkout-bricks/additional-customization/hide-title-and-flags)

> NEXT_STEP_CARD_ES
>
> Seleccionar idioma
>
> Si lo deseas, puedes seleccionar el idioma de Card Payment Brick. Aprende cómo.
>
> [Selecionar idioma](/developers/es/docs/checkout-bricks/additional-customization/select-language)