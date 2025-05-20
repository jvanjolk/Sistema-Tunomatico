El presente trabajo consiste en el modelado arquitectónico completo de: Tunomático, en el cual siguiendo las buenas practicas de diseño orientado a objetos y aplicando 3 patrones de diseño los cuales son: Prototype, Singleton, Adapter.

El sistema considera aspectos operacionales en un entorno digital del Sistema Tunomatico:

- Inicio de sesion y registro de usuario, a partir de RUT y contraseña del usuario.
- Registro,consulta y cancelacion del turno propiamente tal del usuario.
- La consulta, Gestion de Usuarios, Gestion de Turnos y Reporte de Turnos por parte del administrador encargado.
- Notificaciones de turno al usuario.
- El objetivo es demostrar la transición completa desde la visión funcional (casos de uso) hasta la arquitectura física (implementación), reflejando tanto el diseño lógico (diagrama de clases con patrones aplicados) como la distribución en nodos y componentes reales (diagrama de implementación UML).

Añadiendo a lo anterior, los diagramas de clases, implementacion y caso de uso, fueron realizados en Plant UML.


---


# 📌 1. Diagrama de Casos de Uso






## 🧑‍💻 Actores Presentes en el Caso de Uso

- **Usuario**: Puede registrarse, iniciar sesión y gestionar turnos.
- **Administrador**: Tiene permisos avanzados para gestionar usuarios, turnos y reportes.
---
### 🔹 Funciones del Usuario
-  **Registrarse** y **Iniciar sesión** en la plataforma.
-  **Registrar Turno** y  **Cancelar Turno** cuando sea necesario.
-  **Consultar Turno** para obtener información detallada.
-  **Notificación de Turno** cuando se registra o cancela un turno.

### 🔹 Funciones del Administrador
-  **Acceso Administrativo**, desde donde puede:
  -  **Gestionar Usuarios** para eliminar, modificar o añadir usuarios manualmente.
  -  **Gestionar Turnos** para eliminar, modificar o registrar turno manualmente.
  -  **Gestionar Reportes** para eliminar, modificar o realizar un reporte, manualmente.
- También puede acceder a **Consultar Turno** y **Reporte de Turnos**.
---
## 🔗 Relaciones entre Casos de Uso

### ✅ **`<<include>>`** 
Cuando una acción requiere otra para completarse:
-  **Registrar Turno** → incluye →  **Consultar Turno**  
  _(Es necesario verificar los turnos existentes al registrar uno nuevo)._
-  **Cancelar Turno** → incluye →  **Notificación de Turno**  
  _(Se envía una notificación tras la cancelación)._
- **Registrar Turno** → incluye →  **Notificación de Turno**  
  _(El usuario recibe una confirmación al registrar un turno)._

### ✅ **`<<extend>>`** 
Cuando una funcionalidad se habilita bajo ciertas condiciones:
-  **Acceso Administrativo** → extiende:
  -  **Gestionar Usuarios**
    _(el adm
inistrador puede ingresar a gestionar uno o más usuarios cada que sea pertinente o ocurra un caso especial)._
  -  **Gestionar Turnos**
    _(el administrador puede ingresar a gestionar uno o más turnos cada que sea pertinente o ocurra un caso especial)._
  -  **Gestionar Reportes**
    _(el administrador puede ingresar a gestionar uno o más reportes cada que sea pertinente o ocurra un caso especial)._
  _(El administrador puede realizar estas funciones después de autenticarse)._



---



# 📌 Diagrama de Clases







Se detallan las **relaciones** dentro del sistema **TUNOMATICO**, cómo interactúan las clases y por qué se han aplicado estos patrones de diseño. También se incluyen ejemplos basados en la gestión de turnos dentro del propio sistema.

## 🔗 **Relaciones Entre Clases**

### ✅ **Asociación: Relación Directa Entre Entidades**
📌 **Uso:** Modela la interacción natural entre dos clases sin dependencia rígida.

- `Usuario` → tiene múltiples **Turno**  
  - **Ejemplo en TUNOMATICO:** Un usuario del sistema reserva varios turnos para distintos servicios, como atención médica o asesoría legal.  
- `Usuario` → usa `GestorTurnos` para registrar y cancelar turnos.  
  - **Ejemplo en TUNOMATICO:** Un cliente programa una cita en una sucursal del sistema y posteriormente la cancela si ya no puede asistir.  
- `Administrador` → usa `GestorTurnos` y `GestorReportes` para gestionar turnos y reportes.  
  - **Ejemplo en TUNOMATICO:** Un administrador revisa todos los turnos asignados en un día y genera un informe de asistencia para la jornada.

---

### ✅ **Dependencia: Una Clase Usa a Otra Como Función**
📌 **Uso:** Una clase necesita otra para realizar una tarea, pero no es propietaria de ella.

- `GestorTurnos` → depende de `Turno` para gestionar la asignación de turnos.  
  - **Ejemplo en TUNOMATICO:** Cada turno registrado por un usuario se gestiona dentro del sistema, permitiendo visualizar disponibilidad y horarios.  
- `GestorReportes` → depende de `Reporte` para generar documentos detallados.  
  - **Ejemplo en TUNOMATICO:** Se genera un reporte mensual que muestra la cantidad de turnos atendidos y las tasas de cancelación.

---

### ✅ **Uso de Interfaces (`Notificador`, `ExportadorReporte`)**
📌 **Uso:** Permite desacoplar las implementaciones concretas y hacerlas intercambiables.

- `GestorTurnos` → usa `Notificador` para enviar alertas a los usuarios sobre sus turnos.  
  - **Ejemplo en TUNOMATICO:** Cuando un usuario agenda un turno, recibe una notificación por correo o SMS recordándole la fecha y hora.  
- `GestorReportes` → usa `ExportadorReporte` para generar informes en distintos formatos.  
  - **Ejemplo en TUNOMATICO:** Un administrador descarga un informe en PDF con el detalle de los turnos atendidos durante el mes.

---

### ✅ **Adaptador (`Adapter`): Conexión con Servicios Externos**
📌 **Uso:** Convierte una interfaz existente en otra compatible con el sistema.

- `AdaptadorNotificador` → adapta `APINotificaciones` para enviar mensajes.  
  - **Ejemplo en TUNOMATICO:** El sistema se integra con WhatsApp para enviar confirmaciones de turno a los clientes.  
- `AdaptadorReportePDF` → adapta `ExportadorReporte` para generar documentos PDF.  
  - **Ejemplo en TUNOMATICO:** Un administrador genera un reporte de turnos en PDF y lo comparte con el equipo de planificación.

---

## 🎯 **Patrones de Diseño Aplicados en la Gestión de Turnos**
Estos patrones optimizan la estructura del sistema y mejoran la modularidad.

### ✅ **Singleton**
📌 **Uso:** Garantiza que haya una única instancia de `GestorTurnos` y `GestorReportes`.

📌 **Ejemplo en TUNOMATICO:**  
El sistema tiene un único **Gestor de Turnos**, centralizando todas las reservas en una sola instancia para evitar conflictos de horarios y duplicaciones en los turnos.

---

### ✅ **Prototype**
📌 **Uso:** Permite la clonación de objetos `Turno`.

📌 **Ejemplo en TUNOMATICO:**  
Cuando un usuario reserva un turno similar a uno anterior (por ejemplo, misma categoría de servicio en distinto día), el sistema clona el turno previo en lugar de crear uno desde cero.

---

### ✅ **Adapter**
📌 **Uso:** Convierte una interfaz de un servicio externo para que se integre con el sistema.

📌 **Ejemplo en TUNOMATICO:**  
El sistema se adapta a una API de notificaciones que permite enviar recordatorios de turnos programados, asegurando que los clientes no olviden sus citas.

---

