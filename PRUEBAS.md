# Plan de Pruebas y Validación de Campo - HIRI FirmwarePro V0.1.1

Este documento detalla el plan de trabajo para validar la versión **Pro V0.1.1** en condiciones reales, enfocándose en la estabilidad del sistema ante fallos de conectividad, autonomía y la integridad de los datos en la tarjeta SD.

## 1. Pruebas de Escritorio (Validación Previa)
*Objetivo: Asegurar que las nuevas protecciones (Offline/Cooldown) funcionan antes de salir.*

### A. Prueba de Modo Offline (Simulado)
- [ ] **Configuración:** Desactivar módem o retirar SIM antes de encender.
- [ ] **Verificación:**
    - El equipo debe encender y pasar el chequeo inicial.
    - Al iniciar muestreo, **NO debe bloquearse** intentando conectar.
    - **Icono SD:** Debe indicar escritura exitosa (punto/parpadeo).
    - **UI:** La interfaz debe responder rápida (sin latencia de red).

### B. Prueba de Cooldown de Red (Anti-Bloqueo)
- [ ] **Escenario:** Iniciar con SIM válida, pero en zona sin cobertura (o sin antena).
- [ ] **Comportamiento Esperado:**
    - Intentos de conexión: ~10 seguidos.
    - **Transición:** El sistema debe dejar de intentar y entrar en **periodo de espera (15 min)**.
    - **Durante la espera:** El guardado en SD debe continuar sin pausas.
    - **Reintento:** Pasados 15 min, debe intentar conectar una vez automáticamente.

---

## 2. Plan de Trabajo en Terreno (Exteriores)
*Objetivo: Validar autonomía, GPS y robustez del log en movimiento.*

### Fase 0: Preparación y Registro Inicial
1.  **Bitácora de Campo:** Llevar libreta o notas en celular para cruzar datos (Hora Exacta vs Evento).
2.  **Batería Inicial:** Anotar voltaje al encender (`Bat: X.XX V`).
    - *Objetivo:* Medir consumo real en ciclo activo (Tx + SD).

### Fase 1: Encendido y GPS (Estático - 5 min)
1.  Encender equipo en exterior con cielo despejado.
2.  Cronometrar tiempo hasta obtener **FIX GPS** (Icono satélite + coordenadas en Info).
3.  Verificar fecha/hora automáticas (RTC sincronizado).

### Fase 2: Recorrido y Registro (Dinámico - 30-60 min)
1.  Iniciar Muestreo (`Menú -> Empezar Muestreo`).
2.  Realizar recorrido mixto (zonas despejadas y zonas con obstáculos/árboles).
3.  **Registro de Eventos:**
    - Usar menú del equipo: `Menú -> Opciones -> Mensajes -> [Camión/Humo/Otros]` ante eventos visibles.
    - **Bitácora Manual:** Anotar hora exacta (ej: "15:30 - Camión echando humo") para validar luego contra el sensor PM2.5 en el CSV.
4.  **Observación UI:**
    - Verificar periódicamente si la pantalla se congela al enviar datos HTTP.
    - Confirmar que el contador de SD (`SD: xx`) aumenta constantemente.

### Fase 3: Prueba de Estrés y Recuperación (Túnel/Sombra)
*Objetivo: Verificar que el equipo NO muere en zonas muertas.*
1.  **Simulación:** Ingresar a zona sin cobertura (estacionamiento subterráneo, ascensor, caja metálica) por **5 minutos**.
2.  **Durante el fallo:**
    - Verificar que **NO se reinicia** (Watchdog ok).
    - Verificar que **sigue guardando en SD**.
3.  **Recuperación:** Salir a zona despejada.
    - Cronometrar recuperación de **FIX GPS**.
    - Cronometrar recuperación de **Red 4G** y envío de datos.

### Fase 4: Gestión de Archivos en Campo (Punto de Control)
1.  Sin detener el muestreo ni apagar el equipo.
2.  Ir a `Menú -> Info -> WIFI SD` y activar **ON**.
3.  Conectar celular al AP `HIRIPRO_x` (Pass: `12345678`).
4.  Acceder a `http://192.168.4.1`.
5.  **Descargar el CSV actual** y verificar que se pueda abrir en el celular.
6.  Desactivar WiFi (`OFF`) y continuar operación normal.

### Fase 5: Cierre y Autonomía
1.  Detener muestreo y apagar.
2.  **Batería Final:** Anotar voltaje final.
3.  **Cálculo:** `(V_inicio - V_fin) / Horas = Consumo aprox/hora`.
    - *Alerta:* Si cae >0.2V en 10 min, revisar estado de la celda.

---

## 3. Análisis de Resultados (Post-Salida)
*Revisión de datos en PC (Excel/Bloc de notas)*

- [ ] **Continuidad Temporal:** Revisar columna `timestamp`. ¿Hay saltos de tiempo inexplicables? (huecos > 10 seg).
- [ ] **Coordenadas GPS:** ¿Tienen sentido o hay saltos a 0,0?
- [ ] **Notas de Evento:** Verificar que las notas (`Camión`, `Humo`) aparezcan en la columna `notas` del CSV en la fila correspondiente.
- [ ] **Integridad en Fallo de Red:** Si hubo zonas sin señal (Fase 3), ¿los datos de esos momentos están en el CSV? (Debe haber datos con `sent=0` pero guardados).
- [ ] **Correlación Bitácora:** ¿Coinciden los eventos anotados manualmente con los picos de PM2.5/PM10 en el CSV?

## 4. Criterios de Aceptación para Merge a Main
- [ ] El sistema **NUNCA** debe dejar de guardar en SD por culpa de la red.
- [ ] El GPS no debe perder el FIX por periodos prolongados sin recuperarlo.
- [ ] El archivo CSV descargado por WiFi debe ser idéntico al de la SD física.
