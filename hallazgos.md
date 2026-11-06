# Hallazgos

Defectos encontrados al ejecutar los 30 casos el **5 de noviembre de 2026**, con el usuario
`standard_user`.

Cada hallazgo sigue el formato que uso a diario: **qué está pasando**, **qué lo provoca**
y **qué puede provocar** si no se atiende. Si no se puede explicar en una frase que
entienda alguien del negocio, el caso se replantea antes de reportarlo.

> "Qué lo provoca" es mi hipótesis a partir del comportamiento observado. Sin acceso al
> código del servidor, la causa exacta la confirma el equipo de desarrollo.

---

## H-001 · Se puede completar una compra con el carrito vacío

| | |
|---|---|
| **Severidad** | Media (Alta si la confirmación dispara un despacho) |
| **Módulo** | Carrito / Checkout |
| **Caso relacionado** | CP-CAR-007 |
| **Evidencia** | [Resumen en $0.00](evidencias/H-001-resumen-orden-vacia-0.00.jpg) · [Orden confirmada](evidencias/H-001-orden-vacia-confirmada.jpg) |

**Qué está pasando**
Con el carrito vacío, el botón **Checkout** está habilitado. Se puede completar el
formulario, el resumen muestra un total de $0.00 y la compra se confirma con *"Thank you for
your order!"*.

**Qué lo provoca**
Ni el carrito ni el checkout validan que haya al menos un producto antes de avanzar.

**Qué puede provocar**
Órdenes vacías en el sistema que alguien tiene que detectar y limpiar a mano. Si la
confirmación dispara procesos de despacho o facturación, se generan envíos o comprobantes
sin productos.

**Pasos para reproducir**
1. Iniciar sesión y verificar que el carrito está vacío
2. Abrir el carrito y pulsar **Checkout**
3. Completar nombre, apellido y código postal y pulsar **Continue**
4. Pulsar **Finish**

**Resultado actual:** resumen en $0.00 y orden confirmada.
**Resultado esperado:** no permitir iniciar el checkout sin productos.

---

## H-002 · El checkout acepta campos con solo espacios

| | |
|---|---|
| **Severidad** | Media |
| **Módulo** | Checkout |
| **Caso relacionado** | CP-CHK-005 |
| **Evidencia** | [Formulario con solo espacios](evidencias/H-002-1-formulario-solo-espacios.jpg) · [Avanza al resumen](evidencias/H-002-2-avanza-al-resumen.jpg) |

**Qué está pasando**
Si se escriben solo espacios en First Name, Last Name y Zip/Postal Code, el formulario
avanza al resumen sin mostrar ningún error.

**Qué lo provoca**
La validación solo comprueba que el campo no esté vacío; no recorta los espacios antes de
validar.

**Qué puede provocar**
Órdenes sin datos reales de envío. El problema aparece después, cuando el pedido no se
puede entregar y hay que contactar al cliente sin saber quién es.

**Pasos para reproducir**
1. Con un producto en el carrito, pulsar **Checkout**
2. Escribir tres espacios en cada campo
3. Pulsar **Continue**

**Resultado actual:** avanza al resumen.
**Resultado esperado:** mostrar el error de campo obligatorio.

---

## H-003 · Los espacios al pegar credenciales impiden el acceso

| | |
|---|---|
| **Severidad** | Media |
| **Módulo** | Autenticación |
| **Caso relacionado** | CP-LOG-005 |
| **Evidencia** | [Usuario con espacios (resaltados) y el error](evidencias/H-003-espacios-en-usuario.jpg) |

**Qué está pasando**
Si el usuario copia y pega sus credenciales y el texto arrastra un espacio al inicio o al
final, el acceso se rechaza con el mensaje de credenciales incorrectas.

**Qué lo provoca**
El formulario no recorta los espacios antes de validar.

**Qué puede provocar**
El usuario ve "contraseña incorrecta" cuando en realidad la escribió bien. Genera
reclamos de soporte, reintentos y, si hay bloqueo por intentos fallidos, cuentas
bloqueadas sin motivo real.

**Pasos para reproducir**
1. Escribir `"  standard_user  "`, con dos espacios antes y dos después
2. Escribir la contraseña válida
3. Pulsar **Login**

**Resultado actual:** *"Epic sadface: Username and password do not match any user in this service"*.
**Resultado esperado:** recortar los espacios y permitir el acceso.

---

## H-004 · Las credenciales válidas son visibles en la pantalla de acceso

| | |
|---|---|
| **Severidad** | Media (Alta si el entorno fuera productivo) |
| **Módulo** | Autenticación |
| **Evidencia** | [Pantalla de acceso](evidencias/H-004-credenciales-visibles.jpg) |

**Qué está pasando**
La pantalla de inicio de sesión muestra la lista de usuarios y la contraseña en texto
plano, visible para cualquiera que abra la página.

**Qué lo provoca**
Es intencional: el sitio es un entorno público de práctica. Se documenta porque **en un
producto real sería un defecto crítico**.

**Qué puede provocar**
Si este patrón llegara a producción, cualquiera podría acceder sin autorización. Es el
tipo de detalle que a veces sobrevive desde el ambiente de pruebas por descuido.

---

## H-005 · El orden del catálogo se pierde al volver de la ficha

| | |
|---|---|
| **Severidad** | Media |
| **Módulo** | Catálogo |
| **Caso relacionado** | CP-CAT-006 |
| **Evidencia** | [Ordenado por precio](evidencias/H-005-1-orden-por-precio.jpg) · [Al volver de la ficha](evidencias/H-005-2-al-volver-de-la-ficha.jpg) |

**Qué está pasando**
Si el usuario ordena el catálogo por precio, abre la ficha de un producto y pulsa
**Back to products**, el catálogo vuelve a **Name (A to Z)**.

**Qué lo provoca**
El orden elegido no se guarda; el catálogo se vuelve a cargar con el orden por defecto.

**Qué puede provocar**
El usuario tiene que volver a ordenar cada vez que mira un producto. No impide comprar,
pero en un catálogo grande hace que comparar precios sea tedioso y aumenta el abandono.

**Pasos para reproducir**
1. Elegir **Price (low to high)**
2. Abrir la ficha de cualquier producto
3. Pulsar **Back to products**

**Resultado actual:** el selector muestra Name (A to Z).
**Resultado esperado:** conservar Price (low to high).

---

## H-006 · El subtotal en cero se muestra sin decimales

| | |
|---|---|
| **Severidad** | Baja |
| **Módulo** | Checkout |
| **Encontrado durante** | CP-CAR-007 |
| **Evidencia** | [Resumen: "Item total: $0"](evidencias/H-001-resumen-orden-vacia-0.00.jpg) |

**Qué está pasando**
En el resumen de una orden vacía, el subtotal aparece como *"Item total: $0"*, mientras
que el impuesto y el total aparecen como *"$0.00"*.

**Qué lo provoca**
El subtotal no usa el mismo formato de moneda que el resto de los importes.

**Qué puede provocar**
Solo un detalle visual, pero muestra que el formato de moneda no está centralizado: si
cambia en un lugar, puede quedar distinto en otro.

---

## Resumen

| Severidad | Cantidad |
|---|---|
| Media | 5 |
| Baja | 1 |
| **Total** | **6** |
