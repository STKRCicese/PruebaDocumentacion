# Resultados de Pruebas

> **Versión:** 0.1  
> **Fecha:** AAAA-MM-DD  
> **Responsable:** [Nombre]  
> **Estado:** En progreso / Completado

---

## 1. Resumen Ejecutivo de la Campaña

| Campo | Detalle |
|-------|---------|
| Campaña # | 001 |
| Versión HW bajo prueba | |
| Versión FW bajo prueba | |
| Versión SW bajo prueba | |
| Fecha de inicio | |
| Fecha de cierre | |
| Responsable de pruebas | |
| Entorno | |

### Resultado Global

| Total | Aprobadas ✅ | Fallidas ❌ | Bloqueadas ⛔ | Sin ejecutar ⬜ |
|-------|-------------|------------|--------------|----------------|
| | | | | |

---

## 2. Resultados por Caso de Prueba

> Estados: ✅ Aprobado · ❌ Fallido · ⛔ Bloqueado · ⬜ Sin ejecutar · ↩️ Re-ejecutado

| ID | Nombre | Resultado | Fecha | Ejecutado por | Evidencia | Defecto (Issue #) | Notas |
|----|--------|-----------|-------|---------------|-----------|-------------------|-------|
| TP-UNIT-HW-001 | Verificación de alimentación | ✅ | | | [foto/log] | — | |
| TP-UNIT-HW-002 | Comunicación I2C | ❌ | | | [captura analizador] | #23 | Error intermitente con sensor a 400 kHz |
| TP-UNIT-FW-001 | Lectura de sensor | ✅ | | | [log serial] | — | |
| TP-VAL-001 | Escenario de uso típico | ⬜ | | | — | — | Pendiente prototipo v0.2 |

---

## 3. Detalle de Fallos

### Fallo: TP-UNIT-HW-002 — Comunicación I2C

**Descripción del fallo:**  
<!-- Qué ocurrió, cuándo, bajo qué condiciones. -->

**Datos / Evidencia:**  
<!-- Links a capturas, logs, fotos. -->

**Análisis de causa raíz:**  
<!-- Por qué ocurrió. -->

**Acción correctiva:**  
<!-- Qué se cambió o qué se va a cambiar. -->

**Issue relacionado:** #23  
**Estado:** Abierto / Cerrado  
**Re-prueba requerida:** Sí / No

---
<!-- Repetir sección 3 para cada fallo -->

---

## 4. Métricas de la Campaña

| Métrica | Valor |
|---------|-------|
| Cobertura de requisitos probados | % |
| Tasa de éxito (aprobadas / ejecutadas) | % |
| Densidad de defectos | defectos / caso de prueba |
| Defectos críticos abiertos | |
| Defectos cerrados en esta campaña | |

---

## 5. Conclusiones y Recomendaciones

<!-- Síntesis del estado de calidad del sistema basada en los resultados. ¿Está listo para el siguiente TRL? ¿Qué debe resolverse antes? -->

### ¿El sistema cumple los criterios de salida?

- [ ] 100% pruebas Alta aprobadas
- [ ] ≥ 80% pruebas Media aprobadas  
- [ ] Defectos críticos cerrados o con plan de acción aceptado

**Veredicto:** Aprobado para siguiente fase / Requiere correcciones y re-prueba

### Acciones antes de la siguiente campaña

| Acción | Responsable | Fecha límite | Issue |
|--------|-------------|--------------|-------|
| | | | |

---

## 6. Historial de Campañas

| Campaña | Versiones probadas | Fecha | Resultado global |
|---------|-------------------|-------|-----------------|
| 001 | HW v0.1, FW v0.1.0 | | |

---

## 7. Historial de Cambios del Documento

| Versión | Fecha | Autor | Cambios |
|---------|-------|-------|---------|
| 0.1 | AAAA-MM-DD | | Creación inicial |
