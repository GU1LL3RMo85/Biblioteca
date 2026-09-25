\# Requisitos del sistema

El sistema de gestión de biblioteca deberá permitir registrar libros, usuarios, préstamos y devoluciones, etc.

También deberá permitir consultar la disponibilidad de los libros y buscar ejemplares por título o autor.

El sistema deberá permitir identificar los libros que se encuentran prestados y mostrar la fecha prevista de devolución.

-El sistema limitará a cada usuario a un máximo de 5 libros prestados al mismo tiempo.

## RF-05: Solicitud de Reserva de Libros No Disponibles

### Descripción
El sistema debe permitir a los usuarios registrados solicitar la reserva de un libro cuando este no se encuentre disponible para préstamo inmediato (por encontrarse prestado o en mantenimiento).

### Datos Obligatorios del Registro
Cada solicitud de reserva debe capturar e incorporar de forma automática e inalterable:
1. **Identificador del Usuario:** Código o matrícula del usuario que solicita la reserva.
2. **Identificador del Libro:** Código de catálogo del ejemplar reservado.
3. **Fecha y Hora de la Solicitud:** Marca temporal (timestamp) del momento exacto en que se efectúa la reserva.
4. **Estado Inicial:** Asignación automática del estado `PENDIENTE`.

### Reglas de Negocio (RN)
* **RN-05.1 (Condición de Disparo):** La opción "Reservar" solo estará activa si la cantidad de copias disponibles en estantería es igual a cero.
* **RN-05.2 (Priorización por Tipo de Usuario):** En caso de haber múltiples reservas para un mismo libro, se asignará prioridad en el siguiente orden:
  1. Profesores (por actividades docentes).
  2. Estudiantes y Personal Administrativo (por orden cronológico de registro).
* **RN-05.3 (Límite de Reservas):** Un usuario no podrá tener más de 3 reservas activas simultáneamente.