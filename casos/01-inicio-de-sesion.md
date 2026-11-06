# Casos de prueba — Inicio de sesión

**Módulo:** Autenticación
**URL:** https://www.saucedemo.com
**Fecha de ejecución:** 5 de noviembre de 2026

---

## CP-LOG-001 · Ingreso con credenciales válidas

| | |
|---|---|
| **Tipo** | Funcional — camino feliz |
| **Prioridad** | Alta |
| **Precondición** | Usuario `standard_user` activo |

**Pasos**
1. Abrir https://www.saucedemo.com
2. Escribir `standard_user` en el campo Username
3. Escribir la contraseña válida en el campo Password
4. Pulsar **Login**

**Resultado esperado**
La aplicación redirige a `/inventory.html` y muestra el catálogo con los 6 productos. El
ícono del carrito aparece en la barra superior, sin contador.

**Resultado de la ejecución:** ✅ Pasa — Redirige a `/inventory.html`, muestra los 6 productos y el carrito sin contador.

---

## CP-LOG-002 · Contraseña incorrecta

| | |
|---|---|
| **Tipo** | Negativo |
| **Prioridad** | Alta |

**Pasos**
1. Abrir la pantalla de inicio de sesión
2. Escribir `standard_user` en Username
3. Escribir `contraseña_invalida` en Password
4. Pulsar **Login**

**Resultado esperado**
Se muestra el mensaje *"Epic sadface: Username and password do not match any user in this service"*. El
usuario permanece en la pantalla de login.

**Por qué importa:** el mensaje no debe revelar si el usuario existe o no. Decir "usuario
correcto, contraseña incorrecta" le confirma a un atacante que esa cuenta es válida.

**Resultado de la ejecución:** ✅ Pasa — Mensaje: *"Epic sadface: Username and password do not match any user in this service"*.

---

## CP-LOG-003 · Usuario bloqueado

| | |
|---|---|
| **Tipo** | Negativo |
| **Prioridad** | Alta |
| **Precondición** | Usuario `locked_out_user` |

**Pasos**
1. Ingresar `locked_out_user` con la contraseña válida
2. Pulsar **Login**

**Resultado esperado**
Mensaje *"Epic sadface: Sorry, this user has been locked out."*. No se concede acceso al catálogo.

**Por qué importa:** un usuario bloqueado que logra entrar es una falla de seguridad, no
un detalle de interfaz.

**Resultado de la ejecución:** ✅ Pasa — Mensaje: *"Epic sadface: Sorry, this user has been locked out."* No concede acceso.

---

## CP-LOG-004 · Campos vacíos

| | |
|---|---|
| **Tipo** | Negativo |
| **Prioridad** | Media |

**Pasos**
1. Dejar Username y Password vacíos
2. Pulsar **Login**

**Resultado esperado**
Mensaje *"Epic sadface: Username is required"*. Al completar solo el usuario y reintentar, el mensaje
cambia a *"Epic sadface: Password is required"*.

**Resultado de la ejecución:** ✅ Pasa — Mensajes: *"Epic sadface: Username is required"* y luego *"Epic sadface: Password is required"*.

---

## CP-LOG-005 · Espacios al inicio y al final

| | |
|---|---|
| **Tipo** | Borde |
| **Prioridad** | Media |

**Pasos**
1. Escribir `"  standard_user  "` con espacios antes y después
2. Escribir la contraseña válida
3. Pulsar **Login**

**Resultado esperado**
El sistema debería recortar los espacios y permitir el acceso.

**Por qué importa:** copiar y pegar credenciales arrastra espacios sin que el usuario lo
note. Rechazar el acceso por eso genera reclamos de soporte evitables.

**Resultado de la ejecución:** ❌ Falla — Con `"  standard_user  "` y la contraseña válida se rechaza el acceso como credenciales incorrectas. Ver **H-003**.

---

## CP-LOG-006 · La contraseña no se muestra en pantalla

| | |
|---|---|
| **Tipo** | Visual / Seguridad |
| **Prioridad** | Alta |

**Pasos**
1. Escribir cualquier texto en el campo Password
2. Observar el campo

**Resultado esperado**
Los caracteres aparecen enmascarados (puntos o asteriscos), nunca en texto plano.

**Resultado de la ejecución:** ✅ Pasa — El campo Password es de tipo contraseña: los caracteres se enmascaran.

---

## CP-LOG-007 · Acceso directo sin sesión iniciada

| | |
|---|---|
| **Tipo** | Negativo / Seguridad |
| **Prioridad** | Alta |

**Pasos**
1. Sin haber iniciado sesión, escribir directamente en la barra del navegador:
   `https://www.saucedemo.com/inventory.html`
2. Pulsar Enter

**Resultado esperado**
La aplicación redirige al login y muestra un mensaje de error. **No** debe mostrar el
catálogo.

**Por qué importa:** si la protección está solo en el botón y no en la ruta, cualquiera
que conozca la URL entra sin credenciales.

**Resultado de la ejecución:** ✅ Pasa — Redirige al login con *"Epic sadface: You can only access '/inventory.html' when you are logged in."*

---

## CP-LOG-008 · Cierre de sesión y botón Atrás

| | |
|---|---|
| **Tipo** | Regresión |
| **Prioridad** | Alta |

**Pasos**
1. Iniciar sesión correctamente
2. Abrir el menú lateral y pulsar **Logout**
3. Pulsar el botón **Atrás** del navegador

**Resultado esperado**
No se recupera la sesión. La aplicación permanece en el login.

**Por qué importa:** es el caso clásico que se escapa. En una computadora compartida, si
el botón Atrás devuelve la sesión, el siguiente usuario ve los datos del anterior.

**Resultado de la ejecución:** ✅ Pasa — Tras Logout, el botón Atrás vuelve al login con el mensaje de acceso restringido. La sesión no se recupera.

---

## Resumen

| Prioridad | Casos |
|---|---|
| Alta | 6 |
| Media | 2 |
| **Total** | **8** |

**Ejecución del 5 de noviembre de 2026:** 7 pasan · 1 falla

Casos que fallan: CP-LOG-005. Los defectos están en [hallazgos.md](../hallazgos.md).
