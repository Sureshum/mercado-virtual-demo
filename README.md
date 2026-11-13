# 🛒 SSMarket · Tienda virtual y delivery

**SSMarket** es una plataforma web de compras y delivery pensada para un mercado de barrio: catálogo de productos, carrito, compra como invitado, pagos manuales, factura imprimible, chatbot de atención y un panel de administración completo.

Este repositorio es la **cara pública del proyecto**: documenta qué hace la página completa y cómo probar la demo. El código fuente del sistema (frontend + API + base de datos) se mantiene en un repositorio privado.

<p>
  <img alt="React" src="https://img.shields.io/badge/React-18-61DAFB?logo=react&logoColor=white" />
  <img alt="Node.js" src="https://img.shields.io/badge/Node.js-Express-339933?logo=node.js&logoColor=white" />
  <img alt="PostgreSQL" src="https://img.shields.io/badge/PostgreSQL-16-4169E1?logo=postgresql&logoColor=white" />
  <img alt="Tailwind" src="https://img.shields.io/badge/Tailwind_CSS-3-06B6D4?logo=tailwindcss&logoColor=white" />
</p>

## 🔗 Enlaces

| Entorno | URL |
|---|---|
| 🛍️ Tienda en vivo (API + PostgreSQL) | **https://ssmarket-tienda.onrender.com** |

> La tienda está desplegada en el plan gratuito de Render: si lleva más de 15 minutos sin visitas, el primer acceso puede tardar unos segundos en despertar.

## ✨ Qué hace la página completa

### 1. Portada y catálogo

- **Carrusel de destacados** con hasta 4 productos elegidos por el administrador (o los más vendidos). Avanza solo cada 4 segundos, con barras laterales translúcidas y un desvanecimiento cruzado entre fotos.
- **Catálogo con 12 categorías**: frutas y verduras, carnes, lácteos, despensa, panadería, bebidas, snacks, aseo, cuidado personal, salud, mascotas y otros.
- **Buscador y filtros rápidos** por categoría, con resultados en vivo.
- **Tarjetas de producto** en 2 columnas en móvil, con foto, precio, stock y botón **Agregar** siempre visible.
- Cada precio se muestra en **dólares** y, debajo, su **equivalente en bolívares** según la tasa vigente.

### 2. Detalle del producto

- Foto, descripción, categoría, stock disponible y selector de cantidad.
- Botones para **agregar al carrito** o **comprar ahora**.
- Funciona también para visitantes sin cuenta.

### 3. Carrito y checkout

- Carrito resumido **sin fotos**, con el nombre del producto enlazado a su página.
- Control de cantidades, eliminar productos y vaciar el carrito.
- Total en dólares con su **equivalente en bolívares**.
- **Compra como invitado**: solo se pide un nombre; al finalizar se recomienda crear una cuenta.
- **Métodos de pago manuales**: Pago Móvil, transferencia bancaria, Zelle y pago contra entrega.
- El checkout muestra los datos de cobro configurados por la tienda y pide la **referencia del comprobante**.
- El pedido queda **pendiente** hasta que el administrador verifica el pago.
- **Factura imprimible** en una ruta propia, con enlace privado para invitados.

### 4. Cuentas y seguridad

- Registro e inicio de sesión con **tres roles**: `cliente`, `vendedor` y `admin`.
- La sesión viaja en una **cookie `httpOnly`** (el token no es accesible desde JavaScript).
- Contraseñas con **bcrypt**, validación de datos y **límite de peticiones** en rutas sensibles.
- **Recuperación de contraseña** con un código de 6 dígitos enviado por correo, válido 15 minutos y de un solo uso.
- El administrador también puede asignar contraseñas desde el panel.

### 5. Pedidos del cliente

- Historial en **Mis pedidos** con detalle de productos, total y estado del pago.
- Acceso a la factura de cada compra.

### 6. Chatbot de atención

- Asistente flotante disponible para clientes e invitados.
- Responde consultas sobre **productos reales del catálogo** (precio, stock, categorías, horarios y formas de pago).
- **Tolera errores de escritura**: si alguien escribe “attum”, responde “¿Quisiste decir atún?” y muestra los resultados.
- Si no puede resolver la consulta, **escala a un empleado** y crea un ticket de soporte.
- Las respuestas del empleado aparecen en el mismo chat (actualización por polling).

### 7. Soporte

- Bandeja de tickets para el personal, con conversación por ticket.
- Los tickets se eliminan automáticamente **24 horas** después de su creación.

### 8. Panel de administración

| Sección | Qué permite |
|---|---|
| **Resumen** | Métricas de productos, clientes, ventas y tickets abiertos. |
| **Usuarios** | Crear, editar y cambiar contraseñas; asignar roles. |
| **Inventario** | Alta, edición y borrado de productos, stock, precios y fotos. |
| **Ventas** | Revisar pedidos, verificar referencias y confirmar o rechazar pagos. |
| **Bandeja de soporte** | Responder tickets escalados por el chatbot. |
| **Configuración** | Elegir la tasa del dólar: automática del BCV o manual. |

### 9. Tasa del dólar (BCV o manual)

- Los precios se guardan **siempre en dólares**; la tasa solo se usa para mostrar el equivalente en bolívares.
- Modo **BCV**: consulta la tasa oficial y la actualiza cada 30 minutos.
- Modo **manual**: el administrador escribe la tasa y se mantiene hasta cambiarla.
- Un **cuadrito en la cabecera** muestra a clientes y empleados qué tasa se está usando (`BCV` o `Manual`) y su valor.
- Si la consulta al BCV falla, se conserva la última tasa conocida para no mostrar montos en cero.

### 10. Diseño y experiencia

- **Mobile-first** y totalmente responsivo.
- Catálogo en 2 columnas en móvil y hasta 4 en escritorio.
- Carrusel con transiciones suaves y botones laterales tipo barra translúcida.
- Mensajes de error claros, estados de carga y confirmaciones visuales.

## 🧪 La demo estática

La demo no necesita servidor ni base de datos:

- La API se **simula en el navegador**.
- Todo (cuentas, productos, pedidos, tickets y tasa del dólar) se guarda en `localStorage`.
- El aviso superior permite **restablecer la demo** en cualquier momento.
- La recuperación de contraseña es simulada y muestra el código en pantalla.
- La tasa del dólar funciona con datos de ejemplo y también permite el modo manual.

### Cuentas de prueba

| Rol | Correo | Contraseña |
|---|---|---|
| Administrador | `admin@ssmarket.com` | `admin1234` |
| Vendedor | `vendedor@ssmarket.com` | `vendedor123` |
| Cliente | `cliente@ssmarket.com` | `cliente123` |

## 🏗️ Arquitectura

```
Navegador (React SPA)
        │  cookie httpOnly
        ▼
API REST (Node.js + Express)
        ├── PostgreSQL (Neon)      → usuarios, productos, ventas, tickets, configuración
        ├── DolarAPI (BCV)         → tasa oficial del dólar
        └── Correo                 → Apps Script / Brevo / SMTP
```

| Capa | Tecnología |
|---|---|
| Frontend | React 18, Vite, Tailwind CSS, React Router v6 |
| Backend | Node.js, Express, JWT, bcrypt |
| Base de datos | PostgreSQL (Neon) |
| Correo | Google Apps Script + Gmail, Brevo o SMTP |
| Despliegue | Render (web service) + Neon (PostgreSQL) |

## 🚀 Puesta en marcha

El código completo es privado. Para levantar una copia local se necesitan las variables de entorno descritas en `backend/.env.example` y una base PostgreSQL:

```bash
# Backend
cd backend
npm install
npm run seed      # crea las tablas y carga datos de prueba
npm run dev       # API en http://localhost:4000

# Frontend (otra terminal)
cd frontend
npm install
npm run dev       # SPA en http://localhost:5173
```

## 📌 Alcance y limitaciones

- Los pagos son **manuales**: el administrador verifica la referencia antes de confirmar el pedido. No hay pasarela de tarjeta.
- El plan gratuito de Render duerme el servicio tras 15 minutos de inactividad.
- El envío de correo en producción usa un webhook HTTPS (Apps Script) porque Render free bloquea SMTP.
- La demo estática es una **simulación** para mostrar la experiencia; no procesa pagos ni envía correos reales.

## 👤 Autor

**Sureshum** · [GitHub](https://github.com/Sureshum)

---

<p align="center">Hecho con cariño para un mercado de barrio 🥑</p>
