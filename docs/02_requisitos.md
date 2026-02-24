# Requisitos del Sistema

> **Versión:** 0.1  
> **Fecha:** AAAA-MM-DD  
> **Responsable:** [Nombre]  
> **Estado:** Borrador / En revisión / Aprobado

---

## 1. Propósito y Alcance

Este documento concentra los requisitos de usuario, del sistema y regulatorios/de convocatoria en una única fuente de verdad versionada. Toda tarea de diseño, implementación o prueba debe poder trazarse a al menos un requisito aquí registrado.

---

## 2. Leyenda y Convenciones

| Campo         | Descripción                                                                                            |
| ------------- | ------------------------------------------------------------------------------------------------------ |
| **ID**        | Identificador único. Formato: `[Tipo]-[Módulo]-[Número]` — ej. `RU-SYS-001`, `RS-HW-003`, `RR-REG-001` |
| **Tipo**      | `RU` Requisito de Usuario · `RS` Requisito de Sistema · `RR` Regulatorio/Convocatoria                  |
| **Módulo**    | `SYS` Sistema · `HW` Hardware · `FW` Firmware · `SW` Software · `COM` Comunicación · `SEG` Seguridad   |
| **Prioridad** | `Alta` · `Media` · `Baja`                                                                              |
| **Estado**    | `Borrador` · `Aprobado` · `Implementado` · `Verificado` · `Obsoleto`                                   |
| **ID Riesgo** | Referencia a `10_riesgos.md` si aplica                                                                 |
| **ID Prueba** | Referencia a `07_pruebas_plan.md`                                                                      |

---

## 3. Requisitos de Usuario (RU)

> Qué necesita lograr el usuario con el sistema, independientemente de cómo se implemente.

| ID         | Descripción                                                                | Prioridad | Criterio de Aceptación                                     | Estado   | ID Riesgo  | ID Prueba     |
| ---------- | -------------------------------------------------------------------------- | --------- | ---------------------------------------------------------- | -------- | ---------- | ------------- |
| RU-SYS-001 | El usuario debe poder visualizar la temperatura y humedad actuales del lab | Alta      | En el dashboard se muestra T y RH actualizadas cada ≤ 60 s | Borrador | RG-TEC-001 | TP-SYS-SW-001 |

---

## 4. Requisitos del Sistema (RS)

> Cómo el sistema satisface los requisitos de usuario. Funcionales y no funcionales.

### 4.1 Funcionales

| ID        | Descripción                                                                                 | Módulo | Prioridad | Criterio de Aceptación                                              | Estado   | ID Riesgo  | ID Prueba      |
| --------- | ------------------------------------------------------------------------------------------- | ------ | --------- | ------------------------------------------------------------------- | -------- | ---------- | -------------- |
| RS-HW-001 | El dispositivo debe medir temperatura en el rango 0–50 °C con resolución de 0.1 °C          | HW     | Alta      | Pruebas de caracterización muestran resolución ≤ 0.1 °C en el rango | Borrador | RG-TEC-001 | TP-UNIT-HW-001 |
| RS-FW-001 | El firmware debe adquirir y enviar lecturas de T/RH al backend al menos cada 60 s           | FW     | Alta      | Logs muestran paquetes cada ≤ 60 s durante 1 h de prueba            | Borrador | RG-TEC-001 | TP-INT-FW-001  |
| RS-SW-001 | El backend debe almacenar y exponer vía API la última medición de T/RH para consulta rápida | SW     | Alta      | Endpoint `/api/measurements/latest` responde en < 1 s con datos     | Borrador | RG-TEC-001 | TP-UNIT-SW-001 |

### 4.2 No Funcionales

| ID            | Descripción                                                           | Módulo | Prioridad | Criterio de Aceptación                            | Estado   | ID Riesgo | ID Prueba |
| ------------- | --------------------------------------------------------------------- | ------ | --------- | ------------------------------------------------- | -------- | --------- | --------- |
| RS-SYS-NF-001 | El sistema debe operar dentro de un rango de temperatura de [X–Y °C]. | SYS    | Alta      | Prueba ambiental en rango especificado sin fallo. | Borrador | —         | —         |
| RS-SEG-001    |                                                                       | SEG    |           |                                                   | Borrador | —         | —         |

---

## 5. Requisitos Regulatorios y de Convocatoria (RR)

> Requisitos impuestos externamente por normas, leyes o términos de la convocatoria de financiamiento.

| ID          | Fuente (Norma/Convocatoria) | Descripción | Criterio de Cumplimiento | Estado   | Evidencia |
| ----------- | --------------------------- | ----------- | ------------------------ | -------- | --------- |
| RR-REG-001  | NOM-XXX / COFEPRIS          |             |                          | Borrador |           |
| RR-CONV-001 | Convocatoria [nombre]       |             |                          | Borrador |           |

---

## 6. Matriz de Trazabilidad (resumen)

> Esta sección se actualiza periódicamente para verificar cobertura.

| ID Req. Usuario | ID Req. Sistema que lo satisface | ID Prueba que lo verifica                    |
| --------------- | -------------------------------- | -------------------------------------------- |
| RU-SYS-001      | RS-HW-001, RS-FW-001, RS-SW-001  | TP-UNIT-HW-001, TP-INT-FW-001, TP-SYS-SW-001 |

---

## 7. Requisitos Pendientes de Definir

<!-- Usar esta sección como "parking lot" de requisitos identificados pero no formalizados aún. -->

- [ ] Definir requisitos de conectividad remota.
- [ ] Confirmar requisitos de clasificación COFEPRIS con asesor.
- [ ] ...

---

## 8. Historial de Cambios

| Versión | Fecha      | Autor | Cambios          |
| ------- | ---------- | ----- | ---------------- |
| 0.1     | AAAA-MM-DD |       | Creación inicial |
