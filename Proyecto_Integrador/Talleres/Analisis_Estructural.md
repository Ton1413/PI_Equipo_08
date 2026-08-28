# Análisis de Simulación Estructural - Suasnabar Jhonatan
# Parte Superior
<img width="470" height="560" alt="Captura de pantalla 2026-08-27 a la(s) 10 07 20 p  m" src="https://github.com/user-attachments/assets/87cae6dd-6976-4380-9c1a-4144a815f76f" />

## Descripción del modelo
- Pieza con forma de **L**.
- Brazo horizontal: 25 cm.
- Brazo vertical: 15 cm.
- Material: **PLA**.
- Carga aplicada: **0.20 N** en el centro del brazo horizontal (simula el peso del sensor).
- Gravedad: **9.8 m/s²** (incluye el peso propio de la pieza).

---

## Propiedades del material (PLA)
| Propiedad | Valor |
|-----------|-------|
| Módulo de Young (E) | 3500 MPa |
| Coeficiente de Poisson (ν) | 0.36 |
| Densidad (ρ) | 1250 kg/m³ |

---

## Cargas aplicadas
- **0.20 N (hacia abajo)** → Peso del sensor que va a soportar la pieza.
- **Gravedad (9.8 m/s²)** → Peso propio de la estructura (la propia barra).

---

## Resultados principales
<img width="1897" height="898" alt="prueba3" src="https://github.com/user-attachments/assets/abe1fc84-e51c-4fa9-9b75-5bc81f8887ad" />

<img width="889" height="703" alt="Captura de pantalla 2026-08-27 a la(s) 10 23 40 p  m" src="https://github.com/user-attachments/assets/e90f43a1-de3e-4c67-bddc-a7f91acd7f72" />

### 1. Tensión (esfuerzo)
- **Valor máximo:** 1.77 kPa (0.00177 MPa).
- El PLA soporta hasta ~50–80 MPa antes de fallar.
- **Conclusión:** La tensión es **muy baja** (solo el 0.0035% de la capacidad del material). No hay riesgo de rotura.

### 2. Desplazamiento (deformación)
- **Desplazamiento máximo estimado:** ~9.23 × 10⁻¹⁰ m (prácticamente nulo).
- Es mucho menor que el grosor de un cabello.
- **Conclusión:** La pieza es **extremadamente rígida**; no se deforma de forma apreciable.

---

## Conclusiones
- El soporte es **estructuralmente sobrado** para el sensor que va a sujetar.
- No hay riesgo de fallo por tensión ni deformación excesiva.
- Se podría **reducir material** (espesor, tamaño) para aligerar la pieza.
- El diseño es **seguro y fiable** para la carga especificada.

---
