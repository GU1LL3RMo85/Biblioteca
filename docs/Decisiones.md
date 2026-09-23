# Uso de un modelo relacional para el control de inventario y préstamos

**Fecha:** 2026-09-23
**Estado:** Aceptada

## Contexto
El sistema de gestión de la biblioteca necesita llevar un registro exacto de los libros en catálogo, los ejemplares físicos disponibles, los usuarios registrados y el historial de préstamos. Es crítico garantizar que no haya inconsistencias (por ejemplo, registrar el préstamo de un ejemplar que ya se encuentra prestado a otro usuario) y que se puedan consultar rápidamente las fechas de vencimiento y multas.

## Decisión
Se implementará una base de datos relacional estructurada y normalizada que separe el Catálogo (metadatos del libro como título, autor, ISBN), los Ejemplares (las copias físicas específicas) y las Transacciones (préstamos y devoluciones). Se usarán llaves foráneas para mantener la integridad referencial entre estas entidades.

## Consecuencias
* **Positivas:** 
  - Evita la duplicidad de datos en el registro de los libros.
  - Permite saber exactamente qué copia física tiene cada usuario y su estado (disponible, prestado, extraviado).
  - Facilita la creación de consultas complejas (ej. "libros más prestados este mes" o "usuarios con devoluciones atrasadas").
* **Negativas / Riesgos:** 
  - Requiere un diseño de base de datos más estricto desde el inicio.
  - Las consultas de búsqueda (como buscar por título o autor) requerirán usar múltiples `JOIN`, lo que podría requerir optimización e índices adecuados para no afectar el rendimiento.

  ---

# Actualización de Decisiones - Versión 1.1

**Fecha:** 2026-09-23  
**Estado:** Aceptada

## 1. Sistema de Roles y Permisos
* **Decisión:** Se implementa un control de acceso basado en roles (*RBAC*) divididos en: **Administrador**, **Bibliotecario** y **Lector / Estudiante**.
* **Motivo:** Garantizar que los lectores solo puedan consultar disponibilidad y su historial personal, restringiendo el alta de libros y la gestión de multas únicamente al personal autorizado.

## 2. Identificación de Ejemplares por Código de Barras
* **Decisión:** Cada ejemplar físico tendrá asignado un código de barras (formato Code 128) generado a partir de su ID único.
* **Motivo:** Agilizar la entrega y recepción de libros en la barra de atención mediante lectores ópticos, evitando errores al digitar IDs manualmente.

## 3. Control de Multas y Notificaciones
* **Decisión:** Se establece una rutina de verificación diaria para marcar préstamos vencidos, calcular multas automáticas según los días de retraso y deshabilitar temporalmente las peticiones de usuarios morosos.
* **Motivo:** Mantener el control del inventario y automatizar las sanciones sin requerir revisión manual del bibliotecario.