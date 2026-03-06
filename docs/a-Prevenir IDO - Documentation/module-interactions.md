---
sidebar_position: 3
---

# Module Interactions

> **Last Updated:** 2026-02-22
> **Document Version:** 1.0
> **Related:** [Architecture Overview](architecture.md)

---

## Table of Contents

1. [Overview](#overview)
2. [Critical Workflows](#critical-workflows)
3. [Data Flow Diagrams](#data-flow-diagrams)
4. [Module Communication Matrix](#module-communication-matrix)
5. [Event System](#event-system)
6. [State Management](#state-management)

---

## Overview

A-Prevenir operates through a series of interconnected workflows that manage patient identification, health measurements, telemedicine consultations, and data export. This document traces the critical paths through the system.

### Communication Patterns Used

| Pattern | Usage | Modules Involved |
|---------|-------|------------------|
| **Direct Method Call** | UI → Business Logic | Controllers → UserData, Tools |
| **Singleton Access** | Global State | UserData.getInstance() |
| **Callback/Observer** | Network Events | SMPv2 → UserData |
| **JavaFX Platform.runLater** | Thread-safe UI | Background → UI Thread |
| **Service/Task** | Async Operations | Services → Background Threads |

---

## Critical Workflows

### Workflow 1: Patient Login & Authentication

```mermaid
sequenceDiagram
    participant User
    participant P1 as Pantalla1Controller
    participant Login as PantallaLoginController
    participant UD as UserData
    participant SMP as SMPv2 Client
    participant Server as SMP Server
    participant LocalDB as SQLite DB

    User->>P1: Touch screen / Activity detected
    P1->>P1: cancelIdleTimer()
    P1->>Tools: changeView(LOGIN)
    Tools->>Login: Initialize FXML

    User->>Login: Enter credentials (CURP/Folio)
    Login->>UD: setCredentials(curp, folio)

    alt Online Mode
        Login->>UD: iniciarInstanciaCliente(true)
        UD->>SMP: connect(serverIP, port)
        SMP->>Server: Authentication Request
        Server-->>SMP: Authentication Response
        SMP-->>UD: connectionStatusChanged()
        UD->>UD: updateConnectionState(CONECTADO_CON_SERVIDOR)
        UD-->>Login: Notify success via callback
    else Offline Mode
        Login->>LocalDB: queryLocalUser(curp)
        LocalDB-->>Login: User data or null
    end

    Login->>Tools: changeView(SERVICIOS)
```

**Entry Point:** `src/aPrevenir/Controladores/Pantalla1Controller.java`

**Key Code Paths:**
- Touch detection: `Pantalla1Controller.java:initialize()` - Sets up idle timer
- View transition: `Tools.java:changeView()` - Manages scene switching
- Authentication: `PantallaLoginController.java` - Credential validation
- Network init: `UserData.java:iniciarInstanciaCliente()` - SMPv2 client creation

---

### Workflow 2: Health Measurement Collection

```mermaid
sequenceDiagram
    participant User
    participant TD as PantallaTomarDatosController
    participant DCM as DispositivosConfigurationManager
    participant Periph as perifericos.* (modulos.jar)
    participant Device as Medical Device
    participant UD as UserData
    participant LocalDB as SQLite DB

    User->>TD: Select measurement type
    TD->>DCM: getDeviceConfig(deviceType)
    DCM-->>TD: DeviceConfiguration

    TD->>Periph: initializeDevice(config)
    Periph->>Device: Open serial port
    Device-->>Periph: Connection ACK

    TD->>TD: updateUI("Ready to measure")
    User->>TD: Click "Start Measurement"

    TD->>Periph: requestReading()
    Periph->>Device: Send command

    loop Waiting for reading
        Device-->>Periph: Data packet
        Periph-->>TD: onDataReceived(reading)
        TD->>TD: validateReading(reading)
    end

    TD->>UD: storeMeasurement(type, value)
    UD->>LocalDB: insertMedicion(medicion)

    TD->>TD: displayResult(value, normalRange)
    TD->>TD: checkForNextMeasurement()
```

**Entry Point:** `src/aPrevenir/Controladores/PantallaTomarDatosController.java`

**Supported Measurements:**

| Measurement | Device Class | Data Type |
|------------|--------------|-----------|
| Temperature | `TermometroPelicano` | Float (°C) |
| Blood Pressure | `baumanometroAD` | Int systolic/diastolic (mmHg) |
| Heart Rate | `baumanometroAD` | Int (bpm) |
| Weight | `basculaPelicano/basculaAD` | Float (kg) |
| Height | `estadimetro` | Float (cm) |
| Blood Glucose | `glucometro` | Int (mg/dL) |
| Waist Circumference | `CintaDigital` | Float (cm) |
| Lipid Panel | `CardioCheck` | Multiple values |

**Key Code Paths:**
- Device initialization: `PantallaTomarDatosController.java` - Device setup
- Configuration loading: `DispositivosConfigurationManager.java:loadConfig()`
- Measurement storage: `UserData.java` - Data aggregation
- Device communication: `perifericos.*` package in `modulos.jar`

---

### Workflow 3: Telemedicine Consultation

```mermaid
sequenceDiagram
    participant User
    participant EM as ElegirMedicoController
    participant UD as UserData
    participant SMP as SMPv2 Client
    participant Server as SMP Server
    participant LC as LlamadaController
    participant Doctor as Doctor Workstation
    participant GStreamer as GStreamer Pipeline

    User->>EM: View available doctors
    EM->>UD: getDoctorList()
    UD->>SMP: requestDoctorList()
    SMP->>Server: Query available physicians
    Server-->>SMP: Doctor list response
    SMP-->>UD: doctorListUpdated()
    UD-->>EM: Notify via callback
    EM->>EM: displayDoctorList(doctors)

    User->>EM: Select doctor
    EM->>UD: selectDoctor(medicoId)
    UD->>SMP: requestPeerConnection(medicoId)
    SMP->>Server: P2P connection request
    Server->>Doctor: Incoming call notification
    Doctor-->>Server: Accept call
    Server-->>SMP: P2P connection established
    SMP-->>UD: connectionStatusChanged(CONECTADO_CON_PEER)

    UD->>UD: updateConnectionState()
    UD-->>LC: Initialize call screen

    LC->>GStreamer: initializeVideoReceive()
    LC->>GStreamer: initializeAudioChannels()

    Doctor->>GStreamer: Stream video/audio
    GStreamer-->>LC: Display video frame

    loop During Consultation
        Doctor->>SMP: sendAdvice(recommendation)
        SMP-->>UD: adviceReceived(text)
        UD-->>LC: displayRecommendation(text)
    end

    User->>LC: End call
    LC->>UD: endCall()
    UD->>SMP: disconnectPeer()
```

**Entry Point:** `src/aPrevenir/Controladores/ElegirMedicoController.java`

**Key Code Paths:**
- Doctor listing: `ElegirMedicoController.java` - UI for physician selection
- P2P connection: `UserData.java:iniciarInstanciaCliente()` - Network management
- Video handling: `LlamadaController.java` - GStreamer integration
- Recommendations: `UserData.java:adviceReceived()` - Display medical advice

**Connection States:**

```
DESCONECTADO → CONECTANDO_CON_SERVIDOR → CONECTADO_CON_SERVIDOR
                                              ↓
                                    CONECTANDO_CON_PEER
                                              ↓
                                    CONECTADO_CON_PEER
```

---

### Workflow 4: FINDRISC Diabetes Risk Assessment

```mermaid
sequenceDiagram
    participant User
    participant FC as PantallaFINDRISCController
    participant UD as UserData
    participant LocalDB as SQLite DB
    participant Email as MedicionesEmailService

    User->>FC: Start FINDRISC questionnaire
    FC->>FC: loadQuestions()

    loop For each question (8 total)
        FC->>FC: displayQuestion(questionNum)
        User->>FC: Select answer
        FC->>FC: storeAnswer(questionNum, value)
        FC->>FC: calculatePartialScore()
    end

    FC->>FC: calculateFinalScore()
    FC->>FC: determineRiskLevel(score)

    Note over FC: Risk Levels:<br/>0-7: Low<br/>8-11: Moderate<br/>12-14: High<br/>15+: Very High

    FC->>UD: storeFINDRISCResult(score, riskLevel)
    UD->>LocalDB: insertPruebaFindrisc(result)

    FC->>FC: displayResults(score, riskLevel, recommendations)

    opt User requests email
        User->>FC: Send results via email
        FC->>Email: sendFINDRISCResults(userEmail, results)
        Email-->>FC: Confirmation
    end
```

**Entry Point:** `src/aPrevenir/Controladores/PantallaFINDRISCController.java`

**FINDRISC Scoring Factors:**
1. Age
2. BMI
3. Waist circumference
4. Physical activity
5. Vegetable/fruit consumption
6. Hypertension medication
7. High blood glucose history
8. Family diabetes history

---

### Workflow 5: Data Export & Reporting

```mermaid
sequenceDiagram
    participant Admin
    participant CC as PantallaConfiguracionController
    participant EDS as ExportarDBService
    participant LocalDB as SQLite DB
    participant FS as File System
    participant EES as ExportarEmailService
    participant SMTP as Email Server

    Admin->>CC: Login with admin credentials
    CC->>AdminLogin: validatePassword(password)
    AdminLogin-->>CC: Authentication result

    Admin->>CC: Select export type

    alt CSV Export
        CC->>EDS: new ExportarDBService(dateRange, type)
        EDS->>LocalDB: getMedicionesReporte(dateRange)
        LocalDB-->>EDS: List<Medicion>
        EDS->>FS: writeCSV(data, "exportar/mediciones_*.csv")
        FS-->>EDS: File path
        EDS-->>CC: Export complete
    else Excel Export
        CC->>EDS: exportToExcel(dateRange)
        EDS->>LocalDB: getMedicionesReporte(dateRange)
        EDS->>FS: writeXLS(data, "exportar/*.xls")
    else Email Report
        CC->>EES: new ExportarEmailService(data)
        EES->>EES: generateCharts(data)
        EES->>FS: savePNGCharts()
        EES->>SMTP: sendEmail(recipient, charts, data)
        SMTP-->>EES: Delivery confirmation
    end

    CC->>CC: displayExportResult(success, filePath)
```

**Entry Point:** `src/aPrevenir/Controladores/PantallaConfiguracionController.java`

**Export Outputs:**
- CSV files: `exportar/exportar_mediciones_*.csv`
- Excel files: `exportar/medicionesA_Prevenir.xls`
- Chart images: `exportar/*Chart.png`

---

## Data Flow Diagrams

### Patient Data Flow

```mermaid
flowchart LR
    subgraph Input
        UI[User Interface]
        Devices[Medical Devices]
        Server[SMP Server]
    end

    subgraph Processing
        Controllers[Controllers]
        UserData[UserData Singleton]
        Services[JavaFX Services]
    end

    subgraph Storage
        LocalDB[(SQLite DB)]
        Props[Properties Files]
        Logs[Log Files]
    end

    subgraph Output
        Display[Screen Display]
        Email[Email Reports]
        CSV[CSV/Excel Files]
        Print[Thermal Printer]
    end

    UI --> Controllers
    Devices --> Controllers
    Server --> UserData

    Controllers --> UserData
    UserData --> Services

    UserData --> LocalDB
    Controllers --> Props
    Controllers --> Logs

    UserData --> Display
    Services --> Email
    Services --> CSV
    Controllers --> Print
```

### Measurement Data Structure

```mermaid
classDiagram
    class Medicion {
        +int id
        +String pacienteId
        +DateTime fecha
        +float temperatura
        +int presionSistolica
        +int presionDiastolica
        +int ritmoCardiaco
        +float peso
        +float altura
        +float imc
        +int glucosa
        +float perimetroAbdominal
        +int colesterol
        +int trigliceridos
        +int hdl
        +int ldl
    }

    class UserData {
        +Paciente pacienteActual
        +List~Medicion~ medicionesSession
        +clienteSMPv2 smpClient
        +storeMeasurement()
        +getMediciones()
    }

    class PruebaFindrisc {
        +int id
        +String pacienteId
        +DateTime fecha
        +int puntaje
        +String nivelRiesgo
        +int porcentajeRiesgo
    }

    UserData --> Medicion : stores
    UserData --> PruebaFindrisc : stores
```

---

## Module Communication Matrix

### Direct Dependencies

| Caller Module | Called Module | Method/Interface | Frequency |
|--------------|---------------|------------------|-----------|
| All Controllers | `UserData` | `getInstance()` | Every operation |
| All Controllers | `Tools` | `changeView()`, `showAlert()` | Screen transitions |
| `PantallaTomarDatosController` | `DispositivosConfigurationManager` | `loadConfig()` | Device init |
| `PantallaConfiguracionController` | `AdminLoginManager` | `validatePassword()` | Admin login |
| `LlamadaController` | `UserData` | Network callbacks | During calls |
| `UserData` | `clienteSMPv2` | All network operations | Network activity |
| Services | `AprevenirLocalDB` | Database queries | Data operations |

### Callback Relationships

```mermaid
flowchart TD
    subgraph "Network Layer (modulos.jar)"
        SMP[clienteSMPv2]
    end

    subgraph "Business Layer"
        UD[UserData]
    end

    subgraph "Presentation Layer"
        LC[LlamadaController]
        EM[ElegirMedicoController]
        Login[PantallaLoginController]
    end

    SMP -->|connectionStatusChanged| UD
    SMP -->|dataAvailable| UD
    SMP -->|doctorListUpdated| UD
    SMP -->|adviceReceived| UD
    SMP -->|multimediaStatusChanged| UD

    UD -->|Platform.runLater| LC
    UD -->|Platform.runLater| EM
    UD -->|Platform.runLater| Login
```

---

## Event System

### Event Types

| Event | Source | Handler | Description |
|-------|--------|---------|-------------|
| `connectionStatusChanged` | SMPv2 | UserData | Network state change |
| `doctorListUpdated` | SMPv2 | UserData → ElegirMedicoController | Available doctors refresh |
| `adviceReceived` | SMPv2 | UserData → LlamadaController | Medical recommendation |
| `dataAvailable` | SMPv2 | UserData | Incoming data packet |
| `multimediaStatusChanged` | SMPv2 | UserData → LlamadaController | Video/audio status |
| `deviceStateChanged` | perifericos.* | Controllers | Device connection status |

### Thread Safety Pattern

All UI updates from background threads use JavaFX's thread-safe pattern:

```java
// Pattern used throughout codebase
Platform.runLater(() -> {
    // UI update code here
    label.setText(newValue);
    updateConnectionIndicator(status);
});
```

**Locations:**
- `UserData.java` - All callback handlers
- `LlamadaController.java` - Video frame updates
- `Services/*.java` - Progress/completion updates

---

## State Management

### Application State (UserData Singleton)

```mermaid
stateDiagram-v2
    [*] --> Initialized: Application Start
    Initialized --> LoggedIn: User Authentication
    LoggedIn --> Measuring: Start Measurements
    Measuring --> LoggedIn: Measurements Complete
    LoggedIn --> InConsultation: Doctor Selected
    InConsultation --> LoggedIn: Call Ended
    LoggedIn --> [*]: Logout/Timeout
```

### Connection State Machine

```mermaid
stateDiagram-v2
    [*] --> DESCONECTADO
    DESCONECTADO --> CONECTANDO_CON_SERVIDOR: connect()
    CONECTANDO_CON_SERVIDOR --> CONECTADO_CON_SERVIDOR: success
    CONECTANDO_CON_SERVIDOR --> DESCONECTADO: failure/timeout
    CONECTADO_CON_SERVIDOR --> CONECTANDO_CON_PEER: selectDoctor()
    CONECTANDO_CON_PEER --> CONECTADO_CON_PEER: peer accepts
    CONECTANDO_CON_PEER --> CONECTADO_CON_SERVIDOR: peer rejects/timeout
    CONECTADO_CON_PEER --> CONECTADO_CON_SERVIDOR: endCall()
    CONECTADO_CON_SERVIDOR --> DESCONECTADO: disconnect()
```

### Session Data Lifecycle

| Phase | Data Held | Persistence |
|-------|-----------|-------------|
| Pre-login | App configuration | Properties files |
| Login | User credentials, patient profile | Memory + Server |
| Measurement | Current readings, device states | Memory |
| Post-measurement | Aggregated measurements | SQLite DB |
| Consultation | Doctor info, video state | Memory |
| Export | Historical data | CSV/Excel files |

---

## See Also

- [Architecture Overview](architecture.md) - System structure
- [Design Decisions](design-decisions.md) - Why this design
- [API Overview](api-overview.md) - Interface details
- [Hardware Integration](hardware-integration.md) - Device workflows

---

*Document generated for A-Prevenir IDO architecture review*
