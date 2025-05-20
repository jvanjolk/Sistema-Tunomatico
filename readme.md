# SISTEMA DE GESTION DE TURNOS DIGITALES "TUNOMATICO"
El presente trabajo consiste en el modelado arquitectónico completo de: Tunomático, en el cual siguiendo las buenas practicas de diseño orientado a objetos y aplicando 3 patrones de diseño los cuales son: Prototype, Singleton, Adapter.

El sistema considera aspectos operacionales en un entorno digital de turnos, reportes, usuarios:

- Inicio de sesion y registro de usuario, a partir de RUT y contraseña del usuario.
- Registro,consulta y cancelacion del turno propiamente tal del usuario.
- La consulta, Gestion de Usuarios, Gestion de Turnos, Gestion de Reportes y creacion de Reportes de Turnos por parte del administrador encargado.
- Notificaciones de turno al usuario.
- El objetivo es demostrar la transición completa desde la visión funcional (casos de uso) hasta la arquitectura física (implementación), reflejando tanto el diseño lógico (diagrama de clases con patrones aplicados) como la distribución en nodos y componentes reales (diagrama de implementación UML).

Añadiendo a lo anterior, los diagramas de clases, implementacion y caso de uso, fueron realizados en Plant UML.
Ademas, cabe recalcar que este Sistema fue diseñado para variables casos para un uso en la vida real, ya sea para aplicarse en un lugar de atencion medica, atencion municipal, etc.

---


# 📌 1. Diagrama de Casos de Uso

![casos de uso](https://github.com/jvanjolk/Sistema-Tunomatico/blob/main/diagrama_casodeuso.png)





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
- También puede acceder a **Consultar Turno** y **Reporte de Turnos** desde donde puede:
  - **Consultar Turno** para una gestion de estos, ya sea ver turnos cancelados u que estan operativos y llevar un orden dentro del sistema y lugar de atencion que se aplique este sistema de turnos

---


## 🔗 Relaciones entre Casos de Uso

### 🔹 **`<<include>>`** 
Cuando una acción requiere otra para completarse:
-  **Registrar Turno** → incluye →  **Consultar Turno**  
  _(Es necesario verificar los turnos existentes al registrar uno nuevo)._
-  **Cancelar Turno** → incluye →  **Notificación de Turno**  
  _(Se envía una notificación tras la cancelación)._
- **Registrar Turno** → incluye →  **Notificación de Turno**  
  _(El usuario recibe una confirmación al registrar un turno)._

### 🔹 **`<<extend>>`** 
Cuando una funcionalidad se habilita bajo ciertas condiciones:
-  **Acceso Administrativo** → extiende:
  -  **Gestionar Usuarios**
    _(el administrador puede ingresar a gestionar uno o más usuarios cada que sea pertinente o ocurra un caso especial)._
  -  **Gestionar Turnos**
    _(el administrador puede ingresar a gestionar uno o más turnos cada que sea pertinente o ocurra un caso especial)._
  -  **Gestionar Reportes**
    _(el administrador puede ingresar a gestionar uno o más reportes cada que sea pertinente o ocurra un caso especial)._
  _(El administrador puede realizar estas funciones después de autenticarse)._



---



# 📌 Diagrama de Clases



![diagrama de clases](https://github.com/jvanjolk/Sistema-Tunomatico/blob/main/diagrama_clases.png)



Se detallan las **relaciones** dentro del sistema **TUNOMATICO**, cómo interactúan las clases y por qué se han aplicado estos patrones de diseño. También se incluyen ejemplos basados en la gestión de turnos dentro del propio sistema.


## 🔗 **Relaciones Entre Clases**

### 🔗 **Asociación: Relación Directa Entre Entidades**
📌 **Uso:** Modela la interacción natural entre dos clases sin dependencia rígida.

- `Usuario` → tiene múltiples `Turno`  
  - **Ejemplo en TUNOMATICO:** Un usuario del sistema reserva varios turnos para distintos servicios, como atención médica o asesoría legal.  
- `Usuario` → usa `GestorTurnos` para registrar y cancelar turnos.  
  - **Ejemplo en TUNOMATICO:** Un cliente programa una cita en una sucursal del sistema y posteriormente la cancela si ya no puede asistir.  
- `Administrador` → usa `GestorTurnos` y `GestorReportes` para gestionar turnos y reportes.  
  - **Ejemplo en TUNOMATICO:** Un administrador revisa todos los turnos asignados en un día y genera un informe de asistencia para la jornada.

---

### 🔗 **Dependencia: Una Clase Usa a Otra Como Función**
📌 **Uso:** Una clase necesita otra para realizar una tarea, pero no es propietaria de ella.

- `GestorTurnos` → depende de `Turno` para gestionar la asignación de turnos.  
  - **Ejemplo en TUNOMATICO:** Cada turno registrado por un usuario se gestiona dentro del sistema, permitiendo visualizar disponibilidad y horarios.  
- `GestorReportes` → depende de `Reporte` para generar documentos detallados.  
  - **Ejemplo en TUNOMATICO:** Se genera un reporte mensual que muestra la cantidad de turnos atendidos y las tasas de cancelación.

---


### 🔗 **Uso de Interfaces (`Notificador`, `ExportadorReporte`)**
📌 **Uso:** Permite desacoplar las implementaciones concretas y hacerlas intercambiables.

- `GestorTurnos` → usa `Notificador` para enviar alertas a los usuarios sobre sus turnos.  
  - **Ejemplo en TUNOMATICO:** Cuando un usuario agenda un turno, recibe una notificación por correo o SMS recordándole la fecha y hora.  
- `GestorReportes` → usa `ExportadorReporte` para generar informes en distintos formatos.  
  - **Ejemplo en TUNOMATICO:** Un administrador descarga un informe en PDF con el detalle de los turnos atendidos durante el mes.

---


#### 🔗 **Adaptador (`Adapter`): Conexión con Servicios Externos**
📌 **Uso:** Convierte una interfaz existente en otra compatible con el sistema.

- `AdaptadorNotificador` → adapta `APINotificaciones` para enviar mensajes.  
  - **Ejemplo en TUNOMATICO:** El sistema se integra con WhatsApp para enviar confirmaciones de turno a los clientes.  
- `AdaptadorReportePDF` → adapta `ExportadorReporte` para generar documentos PDF.  
  - **Ejemplo en TUNOMATICO:** Un administrador genera un reporte de turnos en PDF y lo comparte con el equipo de planificación.

---


## 🏗️ **Patrones de Diseño Aplicados**

### 🔁 Singleton (`GestorTurnos` y `GestorReportes`)
**Objetivo:** Garantizar que exista una única instancia del objeto en toda la aplicación.  

📌**Aplicación en el sistema TUNOMATICO:**
- `GestorTurnos` centraliza la administración de turnos, evitando inconsistencias.  
- `GestorReportes` permite la generación única de reportes, asegurando datos coherentes. 

---


### 🔁 Prototype (`Turno`)
**Obejtivo:** Evitar la creación repetitiva de objetos costosos, permitiendo la clonación eficiente.

📌 **Aplicación en el sistema TUNOMATICO**:
- `Turno`:
  - En lugar de instanciar nuevos turnos desde cero, el sistema puede clonar turnos preexistentes con modificaciones mínimas.

  - Ideal para cuando se crean múltiples turnos con características similares, agilizando el proceso.

---


### 🔁 Adapter (`AdaptadorNotificador` y `AdaptadorReportePDF`)
**Obejtivo**: Permitir la integración de sistemas con interfaces incompatibles mediante una capa intermedia.

📌 Aplicación en TUNOMATICO:
- `AdaptadorNotificador` permite que el sistema de notificaciones del gestor de turnos funcione con una API externa sin necesidad de modificar su estructura interna.

- `AdaptadorReportePDF` se encarga de exportar reportes en formato PDF, adaptando el sistema de generación de reportes a una librería externa.


---



# 📌 Diagrama de Implementacion 

![diagrama de implementacion](https://github.com/jvanjolk/Sistema-Tunomatico/blob/main/diagrama_implementacion.png)

## 🏗️ **Arquitectura del Sistema**
El sistema está dividido en módulos organizados en distintas capas: **Cliente**, **Servidor Web**, **Base de Datos** y **Servicios Externos**, asegurando una separación clara de responsabilidades.


## 📌 **Componentes Principales**

### 👦 **Cliente** 
Interfaz de usuario para interactuar con el sistema.
- `Interfaz Tunomatico` → Interfaz para que los usuarios gestionen sus turnos.
---


### 🌐🖥 **Servidor Web**
Centro de operaciones del sistema que gestiona la lógica y la comunicación con la base de datos.
- `Servicio de Gestión de Turnos` → Procesa la creación, modificación y cancelación de turnos.
- `Servicio de Gestión de Usuarios` → Administra cuentas de usuario y autenticación.
- `GestorTurnos` (**Singleton**) → Controla la gestión única de turnos.
- `GestorReportes` (**Singleton**) → Encargado de la generación de reportes.
- `Controlador de Usuario` → Maneja la interacción entre el cliente y los servicios del sistema.
- `Servicio de Autenticación de Usuarios` → Valida credenciales para el acceso al sistema.
---


### 👨‍💼 **Administrador**
Módulo encargado de la supervisión y gestión del sistema de turnos.
- `Interfaz Administrador` → para que el administrador accesa a sus permisos administrativos, para gestionar el sistema o generar un reporte. Se comunica con `GestorTurnos` para administrar turnos.
- `Administrador` → Gestiona usuarios, turnos y reportes.
- `Servicio de Gestión de Turnos` → Permite modificar o eliminar turnos asignados a los usuarios.
- `GestorReportes` → Permite generar informes de gestión sobre la actividad en el sistema.
---
### 🛢 **Base de Datos**
Almacena la información de usuarios, turnos y reportes.
- `BD Usuarios` → Registra credenciales y datos de usuarios.
- `BD Turnos` → Guarda los turnos registrados.
- `BD Reportes` → Almacena informes generados por el sistema.
---
### 🔗🔀 **Servicios Externos**
Interfaces de terceros integradas con **TUNOMATICO** para mejorar la funcionalidad.
- `API de Notificaciones` → Servicio externo para enviar alertas de turnos.
- `Exportador PDF` → Biblioteca para generar reportes en formato PDF.

---

# 🤔💡🤓☝️ Reflexiones Finales del modelado
- El modelado de Tunomático representa un enfoque sólido en el diseño de sistemas de gestión digital de turnos, logrando una estructura clara y organizada. Se han aplicado principios fundamentales de orientación a objetos, asegurando la modularida. 
- Los patrones de diseño utilizados: Prototype, Singleton y Adapter, que permiten escalabilidad y eficiencia operativa, elementos clave en cualquier sistema que maneje flujos de datos constantes.
- El desarrollo y representación a través de PlantUML ha sido una decisión acertada, ya que facilita la comprensión del modelo a través de diagramas estructurados. Además, la transición desde casos de uso hasta la implementación física
  refuerza la coherencia entre los requisitos funcionales y la arquitectura técnica.
- Uno de los aspectos más valiosos del diseño es su versatilidad para distintos entornos, desde atención médica hasta administración municipal, demostrando su potencial para ser adaptado a necesidades reales. La incorporación de notificaciones y reportes también optimiza la experiencia del usuario y la gestión interna.
✅En conclusión, el Tunomático no solo representa un buen ejercicio de modelado arquitectónico, sino que también sienta bases sólidas para la implementación de un sistema escalable y eficiente, alineado con buenas prácticas de desarrollo y gestión de un sistema digital.










