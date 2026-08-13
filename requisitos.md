# Módulo 3.4 - Especificación de Ingeniería de Requisitos
**Proyecto:** Aplicación de Gestión de Viajes para Choferes  

---

## 1. Requisitos Funcionales (RF)

| ID | Nombre | Descripción |
|---|---|---|
| **RF-01** | Visualización de viajes | El sistema debe permitir al chofer ver solicitudes de viajes cercanos en un mapa interactivo. |
| **RF-02** | Aceptación de viaje | El chofer debe poder aceptar o rechazar solicitudes de viaje recibidas. |
| **RF-03** | Cálculo de ruta optima | El sistema debe generar automáticamente la ruta más corta y rápida al confirmar un viaje. |
| **RF-04** | Historial de viajes | El chofer podrá consultar el historial de viajes completados y sus ganancias. |

---

## 2. Requisitos No Funcionales (RNF)

| ID | Categoría | Especificación / Criterio de Medición |
|---|---|---|
| **RNF-01** | **Seguridad** | Toda la comunicación entre la app y el servidor debe cifrarse mediante TLS/SSL y algoritmos de cifrado en tránsito y reposo. |
| **RNF-02** | **Rendimiento** | El sistema debe soportar hasta 1,000 choferes conectados simultáneamente con tiempos de respuesta menores a 2 segundos. |
| **RNF-03** | **Disponibilidad** | La actualización de la geolocalización (GPS) en el mapa del chofer debe tener una latencia máxima de 1 segundo en tiempo real. |

---

## 3. Historias de Usuario (US)

### 📌 US-01: Visualización de viajes cercanos en tiempo real
* **COMO:** Chofer activo en la plataforma
* **QUIERO:** Ver las solicitudes de viaje más cercanas a mi ubicación en tiempo real
* **PARA:** Aceptar servicios de forma rápida y reducir tiempos de espera

#### Criterios de Aceptación (Formato Gherkin)
```gherkin
Escenario: Mostrar solicitudes dentro del rango de cobertura
  Dado que el chofer ha iniciado sesión y su estado es "Disponible"
  Y tiene activada la ubicación GPS en su dispositivo
  Cuando existan solicitudes de pasajeros en un radio menor o igual a 5 km
  Entonces la aplicación debe listar las solicitudes disponibles en el mapa en menos de 1 segundo
  Y mostrar la dirección de origen y distancia estimada para cada viaje

Escenario: Sin solicitudes disponibles en el área
  Dado que el chofer se encuentra "Disponible"
  Cuando no haya solicitudes de pasajeros en un radio de 5 km
  Entonces la pantalla debe mostrar el mensaje "Buscando viajes cercanos..."

---

### 📌 US-02: Generación automática de ruta óptima
* **COMO:** Chofer de la aplicación
* **QUIERO:** Recibir la ruta más corta e instrucciones de navegación al aceptar un viaje
* **PARA:** Llegar rápido por el pasajero y consumir menos combustible

#### Criterios de Aceptación (Formato Gherkin)
```gherkin
Escenario: Cálculo e inicio de navegación tras aceptar viaje
  Dado que el chofer selecciona una solicitud de viaje activa
  Cuando presiona el botón "Aceptar Viaje"
  Entonces el sistema debe confirmar la asignación del viaje al pasajero
  Y trazar en el mapa la ruta más rápida considerando el tráfico en tiempo real
