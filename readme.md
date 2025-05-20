El presente trabajo consiste en el modelado arquitectónico completo de: Tunomático, en el cual siguiendo las buenas practicas de diseño orientado a objetos y aplicando 3 patrones de diseño los cuales son: Prototype, Singleton, Adapter.

El sistema considera aspectos operacionales en un entorno digital del Sistema Tunomatico:

- Inicio de sesion y registro de usuario, a partir de RUT y contraseña del usuario.
- Registro,consulta y cancelacion del turno propiamente tal del usuario.
- La consulta, Gestion de Usuarios, Gestion de Turnos y Reporte de Turnos por parte del administrador encargado.
- Notificaciones de turno al usuario.
- El objetivo es demostrar la transición completa desde la visión funcional (casos de uso) hasta la arquitectura física (implementación), reflejando tanto el diseño lógico (diagrama de clases con patrones aplicados) como la distribución en nodos y componentes reales (diagrama de implementación UML).

Añadiendo a lo anterior, los diagramas de clases, implementacion y caso de uso, fueron realizados en Plant UML.


---


# 📌 1. Diagrama de Casos de Uso UML


## 🧑‍💻 Actores Presentes en el Caso de Uso

- **Usuario**: Puede registrarse, iniciar sesión y gestionar turnos.
- **Administrador**: Tiene permisos avanzados para gestionar usuarios, turnos y reportes.


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

