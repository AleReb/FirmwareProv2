# Plan de Pruebas - HIRI FirmwarePro

Estado actual de validación y pruebas pendientes para el dispositivo.

## 1. Arranque e Inicialización (Boot)
- [x] ~~Encendido en frío: Verificar logo/animación.~~
- [x] ~~Detección de Hardware: PMS, GPS, RTC, SD detectados.~~
- [x] ~~Fallo de SD: Aviso en sistema si no hay tarjeta.~~
- [ ] **Partir sin SIM / SIM Bloqueada**
    - **Objetivo:** Verificar comportamiento cuando no hay conectividad celular.
    - **Esperado:** El sistema debe generar un flag (bool) indicando fallo de transmisión, pero **debe seguir guardando datos en la SD** sin interrupciones.

## 2. Sensores y Datos
- [x] ~~Lectura PMS: Valores estables PM2.5/PM10.~~
- [x] ~~GPS Lock: Obtención de fix en exteriores.~~
- [x] ~~RTC vs GPS: Sincronización de hora.~~

## 3. Almacenamiento (SD Card)
- [x] ~~Escritura de Logs: Creación de archivos.~~
- [x] ~~Formato de Datos: Integridad del CSV/TXT.~~
- [ ] **Extracción en caliente**
    - **Objetivo:** Simular fallo crítico de almacenamiento.
    - **Prueba:** Retirar SD durante escritura.
    - **Pregunta:** ¿Recupera al reinsertar o requiere reinicio?

## 4. Conectividad (HTTP)
- [x] ~~Modo AP: Levantar punto de acceso propio.~~
- [x] ~~Envío HTTP: Código 200 OK en servidor.~~

## 5. Interfaz de Usuario (UI)
- [x] ~~Navegación: Recorrido de menús.~~
- [x] ~~Botones: Pulsación corta/larga.~~
- [x] ~~Refresco de pantalla.~~
    - **Nota:** *Existe un ligero retraso en la UI cuando la transmisión está activa, ya que el envío HTTP es bloqueante (no asincrónico).*

## 6. Estabilidad y Terreno
- [x] ~~Estabilidad Estática: Prueba continua > 24 horas enchufado.~~
- [ ] **Salida a terreno - Auto**
    - **Duración:** > 2 horas.
    - **Objetivo:** Verificar comportamiento con vibración y velocidad (GPS updating).
- [ ] **Salida a terreno - Caminata**
    - **Duración:** > 2 horas.
    - **Objetivo:** Portabilidad, batería en movimiento y precisión GPS a baja velocidad.
