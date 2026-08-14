# Especificación de Requisitos (SRS) - Sistema de Inventario

## 1. Alcance y Fronteras del Software
El propósito de este proyecto es desplegar una plataforma web orientada al control estricto de existencias e inventario para tiendas locales.

### Incluido en el alcance:
- Módulo de gestión y registro de productos en la plataforma web.
- Control en tiempo real del stock de mercancías.
- Módulo de alertas automáticas para notificación de stock bajo (umbral mínimo parametrizable).
- Validación estricta de códigos de barra bajo la normativa de El Salvador (estándar EAN-13, prefijo 741).

### Fuera del alcance (Fronteras):
- Procesamiento directo de pagos o integración con pasarelas de pago externas.
- Facturación electrónica gubernamental o integración directa con Hacienda.
- Módulos de gestión de personal / nómina de empleados.

---

## 2. Restricciones Tecnológicas Obligatorias
- **Base de Datos:** Motor PostgreSQL (versión 13 o superior), preinstalado y configurado en el servidor del cliente.
- **Entorno de Ejecución:** Aplicación accesible desde navegadores web modernos (Google Chrome v100+, Mozilla Firefox v100+, Microsoft Edge).
- **Validación Regional:** Los algoritmos de entrada de producto deben validar que los códigos de barra cumplan con el prefijo asignado a El Salvador (741) y la longitud exacta de 13 dígitos numéricos.

---

## 3. Actores del Sistema
- **Administrador de Inventario:** Usuario con permisos de lectura, escritura y eliminación sobre el catálogo de productos y rangos de stock.
- **Encargado de Caja / Bodega:** Usuario con permisos de solo lectura y actualización rápida de entradas/salidas de existencias.

---

## 4. Requisitos Funcionales y Historias de Usuario (BDD / Gherkin)

### Módulo de Registro y Control de Inventario

#### US-01: Registro de Producto con Código de Barras de El Salvador
- **COMO:** Administrador de Inventario.
- **QUIERO:** Registrar un nuevo producto ingresando su código de barras.
- **PARA:** Mantener organizado el catálogo local del establecimiento.
- **CRITERIOS DE ACEPTACIÓN:**
  - **Dado que** el usuario ingresa un código de barras de 13 dígitos numéricos que comience con el prefijo `741`.
  - **Cuando** presione el botón de "Guardar Producto".
  - **Entonces** el sistema guardará el registro exitosamente en la base de datos PostgreSQL.
  - **Dado que** el usuario ingresa un código que no comience por `741` o no posea 13 dígitos.
  - **Entonces** el sistema bloqueará el guardado y desplegará el mensaje de error: `"El código de barras debe ser un EAN-13 válido de El Salvador (Prefijo 741)"`.

#### US-02: Alerta de Stock Bajo
- **COMO:** Encargado de Bodega.
- **QUIERO:** Recibir una notificación visual cuando un producto alcance las 5 unidades o menos.
- **PARA:** Realizar el reabastecimiento antes de que el producto se agote por completo.
- **CRITERIOS DE ACEPTACIÓN:**
  - **Dado que** la cantidad en stock de un producto disminuye a un valor menor o igual a 5 unidades.
  - **Entonces** el sistema resaltará la fila del producto en color rojo e indicará el estado `"Stock Crítico"`.

---

## 5. Requisitos de Calidad y No Funcionales (IEEE 830 - Cero Ambigüedad)
- **NFR-01 (Rendimiento):** El sistema deberá ejecutar las consultas de búsqueda de productos por código de barras y renderizar el resultado en pantalla en un tiempo inferior a 1.2 segundos bajo una carga de hasta 50 usuarios concurrentes.
- **NFR-02 (Disponibilidad):** La plataforma web mantendrá una disponibilidad operativa del 99.5% durante las horas hábiles del comercio (07:00 AM a 07:00 PM).

---

## 6. Matriz de Trazabilidad de Requisitos

| ID Requisito | Historia de Usuario | Componente Código | ID Test de QA |
| :--- | :--- | :--- | :--- |
| **REQ-01** | US-01 (Registro EAN-13 SV) | `productController.js` | TC-101 (Validación Código 741) |
| **REQ-02** | US-02 (Alerta Stock) | `stockAlert.service.js` | TC-202 (Notificación Umbral Bajo) |