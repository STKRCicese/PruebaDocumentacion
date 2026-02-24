# Plan de Pruebas

> **Versión:** 0.1  
> **Fecha:** AAAA-MM-DD  
> **Responsable:** [Nombre]  
> **Estado:** Borrador / En revisión / Aprobado

---

## 1. Propósito

Este documento define qué se va a probar, cómo, bajo qué condiciones y cuáles son los criterios de aceptación. Los resultados se registran en `08_pruebas_resultados.md`.

---

## 2. Alcance de las Pruebas

### 2.1 Incluye

- ...
- ...

### 2.2 Excluye

- ...

---

## 3. Tipos de Prueba

| Tipo                      | Descripción                                                      | Responsable |
| ------------------------- | ---------------------------------------------------------------- | ----------- |
| Prueba unitaria           | Verificación de módulos individuales de FW/SW                    |             |
| Prueba de integración     | Verificación de interfaces entre módulos                         |             |
| Prueba de sistema         | Verificación del sistema completo en condiciones nominales       |             |
| Prueba de validación      | Demostración de que el sistema cumple los requisitos de usuario  |             |
| Prueba de estrés / límite | Operación fuera de condiciones nominales para verificar robustez |             |
| Prueba ambiental          | Temperatura, humedad, vibración, etc.                            |             |
| Prueba de seguridad       | Verificación de aspectos de seguridad (si aplica para COFEPRIS)  |             |

---

## 4. Entorno de Prueba

| Elemento                   | Descripción                                                            |
| -------------------------- | ---------------------------------------------------------------------- |
| Hardware bajo prueba (DUT) | Versión de PCB: · Versión de FW:                                       |
| Instrumentación            | Osciloscopio, analizador lógico, multímetro, fuente de alimentación, … |
| Software de prueba         | Scripts en: `/software/tests/`                                         |
| Entorno de referencia      | Laboratorio de electrónica / entorno clínico simulado / …              |
| Estándar de referencia     | Norma / instrumento de referencia calibrado                            |

---

## 5. Catálogo de Casos de Prueba

### 5.1 Convenciones de ID

Formato: `TP-[Tipo]-[Módulo]-[Número]`  
Ejemplo: `TP-INT-HW-001` (Prueba de integración, Hardware, caso 001)

| Tipos             | Módulos                 |
| ----------------- | ----------------------- |
| `UNIT` Unitaria   | `HW`, `FW`, `SW`, `SYS` |
| `INT` Integración |                         |
| `SYS` Sistema     |                         |
| `VAL` Validación  |                         |
| `AMB` Ambiental   |                         |
| `SEG` Seguridad   |                         |

---

### 5.2 Pruebas de Hardware

| ID             | Nombre                      | Req. relacionado | Precondiciones                   | Pasos                                                                | Criterio de aceptación                         | Prioridad |
| -------------- | --------------------------- | ---------------- | -------------------------------- | -------------------------------------------------------------------- | ---------------------------------------------- | --------- |
| TP-UNIT-HW-001 | Caracterización sensor T/RH | RS-HW-001        | PCB ensamblada, cámara climática | 1. Colocar DUT en cámara. 2. Barrer T 0–50 °C. 3. Registrar lecturas | Resolución observada ≤ 0.1 °C en todo el rango | Alta      |

---

### 5.3 Pruebas de Firmware

| ID            | Nombre                           | Req. relacionado | Precondiciones                  | Pasos                                                                                | Criterio de aceptación                         | Prioridad |
| ------------- | -------------------------------- | ---------------- | ------------------------------- | ------------------------------------------------------------------------------------ | ---------------------------------------------- | --------- |
| TP-INT-FW-001 | Ciclo adquisición–envío de datos | RS-FW-001        | FW flasheado, backend corriendo | 1. Encender dispositivo. 2. Capturar tráfico. 3. Verificar intervalos entre mensajes | Intervalo entre mensajes ≤ 60 s durante 1 hora | Alta      |

---

### 5.4 Pruebas de Software

| ID            | Nombre                        | Req. relacionado      | Precondiciones            | Pasos                                                                             | Criterio de aceptación                     | Prioridad |
| ------------- | ----------------------------- | --------------------- | ------------------------- | --------------------------------------------------------------------------------- | ------------------------------------------ | --------- |
| TP-SYS-SW-001 | Visualización en tiempo (lab) | RU-SYS-001, RS-SW-001 | Sistema completo operando | 1. Dispositivo enviando datos. 2. Abrir dashboard. 3. Observar T/RH durante 5 min | Actualización ≤ 60 s, sin errores de carga | Alta      |

---

### 5.5 Pruebas de Validación (nivel usuario)

| ID         | Nombre                  | Req. relacionado       | Escenario                                                               | Criterio de aceptación                                        | Prioridad |
| ---------- | ----------------------- | ---------------------- | ----------------------------------------------------------------------- | ------------------------------------------------------------- | --------- |
| TP-VAL-001 | Escenario de uso típico | RU-SYS-001, RU-SYS-002 | Usuario completa tarea [X] de extremo a extremo sin asistencia técnica. | Tarea completada exitosamente en < min, sin errores críticos. | Alta      |

---

## 6. Criterios de Entrada y Salida de las Pruebas

### Criterios de Entrada (para iniciar la campaña de pruebas)

- [ ] Hardware ensamblado y verificado visualmente.
- [ ] Firmware compilado sin errores y flasheado.
- [ ] Entorno de prueba configurado y herramientas calibradas.
- [ ] Casos de prueba revisados y aprobados.

### Criterios de Salida (para declarar éxito)

- [ ] 100% de pruebas de prioridad Alta aprobadas.
- [ ] ≥ 80% de pruebas de prioridad Media aprobadas.
- [ ] Todos los fallos críticos documentados y con plan de acción.
- [ ] Resultados registrados en `08_pruebas_resultados.md`.

---

## 7. Gestión de Defectos

| Campo       | Descripción                                                              |
| ----------- | ------------------------------------------------------------------------ |
| Herramienta | GitHub Issues con etiqueta `bug`                                         |
| Severidad   | `Critical` / `Major` / `Minor` / `Cosmetic`                              |
| Ciclo       | Fallo detectado → Issue abierto → Corrección → Re-prueba → Issue cerrado |

---

## 8. Historial de Cambios

| Versión | Fecha      | Autor | Cambios          |
| ------- | ---------- | ----- | ---------------- |
| 0.1     | AAAA-MM-DD |       | Creación inicial |
