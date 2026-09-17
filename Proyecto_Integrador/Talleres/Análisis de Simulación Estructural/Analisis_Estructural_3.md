# Análisis de Simulación Estructural - Jhosselyn Dayanna Enriquez Aliaga
# Caja Electrónica
<img width="552" height="556" alt="Caja electronica" src="https://github.com/user-attachments/assets/d5a6ccdd-2a62-46dc-a55a-008844526ab2" />

## Descripción del modelo
<img width="1896" height="860" alt="caja" src="https://github.com/user-attachments/assets/dbaa0f28-902b-41f4-8a8f-51ce1be0262c" />

- Pieza tipo **caja electrónica** (pared con rejilla de agujeros de ventilación y base horizontal doblada).
- Contiene: ESP32, HX711, fuente de alimentación y conexiones.
- Largo: 35 cm (350 mm).
- Ancho: 15 cm (150 mm).
- Alto: 18 cm (180 mm).
- Material: **Acero (Steel)**.
- Carga aplicada: **-42 N** en dirección **X** (sistema de coordenadas global), aplicada sobre la cara del brazo perforado (face 239 @ Part 1).
- Gravedad: **9.8 m/s²** (incluye el peso propio de la pieza).

---

## Propiedades del material (Acero estructural)
| Propiedad | Valor |
|-----------|-------|
| Módulo de Young (E) | 200 000 MPa |
| Coeficiente de Poisson (ν) | 0.30 |
| Densidad (ρ) | 7850 kg/m³ |
| Límite de fluencia (aprox.) | ~250 MPa |

---

## Cargas aplicadas
- **Force 2: fx = -42 N, fy = 0, fz = 0** → Fuerza lateral (eje X global) aplicada sobre la cara perforada del brazo, simulando el esfuerzo que transmite la estructura sobre ese punto de sujeción.
- **Fixed support 1** → Apoyo fijo en la base del bracket.
- **Gravedad (9.8 m/s²)** → Peso propio de la estructura.

---

## Resultados principales

### 1. Tensión (esfuerzo)
- **Valor máximo:** 7.081 kPa (0.007081 MPa).
- El acero soporta típicamente ~250 MPa antes de fluir.
- **Conclusión:** La tensión máxima representa solo el 0.0028% de la capacidad del material. El margen de seguridad es enorme; no hay ningún riesgo de rotura ni de fluencia bajo esta carga.

### 2. Desplazamiento (deformación)
- **Conclusión preliminar:** Dado lo bajo de la tensión (0.0028% de la capacidad del acero), se espera un desplazamiento prácticamente imperceptible, del orden de fracciones de milímetro o menos.

---

## Conclusiones
- El acero, con su alta rigidez y resistencia, deja un margen de seguridad muy superior al que tendría la misma pieza en PLA.
- No hay riesgo de fallo por tensión bajo esta carga.
- Dado el margen tan amplio (0.0028% de uso), hay espacio considerable para reducir material (agrandar o multiplicar las perforaciones, reducir espesor) sin comprometer la resistencia.
- El diseño es **seguro y fiable** para la carga especificada de -42 N.

---
