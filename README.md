# HIRI FirmwarePro

Firmware profesional para dispositivos de monitoreo ambiental HIRI.
Integra sensores de calidad de aire (PMS5003ST, SHT31, SDS198), GNSS multibanda y conectividad 4G (SIM7600).

## Visualización de Datos
Los datos reportados por el dispositivo pueden visualizarse en tiempo real en:
**[https://hiri.cmasccp.cl/map](https://hiri.cmasccp.cl/map)**

## Características Principales
- **Modo Offline:** Configurable para operar solo como datalogger (sin transmisión).
- **Protección de Red:** Sistema inteligente de reintentos (Cooldown 15min) para evitar bloqueos en zonas sin cobertura.
- **Almacenamiento Seguro:** Escritura en SD optimizada para evitar interrupciones de datos.
- **GNSS Avanzado:** Soporte para GPS, GLONASS, Galileo y Beidou con asistencia XTRA.
- **Detección Automática:** Sensores PMS, SHT31, SDS198 y RTC se autodetectan.

## Configuración
El dispositivo almacena su configuración en memoria flash (Preferences).
Se puede ajustar vía puerto serie:
- Intervalos de muestreo y envío.
- Modos GNSS y OLED.
- Activación/Desactivación de Modo Offline.

## Estado del Proyecto
Versión estable en desarrollo.
Consulta `PRUEBAS.md` para el plan de validación actual.
Consulta `CAMBIOS.md` para el historial reciente de modificaciones.
