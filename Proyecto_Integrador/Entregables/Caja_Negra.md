# Caja negra del sistema RECONEXA

<p align="center">
  <img width="1035" height="627" alt="CAJA NEGRA" src="https://github.com/user-attachments/assets/f142057f-1836-4727-9014-f3cb72029236" />
</p>

<p align="center">
  <em>Figura 1. Caja negra del sistema de caracterización de residuos metálicos RECONEXA.</em>
</p>

## Entradas del sistema

El sistema recibe tres tipos de flujos:

### Materia

- **Muestra de residuo metálico:** muestra de cobre, aluminio o latón que será evaluada por el sistema.

### Energía

- **Energía eléctrica:** permite alimentar los sensores, la cámara, el sistema de procesamiento y los elementos de comunicación.

### Señales e información

- **Señal de encendido y apagado:** activa o desactiva el sistema.
- **Señal de inicio del ciclo:** ordena comenzar la medición de la muestra.
- **Señal de parada de emergencia:** interrumpe el proceso ante una falla o por decisión del usuario.
- **Señal de calibración:** permite ajustar los sensores empleando valores o muestras conocidas.
- **Datos de registro:** identifican la muestra, su procedencia, la fecha y la empresa generadora.

## Salidas del sistema

Después del proceso de medición, RECONEXA genera las siguientes salidas:

### Materia

- **Muestra metálica evaluada:** corresponde a la misma muestra ingresada, sin una transformación física.

### Señales e información

- **Datos de caracterización:** resultados obtenidos mediante los sensores y la cámara.
- **Identificación estimada del tipo de metal:** indica si la muestra probablemente corresponde a cobre, aluminio o latón.
- **Señal de estado:** comunica mediante LED o pantalla si el sistema está preparado, midiendo o presenta una falla.
- **Señal de alarma:** alerta mediante un buzzer cuando se detecta un problema.
- **Señal de ciclo completado:** informa que la medición terminó y que la muestra puede retirarse.
- **Notificación a empresas:** comunica la disponibilidad de un residuo metálico que podría ser aprovechado como materia prima secundaria.

## Leyenda

- **Flecha continua gruesa:** flujo de energía.
- **Flecha continua con borde:** flujo de materia.
- **Flecha punteada:** flujo de señales o información.
