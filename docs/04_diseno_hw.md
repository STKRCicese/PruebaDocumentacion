# Diseño de Hardware

> **Versión:** 0.1  
> **Fecha:** AAAA-MM-DD  
> **Responsable:** [Nombre]  
> **Estado:** Borrador / En revisión / Aprobado

---

## 1. Descripción General del Hardware

<!-- Qué hace el hardware, qué problema resuelve dentro del sistema, y cuáles son sus características principales. -->

---

## 2. Versiones de Hardware

| Versión | Fecha | Cambios principales | Estado |
|---------|-------|---------------------|--------|
| v0.1 | | Primera iteración / prueba de concepto | |
| v0.2 | | | |

---

## 3. Componentes Principales (BOM resumida)

> BOM completa en `/hardware/bom_vX.X.xlsx` o equivalente.

| # | Referencia | Descripción | Valor / PN | Fabricante | Cantidad | Notas |
|---|-----------|-------------|------------|------------|----------|-------|
| 1 | U1 | Microcontrolador | | | 1 | |
| 2 | U2 | Sensor de [parámetro] | | | 1 | |
| 3 | C1–C10 | Capacitor de desacople | 100 nF | | 10 | |

---

## 4. Esquemáticos

<!-- Insertar imagen del esquemático o enlace al archivo. -->

- Esquemático completo: [`/hardware/esquematico_vX.X.pdf`](/hardware/)
- Archivo editable: [`/hardware/esquematico_vX.X.kicad_sch`](/hardware/) _(o el formato de tu herramienta: Altium, Eagle, etc.)_

### 4.1 Subsistemas en el Esquemático

| Hoja / Sección | Descripción |
|----------------|-------------|
| Sheet 1 | Alimentación y regulación |
| Sheet 2 | MCU y periféricos |
| Sheet 3 | Sensores / Actuadores |
| Sheet 4 | Comunicación |

---

## 5. Diseño de PCB

- Archivo de PCB: [`/hardware/pcb_vX.X.kicad_pcb`](/hardware/)
- Gerbers: [`/hardware/gerbers_vX.X/`](/hardware/)
- Número de capas: 
- Dimensiones: mm x mm
- Espesor de pista mínimo: 
- Via mínimo: 

### 5.1 Consideraciones de Layout

<!-- Restricciones de routing, zonas de ground plane, separación de señales analógicas/digitales, etc. -->

- ...
- ...

---

## 6. Alimentación

| Rail | Tensión | Corriente máx. | Fuente | Regulador |
|------|---------|---------------|--------|-----------|
| VCC | 3.3 V | mA | | |
| VBAT | 3.7 V | mA | Batería Li-Po | — |
| VREF | | | | |

### 6.1 Cálculo de Consumo

| Modo de operación | Corriente estimada | Fuente del dato |
|-------------------|--------------------|-----------------|
| Activo (full operation) | mA | Datasheet / Medición |
| Bajo consumo (sleep) | µA | |
| Peak (transmisión) | mA | |

> **Autonomía estimada:** [X horas] con batería de [Y mAh] en modo [Z].

---

## 7. Interfaces de Hardware

| Interfaz | Conector | Señales | Notas |
|----------|----------|---------|-------|
| Programación / Debug | | SWDIO, SWCLK, GND, VCC | |
| UART / Serial | | TX, RX | |
| I2C | | SDA, SCL | Pull-ups: kΩ |
| SPI | | MOSI, MISO, SCK, CS | |
| GPIO externos | | | |

---

## 8. Decisiones de Diseño

| # | Decisión | Alternativas consideradas | Razón | Fecha |
|---|----------|--------------------------|-------|-------|
| 1 | Selección de MCU: [modelo] | [Opción A], [Opción B] | | |
| 2 | | | | |

---

## 9. Restricciones y Notas de Diseño

<!-- Restricciones eléctricas, térmicas, mecánicas, de costo, de disponibilidad de componentes, etc. -->

- ...
- ...

---

## 10. Pruebas de Aceptación de Hardware

> Ver también `07_pruebas_plan.md` para el plan completo.

| Prueba | Descripción | Criterio de aceptación | Resultado |
|--------|-------------|------------------------|-----------|
| Power-on | Verificar que todos los rails están dentro de tolerancia | ±5% del valor nominal | |
| Consumo en reposo | Medir corriente en modo sleep | < µA | |
| Comunicación | Verificar protocolos (I2C, SPI, UART) con analizador lógico | Sin errores en 1000 transacciones | |

---

## 11. Historial de Cambios

| Versión | Fecha | Autor | Cambios | Motivo |
|---------|-------|-------|---------|--------|
| 0.1 | AAAA-MM-DD | | Creación inicial | |
