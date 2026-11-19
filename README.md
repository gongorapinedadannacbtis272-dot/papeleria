# Happy Papelería 🌈

Proyecto web de papelería hecho con **Flask + MongoDB**. Integra el diseño y catálogo de Happy con las funciones de reserva temporal, cuentas de usuario y comprobante QR del proyecto de referencia Honey.

## Funciones incluidas

- Catálogo de productos de papelería con imagen, descripción, precio y existencias.
- Panel de administración para agregar, editar y eliminar productos.
- Registro de clientes con nombre, correo y contraseña.
- Inicio y cierre de sesión.
- Contraseñas guardadas con hash seguro (no se guardan como texto).
- Roles separados: cliente y administrador.
- Reserva de una unidad durante **5 minutos** al agregar un producto al carrito.
- Contador regresivo en el carrito.
- Cancelación de reservas y devolución de existencias.
- Liberación de existencias cuando vence una reserva.
- Checkout protegido: se necesita iniciar sesión para confirmar.
- Comprobante de compra con detalle, fecha, hora, total y código QR.
- El QR abre el comprobante de esa compra mediante un token aleatorio.

## Requisitos

- Python 3.10 o posterior.
- MongoDB local o una cuenta gratuita de MongoDB Atlas.
- Conexión a internet para cargar Bootstrap y generar la imagen QR.

## Instalar y ejecutar en Windows

1. Descomprime `Happy.zip`.
2. Abre la carpeta `Happy` en Visual Studio Code.
3. Abre Terminal > Nueva terminal.
4. Crea el entorno e instala dependencias:

```powershell
py -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
```

5. Configura MongoDB y las variables de entorno en esa misma terminal. Ejemplo:

```powershell
$env:MONGO_URI = "TU_CADENA_DE_MONGODB_ATLAS"
$env:SECRET_KEY = "pon-aqui-una-clave-larga-y-privada"
$env:ADMIN_EMAIL = "tu-correo-administrador@ejemplo.com"
$env:ADMIN_PASSWORD = "una-contrasena-segura"
$env:RESERVA_SEGUNDOS = "300"
$env:APP_BASE_URL = "http://127.0.0.1:5000"
```

6. Inicia la aplicación:

```powershell
.\.venv\Scripts\python.exe app.py
```

7. Abre http://127.0.0.1:5000 en el navegador.

> Si tienes MongoDB instalado localmente, puedes omitir `MONGO_URI`; la aplicación intentará conectarse a `mongodb://127.0.0.1:27017/`. Si usas Atlas, pega tu cadena de conexión en `MONGO_URI`.

## Crear la cuenta administradora

La primera vez, entra a **Crear cuenta** y registra el correo indicado en `ADMIN_EMAIL`. Esa cuenta recibe el rol de administrador. También se puede crear al iniciar sesión con `ADMIN_EMAIL` y `ADMIN_PASSWORD` si todavía no existe.

Las demás personas pueden crear sus propias cuentas desde **Crear cuenta**. Se registran como clientes y no pueden entrar al panel de administración.

No compartas las credenciales del administrador ni publiques `MONGO_URI`, `SECRET_KEY` o contraseñas en GitHub.

## Cómo agregar productos

1. Inicia sesión con la cuenta administradora.
2. Abre **Administrar**.
3. Agrega nombre, categoría, precio, existencias, descripción e imagen opcional.
4. Guarda. El producto aparecerá en el catálogo.

## Cómo funciona la reserva de 5 minutos

1. Entra al catálogo y pulsa **Reservar durante 5 minutos**.
2. El sistema descuenta una unidad disponible y la aparta para ese navegador.
3. En el carrito aparece un contador de cinco minutos.
4. Puedes cancelar para devolver la unidad al inventario.
5. Si el contador llega a cero, la reserva vence y la unidad regresa al inventario cuando se consulta el catálogo o el carrito.
6. Para confirmar, inicia sesión y completa el checkout antes de que termine la reserva.

## Comprobante QR

El QR se genera **después de confirmar la compra**, no en las tarjetas del catálogo. Al escanearlo se abre el comprobante correspondiente. Para que el QR funcione desde otros celulares cuando publiques el proyecto, configura `APP_BASE_URL` con la dirección pública del sitio (por ejemplo, la URL de Render).

## Importante

Este es un proyecto demostrativo/escolar. El checkout registra una compra de demostración, pero **no cobra dinero ni procesa pagos reales**. No solicita datos de tarjetas bancarias.

## Publicar en Render

- Build Command: `pip install -r requirements.txt`
- Start Command: `gunicorn app:app`
- Variables de entorno: `MONGO_URI`, `SECRET_KEY`, `ADMIN_EMAIL`, `ADMIN_PASSWORD`, `RESERVA_SEGUNDOS=300`, `APP_BASE_URL=https://TU-APP.onrender.com`.

En MongoDB Atlas habilita acceso de red para el servicio y concede permisos de lectura/escritura al usuario de base de datos.
