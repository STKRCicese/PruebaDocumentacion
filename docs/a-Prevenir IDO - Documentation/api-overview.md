---
sidebar_position: 5
---

# API Overview

> **Last Updated:** 2026-02-22
> **Document Version:** 1.0
> **Related:** [Architecture Overview](architecture.md), [Module Interactions](module-interactions.md)

---

## Table of Contents

1. [Overview](#overview)
2. [Core Classes API](#core-classes-api)
3. [Controller APIs](#controller-apis)
4. [Service APIs](#service-apis)
5. [Model Classes](#model-classes)
6. [External Interfaces](#external-interfaces)
7. [Configuration Interfaces](#configuration-interfaces)

---

## Overview

This document provides a comprehensive reference of the public APIs and interfaces in the A-Prevenir system. It covers the main entry points, controller interfaces, service classes, and data models.

### API Conventions

| Convention | Description |
|------------|-------------|
| **Language** | Java 8 |
| **Naming** | CamelCase for classes, camelCase for methods |
| **Documentation** | Spanish inline comments |
| **Threading** | UI operations via `Platform.runLater()` |
| **Errors** | Exceptions for critical errors, boolean returns for operations |

---

## Core Classes API

### APrevenir.java (Main Entry Point)

**Location:** `src/aPrevenir/APrevenir.java`

**Class:** `public class APrevenir extends Application`

| Method | Signature | Description |
|--------|-----------|-------------|
| `main` | `public static void main(String[] args)` | Application entry point |
| `start` | `public void start(Stage primaryStage)` | JavaFX initialization |
| `stop` | `public void stop()` | Application shutdown |

**Command Line Arguments:**

```bash
java APrevenir                    # Normal mode
java APrevenir demo               # Demo mode (simulated devices)
java APrevenir demo cabina        # Demo + Workstation mode
java APrevenir cabina             # Workstation mode (full screen)
```

**Initialization Flow:**

```java
// Pseudocode of initialization
public void start(Stage primaryStage) {
    // 1. Initialize logging
    ConsolePrintStream.initialize();

    // 2. Load configuration
    DispositivosConfigurationManager.loadConfig();

    // 3. Set up stage
    Tools.setStage(primaryStage);

    // 4. Load initial view
    Tools.changeView(Pantallas.INICIO);

    // 5. Initialize network (if online)
    UserData.getInstance().iniciarInstanciaCliente(false);
}
```

---

### UserData.java (Session Singleton)

**Location:** `src/aPrevenir/Modelos/UserData.java`

**Class:** `public class UserData`

#### Singleton Access

```java
public static UserData getInstance()
```

Returns the singleton instance of UserData.

#### Patient Session Methods

| Method | Signature | Description |
|--------|-----------|-------------|
| `getPaciente` | `public Paciente getPaciente()` | Get current patient |
| `setPaciente` | `public void setPaciente(Paciente p)` | Set current patient |
| `isRegistered` | `public boolean isRegistered()` | Check if patient is registered |
| `clearSession` | `public void clearSession()` | Reset session data |

#### Network Methods

| Method | Signature | Description |
|--------|-----------|-------------|
| `iniciarInstanciaCliente` | `public clienteSMPv2 iniciarInstanciaCliente(boolean reconnect)` | Initialize SMPv2 client |
| `getConnectionStatus` | `public estadoConexion getConnectionStatus()` | Get current connection state |
| `isConnectedToServer` | `public boolean isConnectedToServer()` | Server connection check |
| `isConnectedToPeer` | `public boolean isConnectedToPeer()` | Peer connection check |
| `disconnectFromServer` | `public void disconnectFromServer()` | Terminate server connection |
| `disconnectFromPeer` | `public void disconnectFromPeer()` | Terminate peer connection |

#### SMPv2 Callback Methods (Implemented Interfaces)

| Method | Signature | Triggered When |
|--------|-----------|----------------|
| `connectionStatusChanged` | `public void connectionStatusChanged()` | Network state changes |
| `dataAvailable` | `public void dataAvailable()` | Data received from server |
| `doctorListUpdated` | `public void doctorListUpdated()` | Doctor list refreshed |
| `adviceReceived` | `public void adviceReceived()` | Medical advice received |
| `multimediaStatusChanged` | `public void multimediaStatusChanged()` | Video/audio status change |

#### Measurement Methods

| Method | Signature | Description |
|--------|-----------|-------------|
| `storeMeasurement` | `public void storeMeasurement(String type, Object value)` | Store a measurement |
| `getMediciones` | `public List<Medicion> getMediciones()` | Get session measurements |
| `getTemperatura` | `public Float getTemperatura()` | Get temperature reading |
| `getPresionSistolica` | `public Integer getPresionSistolica()` | Get systolic pressure |
| `getPresionDiastolica` | `public Integer getPresionDiastolica()` | Get diastolic pressure |
| `getGlucosa` | `public Integer getGlucosa()` | Get glucose reading |
| `getPeso` | `public Float getPeso()` | Get weight |
| `getRitmoCardiaco` | `public Integer getRitmoCardiaco()` | Get heart rate |

#### Doctor/Consultation Methods

| Method | Signature | Description |
|--------|-----------|-------------|
| `getDoctorList` | `public List<Medico> getDoctorList()` | Get available doctors |
| `selectDoctor` | `public void selectDoctor(Medico medico)` | Initiate call to doctor |
| `endCall` | `public void endCall()` | Terminate active call |
| `getRecommendations` | `public String getRecommendations()` | Get medical advice |

---

### Tools.java (Utility Class)

**Location:** `src/aPrevenir/Tools.java`

**Class:** `public class Tools`

#### Stage Management

| Method | Signature | Description |
|--------|-----------|-------------|
| `setStage` | `public static void setStage(Stage stage)` | Set main application stage |
| `getStage` | `public static Stage getStage()` | Get main stage reference |
| `changeView` | `public static void changeView(Pantallas pantalla)` | Navigate to screen |
| `changeViewWithData` | `public static void changeViewWithData(Pantallas p, Object data)` | Navigate with data |

#### Dialog Methods

| Method | Signature | Description |
|--------|-----------|-------------|
| `showAlert` | `public static void showAlert(String message)` | Show info alert |
| `showError` | `public static void showError(String message)` | Show error alert |
| `showConfirmation` | `public static boolean showConfirmation(String message)` | Show yes/no dialog |
| `showInputDialog` | `public static String showInputDialog(String prompt)` | Show text input |

#### Timer Management

| Method | Signature | Description |
|--------|-----------|-------------|
| `startIdleTimer` | `public static void startIdleTimer(int seconds)` | Start inactivity timer |
| `cancelIdleTimer` | `public static void cancelIdleTimer()` | Cancel idle timer |
| `resetIdleTimer` | `public static void resetIdleTimer()` | Reset timer countdown |

#### Utility Methods

| Method | Signature | Description |
|--------|-----------|-------------|
| `formatDate` | `public static String formatDate(Date date)` | Format date for display |
| `isValidCURP` | `public static boolean isValidCURP(String curp)` | Validate CURP format |
| `calculateIMC` | `public static float calculateIMC(float peso, float altura)` | Calculate BMI |
| `getNormalRange` | `public static NormalRange getNormalRange(String type)` | Get normal value range |

---

### AdminLoginManager.java

**Location:** `src/aPrevenir/AdminLoginManager.java`

**Class:** `public class AdminLoginManager`

| Method | Signature | Description |
|--------|-----------|-------------|
| `validatePassword` | `public static boolean validatePassword(String password)` | Validate admin password |
| `changePassword` | `public static boolean changePassword(String oldPwd, String newPwd)` | Change admin password |

**Implementation Notes:**
- Uses JKS keystore at `res/aprevenir_llave.jks`
- Password validated against stored hash
- Returns `true` if authentication successful

---

### DispositivosConfigurationManager.java

**Location:** `src/aPrevenir/DispositivosConfigurationManager.java`

**Class:** `public class DispositivosConfigurationManager`

| Method | Signature | Description |
|--------|-----------|-------------|
| `loadConfig` | `public static void loadConfig()` | Load device configuration |
| `saveConfig` | `public static void saveConfig()` | Persist configuration |
| `isDeviceEnabled` | `public static boolean isDeviceEnabled(PerifericoEnum device)` | Check if device enabled |
| `getDeviceSerialNumber` | `public static String getDeviceSerialNumber(PerifericoEnum device)` | Get device serial |
| `getDevicePort` | `public static String getDevicePort(PerifericoEnum device)` | Get serial port |
| `setDeviceEnabled` | `public static void setDeviceEnabled(PerifericoEnum device, boolean enabled)` | Enable/disable device |

**Configuration Properties:**

```properties
# dispositivos.properties
glucometroHabilitado=1
basculaHabilitado=1
baumanometroHabilitado=1
termometroHabilitado=1
estadimetroHabilitado=1
cintaHabilitado=1
lipidosHabilitado=1
ecgHabilitado=1
glucometroSerialNumber=DBG6314CY
basculaID=AD00003F
baumanometroID=AD000045A
termometroID=TERPEL_V2
```

---

## Controller APIs

### Base Controller Pattern

All controllers follow the JavaFX FXML controller pattern:

```java
public class ExampleController implements Initializable {

    @FXML private Label exampleLabel;
    @FXML private Button exampleButton;

    @Override
    public void initialize(URL url, ResourceBundle rb) {
        // Controller initialization
    }

    @FXML
    private void onButtonClick(ActionEvent event) {
        // Event handler
    }
}
```

### Pantalla1Controller (Welcome Screen)

**Location:** `src/aPrevenir/Controladores/Pantalla1Controller.java`

**FXML:** `Vistas/kiosco/PantallaInicio.fxml`

| Method | Description |
|--------|-------------|
| `initialize()` | Set up idle timer, load welcome content |
| `onScreenTouch()` | Navigate to login screen |
| `onIdleTimeout()` | Return to welcome after inactivity |

---

### PantallaLoginController

**Location:** `src/aPrevenir/Controladores/PantallaLoginController.java`

**FXML:** `Vistas/kiosco/PantallaLogin.fxml`

| Method | Signature | Description |
|--------|-----------|-------------|
| `initialize` | `void initialize(URL, ResourceBundle)` | Set up login form |
| `onLogin` | `void onLogin(ActionEvent)` | Process login attempt |
| `onCancel` | `void onCancel(ActionEvent)` | Return to welcome |
| `onRegister` | `void onRegister(ActionEvent)` | Navigate to registration |

**Login Flow:**

```java
@FXML
private void onLogin(ActionEvent event) {
    String curp = curpField.getText();
    String folio = folioField.getText();

    if (validateCredentials(curp, folio)) {
        UserData.getInstance().setPaciente(loadPaciente(curp));
        Tools.changeView(Pantallas.SERVICIOS);
    } else {
        Tools.showError("Credenciales inválidas");
    }
}
```

---

### PantallaServiciosController

**Location:** `src/aPrevenir/Controladores/PantallaServiciosController.java`

**FXML:** `Vistas/kiosco/PantallaServicios.fxml`

| Method | Description |
|--------|-------------|
| `onSomatometria()` | Start measurement-only flow |
| `onConsulta()` | Start telemedicine consultation |
| `onHistorial()` | View measurement history |
| `onLogout()` | End session and return to welcome |

---

### PantallaTomarDatosController

**Location:** `src/aPrevenir/Controladores/PantallaTomarDatosController.java`

**FXML:** `Vistas/kiosco/PantallaTomarDatos.fxml`

| Method | Signature | Description |
|--------|-----------|-------------|
| `initialize` | `void initialize(URL, ResourceBundle)` | Initialize device connections |
| `onMeasureTemperature` | `void onMeasureTemperature(ActionEvent)` | Start temperature measurement |
| `onMeasureBloodPressure` | `void onMeasureBloodPressure(ActionEvent)` | Start BP measurement |
| `onMeasureWeight` | `void onMeasureWeight(ActionEvent)` | Start weight measurement |
| `onMeasureGlucose` | `void onMeasureGlucose(ActionEvent)` | Start glucose measurement |
| `onMeasureWaist` | `void onMeasureWaist(ActionEvent)` | Start waist measurement |
| `onMeasureLipids` | `void onMeasureLipids(ActionEvent)` | Start lipid panel |
| `onDeviceDataReceived` | `void onDeviceDataReceived(String type, Object value)` | Handle device reading |
| `onComplete` | `void onComplete(ActionEvent)` | Finish measurements |

**Device Callback Pattern:**

```java
// Pseudocode for device measurement
private void measureTemperature() {
    termometro.setCallback((value) -> {
        Platform.runLater(() -> {
            onDeviceDataReceived("temperatura", value);
        });
    });
    termometro.startReading();
}
```

---

### LlamadaController (Telemedicine Call)

**Location:** `src/aPrevenir/Controladores/LlamadaController.java`

**FXML:** `Vistas/kiosco/PantallaLlamada.fxml`

| Method | Signature | Description |
|--------|-----------|-------------|
| `initialize` | `void initialize(URL, ResourceBundle)` | Set up video display |
| `startCall` | `void startCall(Medico doctor)` | Initiate P2P connection |
| `onVideoFrameReceived` | `void onVideoFrameReceived(Image frame)` | Display video frame |
| `onRecommendationReceived` | `void onRecommendationReceived(String text)` | Show doctor's advice |
| `onEndCall` | `void onEndCall(ActionEvent)` | Terminate call |
| `onMuteAudio` | `void onMuteAudio(ActionEvent)` | Toggle audio mute |
| `onToggleVideo` | `void onToggleVideo(ActionEvent)` | Toggle video display |

---

### PantallaFINDRISCController

**Location:** `src/aPrevenir/Controladores/PantallaFINDRISCController.java`

**FXML:** `Vistas/kiosco/PantallaFINDRISC.fxml`

| Method | Signature | Description |
|--------|-----------|-------------|
| `initialize` | `void initialize(URL, ResourceBundle)` | Load questionnaire |
| `nextQuestion` | `void nextQuestion()` | Advance to next question |
| `previousQuestion` | `void previousQuestion()` | Go back one question |
| `onAnswerSelected` | `void onAnswerSelected(int questionId, int answer)` | Record answer |
| `calculateScore` | `int calculateScore()` | Compute FINDRISC score |
| `showResults` | `void showResults(int score)` | Display risk assessment |

**FINDRISC Scoring:**

| Score Range | Risk Level | 10-Year Diabetes Risk |
|-------------|------------|----------------------|
| 0-7 | Low | ~1% |
| 8-11 | Slightly elevated | ~4% |
| 12-14 | Moderate | ~17% |
| 15-20 | High | ~33% |
| >20 | Very High | ~50% |

---

### PantallaConfiguracionController

**Location:** `src/aPrevenir/Controladores/PantallaConfiguracionController.java`

**FXML:** `Vistas/kiosco/PantallaConfiguracion.fxml`

| Method | Signature | Description |
|--------|-----------|-------------|
| `initialize` | `void initialize(URL, ResourceBundle)` | Load current config |
| `onAdminLogin` | `void onAdminLogin(ActionEvent)` | Authenticate admin |
| `onSaveDeviceConfig` | `void onSaveDeviceConfig(ActionEvent)` | Save device settings |
| `onExportData` | `void onExportData(ActionEvent)` | Export measurements |
| `onExportFINDRISC` | `void onExportFINDRISC(ActionEvent)` | Export FINDRISC data |
| `onSendEmail` | `void onSendEmail(ActionEvent)` | Email reports |
| `onCalibrate` | `void onCalibrate(PerifericoEnum device)` | Calibrate device |

---

## Service APIs

### MedicionesEmailService

**Location:** `src/aPrevenir/services/MedicionesEmailService.java`

**Class:** `public class MedicionesEmailService extends Service<Boolean>`

```java
// Constructor
public MedicionesEmailService(
    String recipientEmail,
    Paciente paciente,
    List<Medicion> mediciones,
    String recommendations
)

// Service methods (inherited from JavaFX Service)
public void start()
public void cancel()
public void reset()
public boolean getValue()  // Returns true if email sent successfully
```

**Usage:**

```java
MedicionesEmailService emailService = new MedicionesEmailService(
    "patient@email.com",
    UserData.getInstance().getPaciente(),
    UserData.getInstance().getMediciones(),
    UserData.getInstance().getRecommendations()
);

emailService.setOnSucceeded(e -> {
    if (emailService.getValue()) {
        Tools.showAlert("Email enviado exitosamente");
    }
});

emailService.start();
```

---

### ExportarDBService

**Location:** `src/aPrevenir/services/ExportarDBService.java`

**Class:** `public class ExportarDBService extends Service<String>`

```java
// Constructor
public ExportarDBService(
    Date startDate,
    Date endDate,
    ExportType type  // MEDICIONES or FINDRISC
)

// Returns file path of exported file
public String getValue()
```

**Export Formats:**

| Type | Output File | Columns |
|------|-------------|---------|
| MEDICIONES | `exportar/exportar_mediciones_*.csv` | 20 columns |
| FINDRISC | `exportar/exportar_findrisc_*.csv` | 17 columns |

**CSV Header (Mediciones):**

```
ID,Fecha,Nombre,Apellidos,CURP,Edad,Sexo,Temperatura,
PresionSistolica,PresionDiastolica,RitmoCardiaco,Peso,
Altura,IMC,Glucosa,PerimetroAbdominal,Colesterol,
Trigliceridos,HDL,LDL
```

---

### ExportarEmailService

**Location:** `src/aPrevenir/services/ExportarEmailService.java`

**Class:** `public class ExportarEmailService extends Service<Boolean>`

```java
// Constructor
public ExportarEmailService(
    String recipientEmail,
    List<Medicion> mediciones,
    boolean includeCharts
)
```

**Generated Charts:**
- `glucosaChart.png` - Blood glucose history
- `presionChart.png` - Blood pressure trend
- `pesoChart.png` - Weight tracking
- `colesterolChart.png` - Cholesterol levels

---

## Model Classes

### Paciente (Patient)

**Package:** `dao.Paciente` (in external JAR)

| Field | Type | Description |
|-------|------|-------------|
| `id` | `int` | Database ID |
| `curp` | `String` | Unique patient identifier (CURP) |
| `folio` | `String` | Registration folio number |
| `nombre` | `String` | First name |
| `apellidoPaterno` | `String` | Father's last name |
| `apellidoMaterno` | `String` | Mother's last name |
| `fechaNacimiento` | `Date` | Birth date |
| `sexo` | `String` | Gender (M/F) |
| `email` | `String` | Email address |
| `telefono` | `String` | Phone number |

---

### Medicion (Measurement)

**Package:** `dao.Medicion` (in external JAR)

| Field | Type | Unit | Normal Range |
|-------|------|------|--------------|
| `id` | `int` | - | - |
| `pacienteId` | `int` | - | - |
| `fecha` | `DateTime` | - | - |
| `temperatura` | `float` | °C | 36.1-37.2 |
| `presionSistolica` | `int` | mmHg | 90-120 |
| `presionDiastolica` | `int` | mmHg | 60-80 |
| `ritmoCardiaco` | `int` | bpm | 60-100 |
| `peso` | `float` | kg | varies |
| `altura` | `float` | cm | varies |
| `imc` | `float` | kg/m² | 18.5-24.9 |
| `glucosa` | `int` | mg/dL | 70-100 (fasting) |
| `perimetroAbdominal` | `float` | cm | < 102 M, < 88 F |
| `colesterol` | `int` | mg/dL | < 200 |
| `trigliceridos` | `int` | mg/dL | < 150 |
| `hdl` | `int` | mg/dL | >40 M, >50 F |
| `ldl` | `int` | mg/dL | < 100 |

---

### Medico (Physician)

**Location:** `src/aPrevenir/Modelos/Medico.java`

| Field | Type | Description |
|-------|------|-------------|
| `id` | `int` | Server ID |
| `nombre` | `String` | Full name |
| `especialidad` | `String` | Medical specialty |
| `estado` | `MedicoEstadoEnum` | Availability status |
| `foto` | `Image` | Profile photo |

---

### Enumerations

#### Pantallas (Screens)

**Location:** `src/aPrevenir/Modelos/Pantallas.java`

```java
public enum Pantallas {
    INICIO,
    LOGIN,
    SERVICIOS,
    MEDICIONES,
    ELEGIR_MEDICO,
    LLAMADA,
    FINDRISC,
    CONFIGURACION,
    REGISTRO,
    REGISTRO_LOCAL,
    HISTORIAL,
    GRAFICAS
}
```

#### PerifericoEnum (Peripheral Devices)

**Location:** `src/aPrevenir/Modelos/PerifericoEnum.java`

```java
public enum PerifericoEnum {
    GLUCOMETRO,
    BASCULA,
    BAUMANOMETRO,
    TERMOMETRO,
    ESTADIMETRO,
    CINTA_DIGITAL,
    LIPIDOS,
    ECG
}
```

#### MedicoEstadoEnum (Doctor Status)

**Location:** `src/aPrevenir/Modelos/MedicoEstadoEnum.java`

```java
public enum MedicoEstadoEnum {
    DISPONIBLE,      // Available for calls
    OCUPADO,         // In another call
    NO_DISPONIBLE,   // Offline
    AUSENTE          // Away
}
```

#### ServiceEnum

**Location:** `src/aPrevenir/Modelos/ServiceEnum.java`

```java
public enum ServiceEnum {
    SOMATOMETRIA,    // Measurements only
    CONSULTA,        // Full consultation
    FINDRISC         // Diabetes risk test
}
```

---

## External Interfaces

### SMPv2 Client Interface

**Package:** `conectividad.usuarios.estacion.SMPv2.clienteSMPv2` (in modulos.jar)

| Method | Description |
|--------|-------------|
| `connect(String ip, int port)` | Connect to SMP server |
| `disconnect()` | Disconnect from server |
| `requestDoctorList()` | Request available doctors |
| `connectToPeer(int doctorId)` | Initiate P2P connection |
| `disconnectFromPeer()` | End P2P connection |
| `sendData(byte[] data)` | Send data to peer |
| `setCallback(SMPCallback callback)` | Set event callback handler |

### Device Interface Pattern

**Package:** `perifericos.*` (in modulos.jar)

All device classes follow a common pattern:

```java
public interface DeviceInterface {
    void initialize(String serialNumber, String port);
    void connect();
    void disconnect();
    boolean isConnected();
    void startReading();
    void stopReading();
    void setCallback(DeviceCallback callback);
    DeviceState getState();
}

public interface DeviceCallback {
    void onDataReceived(Object value);
    void onError(DeviceError error);
    void onStateChanged(DeviceState state);
}
```

---

## Configuration Interfaces

### Properties File Format

**dispositivos.properties:**

```properties
# Device Enable Flags (1=enabled, 0=disabled)
glucometroHabilitado=1
basculaHabilitado=1
baumanometroHabilitado=1
termometroHabilitado=1
estadimetroHabilitado=1
cintaHabilitado=1
lipidosHabilitado=1
ecgHabilitado=1

# Device Identifiers
glucometroSerialNumber=DBG6314CY
estadimetroID=ESTPEL_V1A
basculaID=AD00003F
baumanometroID=AD000045A
termometroID=TERPEL_V2
cintaID=DPUQWR72A
lipidID=CARDIOCHECKA

# System Flags
eceIntegrado=1
modoDemo=0
```

### Server Configuration

**server.ip:**

```
192.168.1.100
```

Single line containing server IP address.

---

## Error Codes

### Connection Errors

| Code | Constant | Description |
|------|----------|-------------|
| 0 | `NO_ERROR` | Success |
| 1 | `CONNECTION_TIMEOUT` | Server unreachable |
| 2 | `AUTHENTICATION_FAILED` | Invalid credentials |
| 3 | `PEER_REJECTED` | Doctor declined call |
| 4 | `NETWORK_ERROR` | General network failure |
| 5 | `SERVER_ERROR` | Server-side error |

### Device Errors

| Code | Constant | Description |
|------|----------|-------------|
| 100 | `DEVICE_NOT_FOUND` | Device not connected |
| 101 | `DEVICE_TIMEOUT` | Reading timeout |
| 102 | `DEVICE_ERROR` | General device error |
| 103 | `CALIBRATION_NEEDED` | Device needs calibration |
| 104 | `INVALID_READING` | Reading out of range |

---

## See Also

- [Architecture Overview](architecture.md) - System structure
- [Module Interactions](module-interactions.md) - Workflow details
- [Hardware Integration](hardware-integration.md) - Device specifics
- [Glossary](glossary.md) - Term definitions

---

*Document generated for A-Prevenir IDO architecture review*
