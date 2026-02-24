# Diseño de Firmware

> **Versión:** 0.1  
> **Fecha:** AAAA-MM-DD  
> **Responsable:** [Nombre]  
> **Estado:** Borrador / En revisión / Aprobado

---

## 1. Descripción General

<!-- Qué hace el firmware, en qué microcontrolador/plataforma corre, y cuál es su función dentro del sistema. -->

| Parámetro | Valor |
|-----------|-------|
| Plataforma / MCU | |
| Toolchain | GCC / LLVM / … |
| RTOS | FreeRTOS / Zephyr / Bare-metal / … |
| Lenguaje principal | C / C++ / MicroPython / Rust / … |
| Versión del SDK/HAL | |
| Repositorio / Ruta | `/firmware/` |

---

## 2. Versiones de Firmware

| Versión | Fecha | Cambios principales | Estado |
|---------|-------|---------------------|--------|
| v0.1.0 | | Primera iteración funcional | |
| v0.2.0 | | | |

> **Convención de versión:** `MAYOR.MENOR.PARCHE` — compatibilidad con hardware versión `vX.X`.

---

## 3. Arquitectura de Capas

```
┌──────────────────────────────────────┐
│     Capa de Aplicación               │  main.c / app.c / tasks
├──────────────────────────────────────┤
│     Servicios / Middleware            │  ej. gestor de datos, protocolo
├──────────────────────────────────────┤
│     Drivers / HAL                    │  ej. driver_sensor.c, hal_uart.c
├──────────────────────────────────────┤
│     RTOS / Scheduler                 │  FreeRTOS / bare-metal
├──────────────────────────────────────┤
│     Hardware (MCU + Periféricos)     │
└──────────────────────────────────────┘
```

---

## 4. Módulos de Firmware

| Módulo | Descripción | Archivo(s) | Dependencias | Responsable |
|--------|-------------|------------|--------------|-------------|
| Inicialización | Boot y configuración inicial | `main.c`, `init.c` | HAL | |
| Driver Sensor | Lectura y configuración del sensor | `driver_sensor.c/.h` | I2C/SPI HAL | |
| Gestor de datos | Procesamiento y almacenamiento de mediciones | `data_manager.c/.h` | | |
| Comunicación | Protocolo de envío/recepción | `comm.c/.h` | UART/BLE/… | |
| Gestión de energía | Modos de bajo consumo | `power_mgr.c/.h` | | |
| | | | | |

---

## 5. Tareas / Hilos (si usa RTOS)

| Tarea | Prioridad | Stack (bytes) | Periodo / Evento | Descripción |
|-------|-----------|--------------|-----------------|-------------|
| task_sensor | Alta | | Cada ms | Lee sensor y almacena en buffer |
| task_comm | Media | | Evento / Cada s | Envía datos al exterior |
| task_watchdog | Alta | | Cada s | Alimenta watchdog |

---

## 6. Configuración y Parámetros del Sistema

| Parámetro | Define / Variable | Valor por defecto | Descripción |
|-----------|-------------------|-------------------|-------------|
| `SAMPLE_RATE_HZ` | `#define` | `100` | Frecuencia de muestreo del sensor |
| `TX_BUFFER_SIZE` | `#define` | `256` | Tamaño del buffer de transmisión (bytes) |
| `SLEEP_TIMEOUT_MS` | `#define` | `5000` | Tiempo de inactividad antes de entrar a sleep |

---

## 7. Estándar de Código

### 7.1 Convenciones de Nomenclatura

| Elemento | Convención | Ejemplo |
|----------|------------|---------|
| Funciones | `snake_case` con prefijo de módulo | `sensor_read_temperature()` |
| Variables globales | `g_snake_case` | `g_sample_buffer` |
| Constantes / Defines | `UPPER_SNAKE_CASE` | `MAX_RETRY_COUNT` |
| Tipos / Structs | `PascalCase_t` | `SensorData_t` |

### 7.2 Comentarios y Documentación (Doxygen)

Todas las funciones públicas deben documentarse con el siguiente formato:

```c
/**
 * @brief   Lee la temperatura del sensor.
 *
 * @param   raw_value  Puntero donde se almacenará la lectura cruda del ADC.
 * @return  true si la lectura fue exitosa, false en caso de error.
 *
 * @note    Debe llamarse después de sensor_init().
 */
bool sensor_read_temperature(uint16_t *raw_value);
```

### 7.3 Manejo de Errores

- Todas las funciones deben verificar el retorno de las llamadas a hardware.
- Los errores se reportan mediante: `[código de retorno / log / callback / …]`.
- No usar `assert()` en producción sin manejo explícito.

---

## 8. Interfaces de Firmware

### 8.1 Entradas al Sistema

| Entrada | Tipo | Descripción | Pin / Interfaz |
|---------|------|-------------|----------------|
| | | | |

### 8.2 Salidas del Sistema

| Salida | Tipo | Descripción | Pin / Interfaz |
|--------|------|-------------|----------------|
| | | | |

### 8.3 Protocolo de Comunicación

<!-- Describir formato de tramas, comandos, respuestas. -->

| Campo | Longitud | Descripción |
|-------|----------|-------------|
| Header | 1 byte | `0xAA` |
| Comando | 1 byte | Ver tabla de comandos |
| Longitud | 2 bytes | Longitud del payload |
| Payload | N bytes | Datos |
| Checksum | 1 byte | XOR de todos los bytes anteriores |

---

## 9. Decisiones de Diseño

| # | Decisión | Alternativas | Razón | Fecha |
|---|----------|--------------|-------|-------|
| 1 | Uso de FreeRTOS vs bare-metal | Bare-metal, ThreadX | | |
| 2 | | | | |

---

## 10. Restricciones y Notas

- Memoria Flash disponible: KB · Uso actual: KB (%)
- RAM disponible: KB · Uso actual: KB (%)
- Restricciones de tiempo real: ...
- ...

---

## 11. Historial de Cambios

| Versión | Fecha | Autor | Cambios | Motivo |
|---------|-------|-------|---------|--------|
| 0.1 | AAAA-MM-DD | | Creación inicial | |
