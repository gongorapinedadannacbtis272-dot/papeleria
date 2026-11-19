# Correcciones de la versión de reserva temporal

- Se definieron las funciones auxiliares de tiempo UTC, identificación de comprador, consulta de reservas activas y liberación de reservas vencidas.
- Se agregó la configuración `RESERVA_SEGUNDOS` (60 segundos por defecto).
- La reserva registra el comprador y, cuando existe, el usuario de la cuenta.
- Se conserva la identidad de reserva al iniciar sesión para que el carrito no se pierda.
- Se simplificó la confirmación escolar: no solicita números de tarjeta ni CVV y no procesa pagos.
- Se conserva la hoja de estilos original `static/estilos.css`.

Consulta `README.md` para instalar, configurar Atlas y probar los casos de la actividad.


## Reserva de cinco minutos y comprobante QR tipo cine

- La duración predeterminada de la reserva temporal es de 300 segundos (5 minutos), configurable con `RESERVA_SEGUNDOS=300`.
- Se eliminó el QR de las tarjetas de productos: no se muestra al explorar, reservar ni antes de pagar.
- Después de confirmar el checkout se registra la compra y se genera un comprobante con QR.
- El comprobante muestra los productos, cantidades, subtotales, total, fecha y hora de compra.
- Al escanear el QR se vuelve a abrir el comprobante de esa compra. El enlace usa un token aleatorio y no incluye datos personales.
- Configura `APP_BASE_URL` en Render con la URL pública de la aplicación para que el QR abra el comprobante desde otros celulares.
- La compra sigue en modo demostración: no se realiza ningún cobro real.
