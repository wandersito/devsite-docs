# Configuración de integración

Para configurar la integración con Mercado Pago Delivery, sigue los pasos a continuación.

1. Crea la aplicación a través del Dashboard. Consulta [Tus aplicaciones](/developers/es/guides/additional-content/dashboard/applications) para obtener instrucciones.
2. Realiza el proceso de autorización vía [OAuth](/developers/es/guides/additional-content/security/oauth/introduction) con los restaurantes para generar el `access-token` necesario para realizar operaciones y no dejar de recibir notificaciones sobre nuevos pedidos a través de Webhooks.
3. Configura las notificaciones seleccionando la opción "Delivery". Consulta [Webhooks](/developers/es/guides/additional-content/notifications/webhooks/webhooks) para obtener instrucciones.
4. Utiliza las API disponibles para consultar y gestionar información de tus sucursales. Ver [Administración de sucursales](/developers/es/docs/mp-delivery/store-management).
5. Utiliza las API disponibles para gestionar las órdenes. Ver [Administración de órdenes](/developers/es/docs/mp-delivery/order-management).

Como buena práctica, siempre es necesario consultar el status de las requests que se han realizado en nuestras APIs, para los casos en que se produzca algún tipo de intermitencia. Esta medida es necesaria principalmente en los casos en que se realicen requests que cambien el status de una order o tienda. Como en el ejemplo de una acción para aceptar una orden, se recomienda realizar reintentos posteriores, si la request aún no ha arrojado un status positivo (200), esta orden debe cancelarse. Lo importante es mantener siempre sincronizados los status, ya sea de una order o de una tienda, entre Mercado Pago y PDV, por lo que siempre es importante implementar soluciones en caso de que se presenten errores.

> PREV_STEP_CARD_ES
>
> Requisitos prévios
>
> Conoce los requisitos previos para llevar a cabo la integración.
>
> [Requisitos previos](/developers/es/docs/mp-delivery/requirements)

> NEXT_STEP_CARD_ES
>
> Administración de órdenes
>
> Aprende a gestionar órdenes con Mercado Pago Delivery.
>
> [Administración de órdenes](/developers/es/docs/mp-delivery/order-management)

