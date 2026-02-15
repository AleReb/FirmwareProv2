# Registro de Cambios - HIRI FirmwarePro
Fecha: 2026-02-15

## Nuevas Funcionalidades Implementadas

### 1. Modo Sin Internet (Offline Mode)
- **Archivo:** `config.h`
  - Agregada variable `bool offlineMode` en `struct SystemConfig`.
- **Archivo:** `config.ino`
  - Agregada lectura/escritura de `offlineMode` en `loadConfig()` y `saveConfig()`.
  - Default: `false` (Modo Online).
  - Comando serial: Se puede cambiar editando la configuración.
- **Archivo:** `http.ino`
  - En `ensurePdpAndNet()` y `httpGet_webhook()`: Si `config.offlineMode` es `true`, retorna `false` inmediatamente.
  - El módem NO se enciende ni intenta conectar.

### 2. Protección de Escritura SD (Anti-Bloqueo de Red)
- **Archivo:** `http.ino`
  - Implementada lógica de **Cooldown** en `ensurePdpAndNet()`.
  - Si la conexión falla **10 veces consecutivas** (aprox. 5 min de intentos):
    - Se activa un **período de espera de 15 minutos**.
    - Durante este tiempo, la función de red retorna `false` **sin bloquear**.
    - Esto permite que el bucle principal (`loop()`) continúe ejecutándose y **guardando datos en la SD sin interrupciones**.
  - Pasados los 15 minutos, se permite **un** intento de reconexión.
    - Si falla, vuelve al reposo de 15 min.
    - Si conecta, se resetea el contador.

### 3. Plan de Pruebas
- **Archivo:** `PRUEBAS.md`
  - Creado documento con checklist de validación (Arranque, Sensores, SD, Red, UI).
  - Estado actual: Varias pruebas marcadas como completadas.
  - Pendientes: "Partir sin SIM", "Extracción en caliente SD", "Salidas a terreno".

---
**Nota:** Estos cambios están aplicados en el código local (`C:\gitshubs\HIRIPROBASE01\FirmwarePro\`).
**Estado Git:** No se ha inicializado repositorio ni subido a GitHub (carpeta sin historial .git).
