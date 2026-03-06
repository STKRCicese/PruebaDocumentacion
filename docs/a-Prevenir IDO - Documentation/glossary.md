---
sidebar_position: 9
---

# Glossary

> **Last Updated:** 2026-02-22
> **Document Version:** 1.0

---

## Table of Contents

1. [Domain Terms (Medical/Health)](#domain-terms-medicalhealth)
2. [System Terms (A-Prevenir Specific)](#system-terms-a-prevenir-specific)
3. [Technical Terms](#technical-terms)
4. [Acronyms](#acronyms)
5. [Spanish-English Translation Guide](#spanish-english-translation-guide)

---

## Domain Terms (Medical/Health)

### A-C

**Baumanómetro (Blood Pressure Monitor)**
: A medical device used to measure blood pressure. Measures systolic and diastolic pressure in mmHg (millimeters of mercury).

**Báscula (Scale)**
: A device for measuring body weight, typically in kilograms. Some models also measure body composition.

**Cinta Digital (Digital Measuring Tape)**
: An electronic tape measure used to assess waist circumference for cardiovascular risk evaluation.

**Colesterol (Cholesterol)**
: A waxy substance found in blood. Measured in mg/dL. Includes:
- **Total Cholesterol (TC)**: Sum of all cholesterol types
- **HDL**: "Good" cholesterol
- **LDL**: "Bad" cholesterol
- **TC/HDL Ratio**: Risk indicator (lower is better)

**Consulta (Consultation)**
: A telemedicine session between a patient and a physician conducted via video call.

**CURP (Clave Única de Registro de Población)**
: A unique 18-character alphanumeric identifier assigned to Mexican citizens. Used as primary patient identifier in A-Prevenir.

### D-F

**Diastólica (Diastolic)**
: The lower number in a blood pressure reading. Represents pressure when the heart rests between beats. Normal: < 80 mmHg.

**ECG/EKG (Electrocardiogram)**
: A test that measures electrical activity of the heart. Used to detect heart rhythm abnormalities.

**Estadímetro (Stadiometer)**
: A device for measuring standing height, typically in centimeters.

**FINDRISC (Finnish Diabetes Risk Score)**
: A standardized questionnaire to assess 10-year risk of developing Type 2 diabetes. Score ranges from 0 to 26:
- 0-7: Low risk (~1%)
- 8-11: Slightly elevated (~4%)
- 12-14: Moderate (~17%)
- 15-20: High (~33%)
- >20: Very high (~50%)

**Folio**
: A registration number assigned to patients in the A-Prevenir system.

### G-L

**Glucómetro (Glucometer)**
: A device for measuring blood glucose levels. Uses a small blood sample from a finger prick. Results in mg/dL.

**Glucosa (Glucose)**
: Blood sugar level. Normal fasting glucose: 70-100 mg/dL.

**HDL (High-Density Lipoprotein)**
: "Good" cholesterol that helps remove other forms of cholesterol from the bloodstream. Higher is better. Target: >40 mg/dL (men), >50 mg/dL (women).

**IMC (Índice de Masa Corporal) / BMI (Body Mass Index)**
: A measure of body fat based on height and weight. Formula: weight(kg) / height(m)². Categories:
- < 18.5: Underweight
- 18.5-24.9: Normal
- 25-29.9: Overweight
- ≥30: Obese

**LDL (Low-Density Lipoprotein)**
: "Bad" cholesterol that can build up in artery walls. Lower is better. Target: < 100 mg/dL.

### M-P

**Medición (Measurement)**
: A single reading from a medical device during a patient session.

**Médico (Physician/Doctor)**
: A healthcare professional who provides consultations via the telemedicine system.

**Paciente (Patient)**
: A user of the A-Prevenir system who undergoes health measurements or consultations.

**Perímetro Abdominal (Waist Circumference)**
: Measurement around the waist at the level of the navel. Risk thresholds:
- Men: >102 cm = high risk
- Women: >88 cm = high risk

**Presión Arterial (Blood Pressure)**
: Force of blood against artery walls. Expressed as systolic/diastolic (e.g., 120/80 mmHg).

### R-Z

**Ritmo Cardíaco (Heart Rate)**
: Number of heartbeats per minute (bpm). Normal resting: 60-100 bpm.

**Signos Vitales (Vital Signs)**
: Basic physiological measurements including temperature, blood pressure, heart rate, and respiratory rate.

**Sistólica (Systolic)**
: The upper number in a blood pressure reading. Represents pressure when the heart beats. Normal: < 120 mmHg.

**Somatometría (Somatometry)**
: The measurement of body dimensions including height, weight, and circumferences.

**Temperatura (Temperature)**
: Body temperature measured in degrees Celsius. Normal: 36.1-37.2°C.

**Triglicéridos (Triglycerides)**
: A type of fat in the blood. Normal: < 150 mg/dL.

---

## System Terms (A-Prevenir Specific)

### Application Modes

**Cabina (Workstation Mode)**
: Full-screen mode designed for physician workstations. Provides additional administrative features.

**Kiosco (Kiosk Mode)**
: Standard mode (1024x768) designed for public self-service terminals with touch-friendly interface.

**Demo Mode**
: Testing mode that simulates device readings without requiring physical hardware connections.

### Components

**Controlador (Controller)**
: A JavaFX controller class that handles user interactions for a specific screen.

**Pantalla (Screen)**
: A user interface view in the application. Examples: PantallaLogin, PantallaServicios.

**Servicio (Service)**
: A background task class that performs asynchronous operations like email sending or data export.

**Vista (View)**
: The visual representation of a screen, defined in FXML files.

### Connection States

**Conectado con Peer (Connected to Peer)**
: State indicating active video call connection with a physician.

**Conectado con Servidor (Connected to Server)**
: State indicating successful connection to the SMP server.

**Conectando (Connecting)**
: Transitional state while establishing a connection.

**Desconectado (Disconnected)**
: State indicating no active network connection.

### Data Entities

**DispositivosHabilitadosData**
: Configuration object storing which medical devices are enabled in the current installation.

**Medico**
: Data model representing a physician in the system.

**Medicion**
: Data model representing a set of measurements taken during a patient session.

**PruebaFindrisc**
: Data model storing results of a FINDRISC diabetes risk assessment.

**UserData**
: Singleton class that holds all session data for the current patient.

### Workflow Terms

**Exportar (Export)**
: The process of generating CSV or Excel files from stored measurement data.

**Historial (History)**
: Patient's previous measurement records.

**Registro (Registration)**
: Process of creating a new patient account in the system.

---

## Technical Terms

### Architecture

**Callback**
: A function passed to another function to be executed when an event occurs. Used extensively for device and network events.

**God Object (Anti-pattern)**
: A class that knows too much or does too much. UserData is identified as a god object in this codebase.

**HAL (Hardware Abstraction Layer)**
: Software layer that provides a consistent interface to hardware devices regardless of their specific implementations.

**MVC (Model-View-Controller)**
: Design pattern separating data (Model), presentation (View), and logic (Controller). Used in JavaFX with FXML.

**Singleton**
: Design pattern ensuring a class has only one instance. UserData implements this pattern.

### Communication

**Keep-Alive**
: Periodic messages sent to maintain an active network connection.

**P2P (Peer-to-Peer)**
: Direct connection between two endpoints (patient kiosk and physician workstation) without routing through a central server.

**SMP (Server Management Protocol)**
: Custom network protocol used for server communication. Version 2 (SMPv2) is current.

**SMPv2**
: Server Management Protocol version 2. Handles authentication, data synchronization, and peer connections.

### Development

**Ant (Apache Ant)**
: Build automation tool used to compile, package, and deploy the application.

**FXML**
: XML-based markup language for defining JavaFX user interfaces.

**JavaFX**
: Java framework for building desktop GUI applications. Used for all UI in A-Prevenir.

**JKS (Java KeyStore)**
: File format for storing cryptographic keys and certificates. Used for admin password storage.

**JNA (Java Native Access)**
: Framework for calling native shared libraries from Java. Used for device communication.

**Platform.runLater()**
: JavaFX method to execute code on the UI thread from a background thread.

### Database

**SQLite**
: Lightweight, file-based relational database used for local data storage.

**DAO (Data Access Object)**
: Design pattern for abstracting database operations. Used via external JAR.

### Devices

**FTDI**
: Future Technology Devices International. Manufacturer of USB-to-Serial chips used in medical devices.

**Serial Port**
: Communication interface using RS-232 protocol. Most medical devices use serial communication.

**Virtual COM Port**
: Software emulation of a serial port, typically created by USB-to-Serial adapters.

---

## Acronyms

| Acronym | Full Term | Description |
|---------|-----------|-------------|
| **BMI** | Body Mass Index | Weight-height ratio indicator |
| **BP** | Blood Pressure | Systolic/Diastolic measurement |
| **bpm** | Beats Per Minute | Heart rate unit |
| **CICESE** | Centro de Investigación Científica y de Educación Superior de Ensenada | Organization that developed A-Prevenir |
| **CURP** | Clave Única de Registro de Población | Mexican ID number |
| **CSV** | Comma-Separated Values | Data export format |
| **DAO** | Data Access Object | Database abstraction pattern |
| **DI** | Dependency Injection | Design pattern |
| **ECG/EKG** | Electrocardiogram | Heart electrical activity test |
| **FXML** | FX Markup Language | JavaFX UI definition format |
| **HAL** | Hardware Abstraction Layer | Device interface layer |
| **HDL** | High-Density Lipoprotein | "Good" cholesterol |
| **IMC** | Índice de Masa Corporal | Spanish for BMI |
| **JAR** | Java Archive | Java package format |
| **JDK** | Java Development Kit | Java development tools |
| **JKS** | Java KeyStore | Certificate storage format |
| **JNA** | Java Native Access | Native library access |
| **LDL** | Low-Density Lipoprotein | "Bad" cholesterol |
| **MVC** | Model-View-Controller | Design pattern |
| **P2P** | Peer-to-Peer | Direct connection |
| **SMP** | Server Management Protocol | Network protocol |
| **SQL** | Structured Query Language | Database query language |
| **TC** | Total Cholesterol | Sum of all cholesterol |
| **TG** | Triglycerides | Blood fat type |
| **UI** | User Interface | Visual elements |
| **USB** | Universal Serial Bus | Connection standard |

---

## Spanish-English Translation Guide

### Common Code Terms

| Spanish | English | Context |
|---------|---------|---------|
| agregar | add | Methods: `agregarMedicion()` |
| archivo | file | File operations |
| buscar | search | Query methods |
| cambiar | change | `cambiarVista()` = change view |
| cargando | loading | UI states |
| cerrar | close | `cerrarConexion()` = close connection |
| configuración | configuration | Settings |
| conectar | connect | Network operations |
| desconectar | disconnect | Network operations |
| dispositivo | device | Hardware |
| eliminar | delete | CRUD operations |
| enviar | send | `enviarEmail()` = send email |
| error | error | Same in both languages |
| exportar | export | Data export |
| fecha | date | Date fields |
| guardar | save | `guardarDatos()` = save data |
| habilitado | enabled | Configuration flags |
| iniciar | start/init | `iniciarSesion()` = start session |
| lista | list | Collections |
| llamada | call | Video call |
| medición | measurement | Health readings |
| mostrar | show | UI display |
| nombre | name | User fields |
| obtener | get | Getter methods |
| pantalla | screen | UI views |
| periférico | peripheral | Hardware device |
| prueba | test | FINDRISC test |
| registrar | register | User registration |
| reporte | report | Data reports |
| salir | exit | Application exit |
| seleccionar | select | User selection |
| terminar | end/finish | End operations |
| usuario | user | User/patient |
| valor | value | Data values |
| vista | view | UI views |

### UI Labels

| Spanish | English |
|---------|---------|
| Aceptar | Accept/OK |
| Anterior | Previous |
| Cancelar | Cancel |
| Confirmar | Confirm |
| Continuar | Continue |
| Finalizar | Finish |
| Guardar | Save |
| Inicio | Home/Start |
| Ingresar | Enter/Login |
| Siguiente | Next |
| Volver | Back/Return |

### Medical Terms in Code

| Spanish Variable | English Meaning |
|-----------------|-----------------|
| `temperatura` | temperature |
| `presionSistolica` | systolicPressure |
| `presionDiastolica` | diastolicPressure |
| `ritmoCardiaco` | heartRate |
| `peso` | weight |
| `altura` | height |
| `perimetroAbdominal` | waistCircumference |
| `glucosa` | glucose |
| `colesterol` | cholesterol |
| `trigliceridos` | triglycerides |

### Enum Values

| Spanish | English | Enum |
|---------|---------|------|
| `DISPONIBLE` | Available | MedicoEstadoEnum |
| `OCUPADO` | Busy | MedicoEstadoEnum |
| `NO_DISPONIBLE` | Not Available | MedicoEstadoEnum |
| `AUSENTE` | Absent | MedicoEstadoEnum |
| `NORMAL` | Normal | NivelSignoVitalEnum |
| `BAJO` | Low | NivelSignoVitalEnum |
| `ALTO` | High | NivelSignoVitalEnum |
| `MUY_ALTO` | Very High | NivelSignoVitalEnum |

---

## See Also

- [Architecture Overview](architecture.md) - System structure
- [API Overview](api-overview.md) - Interface definitions
- [Getting Started](getting-started.md) - Developer onboarding

---

*Document generated for A-Prevenir IDO architecture review*
