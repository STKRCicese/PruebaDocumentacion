# Gestión de Riesgos

> **Versión:** 0.1  
> **Fecha:** AAAA-MM-DD  
> **Responsable:** [Nombre]  
> **Estado:** Borrador / En revisión / Aprobado

---

## 1. Propósito

Este documento registra los riesgos técnicos del proyecto y, cuando aplica (dispositivo médico o de salud), los riesgos para el usuario o paciente. Se mantiene vivo durante todo el proyecto: cada cambio significativo debe revisarse contra esta lista.

> **Nota regulatoria:** Para una ruta COFEPRIS, este documento es la base para construir el análisis de riesgos formal conforme a ISO 14971. Mantenerlo desde temprano facilita ese proceso.

---

## 2. Leyenda y Convenciones

### 2.1 Formato de ID

`RG-[Tipo]-[Número]`

- `RG-TEC` — Riesgo técnico (proyecto / desarrollo)
- `RG-USR` — Riesgo para el usuario / paciente (seguridad del producto)
- `RG-REG` — Riesgo regulatorio / de cumplimiento
- `RG-PRY` — Riesgo de proyecto (plazos, recursos)

### 2.2 Probabilidad

| Nivel           | Descripción                           |
| --------------- | ------------------------------------- |
| 1 – Remota      | Muy improbable que ocurra             |
| 2 – Baja        | Poco probable en condiciones normales |
| 3 – Media       | Puede ocurrir ocasionalmente          |
| 4 – Alta        | Probable que ocurra                   |
| 5 – Casi segura | Ocurrirá si no se actúa               |

### 2.3 Severidad / Impacto

| Nivel              | Técnico / Proyecto                            | Para usuario/paciente            |
| ------------------ | --------------------------------------------- | -------------------------------- |
| 1 – Insignificante | Retraso < 1 semana, sin impacto al entregable | Sin daño al usuario              |
| 2 – Menor          | Retraso leve, ajuste menor                    | Molestia leve, sin daño          |
| 3 – Moderado       | Retraso significativo o rediseño parcial      | Daño leve reversible             |
| 4 – Mayor          | Falla de un subsistema, retraso > 1 mes       | Daño grave reversible            |
| 5 – Catastrófico   | Falla total del proyecto o producto           | Daño grave irreversible o muerte |

### 2.4 Nivel de Riesgo (Probabilidad × Impacto)

| Puntuación | Nivel      | Acción requerida            |
| ---------- | ---------- | --------------------------- |
| 1–4        | 🟢 Bajo    | Monitorear                  |
| 5–9        | 🟡 Medio   | Mitigar en siguiente sprint |
| 10–16      | 🟠 Alto    | Mitigar con prioridad       |
| 17–25      | 🔴 Crítico | Acción inmediata, escalar   |

---

## 3. Riesgos Técnicos (RG-TEC)

| ID         | Descripción del riesgo                                               | Causa posible                                                  | Consecuencia                                           | P   | I   | Nivel    | Mitigación                                                      | Cómo se verifica                                       | Estado  | Issues |
| ---------- | -------------------------------------------------------------------- | -------------------------------------------------------------- | ------------------------------------------------------ | --- | --- | -------- | --------------------------------------------------------------- | ------------------------------------------------------ | ------- | ------ |
| RG-TEC-001 | El sensor no cumple la precisión requerida en el rango de operación. | Limitaciones del datasheet no validadas en condiciones reales. | Requisito RS-FW-001 incumplido; rediseño HW necesario. | 3   | 4   | 🟠 Alto  | Prueba de caracterización temprana del sensor (TP-UNIT-FW-001). | Resultados de TP-UNIT-FW-001 dentro de especificación. | Abierto |
| RG-TEC-002 | Interferencia EMI afecta comunicación inalámbrica.                   | Layout de PCB no optimizado para RF.                           | Pérdida de paquetes, sistema no confiable.             | 2   | 3   | 🟡 Medio | Revisión de layout por experto, prueba de coexistencia.         | Prueba de coexistencia a 10 m con < 1% pérdida.        | Abierto |
| RG-TEC-003 |                                                                      |                                                                |                                                        |     |     |          |                                                                 |                                                        |         |        |

---

## 4. Riesgos para el Usuario / Paciente (RG-USR)

> Completar esta sección si el producto tiene uso médico, clínico o de salud.  
> Referencia: ISO 14971 — Análisis de riesgos para dispositivos médicos.

| ID         | Situación peligrosa                                                                 | Secuencia de eventos                                        | Daño potencial                               | P   | I   | Nivel   | Mitigación                                                                         | Verificación                                                | Estado  |
| ---------- | ----------------------------------------------------------------------------------- | ----------------------------------------------------------- | -------------------------------------------- | --- | --- | ------- | ---------------------------------------------------------------------------------- | ----------------------------------------------------------- | ------- |
| RG-USR-001 | El sistema muestra una medición errónea que el clínico usa para tomar una decisión. | Fallo de sensor → dato incorrecto → diagnóstico equivocado. | Daño al paciente por tratamiento inadecuado. | 2   | 4   | 🟠 Alto | Alarma de fuera de rango; segunda fuente de verificación; advertencia en etiqueta. | TP-VAL-001: verificar detección de lecturas fuera de rango. | Abierto |

---

## 5. Riesgos Regulatorios (RG-REG)

| ID         | Riesgo                                                  | Causa                                | Consecuencia                            | P   | I   | Nivel   | Mitigación                                    | Estado  |
| ---------- | ------------------------------------------------------- | ------------------------------------ | --------------------------------------- | --- | --- | ------- | --------------------------------------------- | ------- |
| RG-REG-001 | Clasificación incorrecta del dispositivo ante COFEPRIS. | Interpretación errónea de normativa. | Registro denegado o proceso reiniciado. | 2   | 5   | 🟠 Alto | Consultar asesor regulatorio antes del TRL 5. | Abierto |

---

## 6. Riesgos de Proyecto (RG-PRY)

| ID         | Riesgo                                                               | Causa                        | Consecuencia                             | P   | I   | Nivel    | Mitigación                                                      | Estado  |
| ---------- | -------------------------------------------------------------------- | ---------------------------- | ---------------------------------------- | --- | --- | -------- | --------------------------------------------------------------- | ------- |
| RG-PRY-001 | Escasez o tiempo de entrega largo de componentes electrónicos clave. | Cadena de suministro global. | Retraso en fabricación del prototipo.    | 3   | 3   | 🟡 Medio | Identificar componentes alternativos; ordenar stock anticipado. | Abierto |
| RG-PRY-002 | Rotación de personal en el equipo de desarrollo.                     |                              | Pérdida de conocimiento tácito, retraso. | 2   | 3   | 🟡 Medio | Documentar decisiones de diseño y arquitectura.                 | Abierto |

---

## 7. Resumen del Estado de Riesgos

| Nivel      | Cantidad | Abiertos | Cerrados |
| ---------- | -------- | -------- | -------- |
| 🔴 Crítico | 0        | 0        | 0        |
| 🟠 Alto    |          |          |          |
| 🟡 Medio   |          |          |          |
| 🟢 Bajo    |          |          |          |
| **Total**  |          |          |          |

---

## 8. Revisión Periódica de Riesgos

| Fecha      | Evento que motivó la revisión  | Cambios realizados    | Responsable |
| ---------- | ------------------------------ | --------------------- | ----------- |
| AAAA-MM-DD | Inicio del proyecto            | Creación del registro |             |
|            | Cambio en arquitectura HW v0.2 |                       |             |

> **Política:** Revisar esta lista en cada cambio significativo de diseño, al inicio de cada nueva campaña de pruebas y antes de cada entregable de convocatoria.

---

## 9. Historial de Cambios

| Versión | Fecha      | Autor | Cambios          |
| ------- | ---------- | ----- | ---------------- |
| 0.1     | AAAA-MM-DD |       | Creación inicial |
