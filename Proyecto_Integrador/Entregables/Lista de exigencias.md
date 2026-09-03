
**PROYECTO:** SISTEMA DE SELECCIÓN Y REDISTRIBUCIÓN DE RESIDUOS (RECONEXA)
**Cliente:** Universidad Peruana Cayetano Heredia
**Edición:** Rev. 3 — Fecha de corrección: 31/08/2026
**Elaborado:** E.J, U.A, C.P, S.J, Z.A

| Fecha | D/E | Deseo o Exigencia | Descripción | Responsable |
|---|---|---|---|---|
| 10/09/26 | E | **Función Principal** | El sistema debe conectar generadores de residuos industriales con empresas que puedan utilizarlos como recurso. La estación RECONEXA debe medir y caracterizar el residuo (peso, volumen, humedad, tipo de material) en origen, mientras la plataforma digital gestiona el registro, almacenamiento, publicación y búsqueda de compradores. El algoritmo debe identificar oportunidades de simbiosis industrial entre PYMES de un mismo ecosistema local [1][2][3][4][6][10][12]. | U.A |
| 08/09/26 | E | **Geometría** | La estación RECONEXA debe tener un diseño compacto y modular que permita su instalación en espacios reducidos de plantas industriales. Debe contar con compartimentos para sensores (cámara, sensor de peso, humedad y volumen) y una bandeja o plataforma de carga para colocar el residuo a analizar. Dimensiones aproximadas: 60x40x30 cm (similar a un contenedor de reciclaje inteligente) [24]. | U.A |
| 08/09/26 | E | **Cinemática** | Para la versión avanzada del prototipo, se podría considerar un sistema de banda transportadora o plataforma giratoria que permita posicionar el residuo frente a los sensores de manera automatizada. El movimiento debe ser controlado por servomotores y tener una velocidad regulable. El sistema debe poder clasificar hasta 5 tipos de residuos diferentes mediante un mecanismo de compuertas o brazos actuadores [Patente 1]. **Nota:** para un prototipo de alcance universitario/pequeña escala se recomienda iniciar con carga manual sobre una guía o tope fijo, dejando la banda transportadora como escalamiento futuro. | U.A |
| 09/09/26 | E | **Fuerzas** | La estructura debe soportar cargas de hasta 50 kg (peso del residuo a analizar). Los actuadores deben ejercer fuerzas controladas para la manipulación del material. La plataforma debe ser estable ante vibraciones para no afectar las mediciones de los sensores. | U.A |
| 10/09/26 | E | **Energía** | El sistema debe funcionar con alimentación de 220V AC (estándar industrial) y contar con una fuente de alimentación conmutada que proporcione los voltajes necesarios para el ESP32 (3.3V) y los sensores (5V o 12V según el caso). Opcional: batería de respaldo ante cortes de energía. Consumo máximo estimado: <50W [9]. | E.J |
| 10/09/26 | E | **Materia** | La estructura debe ser fabricada con materiales reciclados o reciclables (ej. acero reciclado, plásticos recuperados), alineada con economía circular. Componentes electrónicos de bajo costo y fácilmente reemplazables. Debe operar en entornos con polvo o humedad (grado de protección mínimo IP54, norma IEC 60529) [4][6]. | E.J |
| 26/09/26 | E | **Señales (Información)** | Los sensores deben generar señales eléctricas (analógicas o digitales) que el ESP32 pueda procesar: peso (celda de carga + HX711, 24 bits), volumen (sensor ultrasónico), humedad (DHT22), imagen (cámara ESP32-CAM). Las señales deben ser acondicionadas y filtradas para minimizar ruido [24][9]. | E.J |
| 11/09/26 | E | **Control** | El ESP32 debe ejecutar un firmware que realice la adquisición de datos de todos los sensores de forma secuencial o paralela, controle actuadores (servos, motores) si existen, gestione la comunicación Wi-Fi hacia la nube (ThingSpeak, Firebase u otra) y permita calibración periódica de los sensores [9][26]. | E.J |
| 11/09/26 | E | **Electrónico (hardware)** | La estación debe integrar: ESP32, celda de carga + HX711, sensor ultrasónico HC-SR04, sensor DHT22, cámara ESP32-CAM, módulo Wi-Fi integrado, fuente de alimentación, pantalla LCD/LED. Todos montados en una PCB personalizada [24][25]. | S.J |
| 10/09/26 | E | **Software** | La plataforma digital debe incluir: base de datos de empresas y residuos; panel de administración; algoritmo de matching (reglas o NLP); módulo de autoevaluación; dashboards de impacto ambiental y económico (ahorro de recursos, reducción de CO2) [5][11][27]. | S.J |
| 11/09/26 | E | **Comunicaciones** | La estación debe comunicarse con la nube vía Wi-Fi (802.11 b/g/n), enviando datos en JSON por HTTP o MQTT. La plataforma debe ser accesible desde web y móvil, con notificaciones en tiempo real ante coincidencias [9]. | S.J |
| 10/09/26 | E | **Seguridad** | Autenticación de usuarios (registro/login). Información de residuos confidencial hasta aceptar una coincidencia. Cumplimiento de normas básicas de seguridad eléctrica (aislamiento, tierra). Plan de respaldo de datos [28]. | S.J |
| 11/09/26 | E | **Ergonomía** | Fácil de operar por personal no técnico: botón de inicio, LEDs de estado, pantalla con instrucciones simples. Altura de bandeja de carga cómoda para operador de pie (~90 cm). Interfaz web intuitiva y accesible. | C.P |
| 11/09/26 | E | **Fabricación** | Componentes mecánicos de bajo costo: corte/doblado de planchas metálicas, impresión 3D para piezas plásticas. PCB con componentes SMD o pasantes según disponibilidad. Priorizar componentes del mercado local peruano. | C.P |
| 11/09/26 | E | **Control de calidad** | Pruebas de calibración de sensores (peso con masas patrón, distancia con objetos de tamaño conocido). Documentar errores máximos admisibles (ej. error de peso <2%, en línea con resultados preliminares de balanzas IoT de bajo costo). Validar consistencia de datos ingresados por empresas [24]. | C.P |
| 10/09/26 | E | **Montaje** | Ensamblaje sencillo, instrucciones claras, conexiones tipo plug-and-play entre módulos (sensores, ESP32, fuente). Componentes etiquetados para mantenimiento y reemplazo. | C.P |
| 11/09/26 | E | **Transporte** | Prototipo transportable en caja o maletín, peso total <15 kg, con asas o puntos de sujeción. Versión final apta para envío por mensajería con embalaje protector. | Z.A |
| 11/09/26 | D | **Uso** | Usuarios: PYMES generadoras de residuos y empresas que buscan materia prima secundaria (personal de producción o medio ambiente). Flujo: 1) Colocar residuo; 2) Iniciar medición; 3) Registro/caracterización; 4) Publicación en plataforma; 5) Búsqueda de compradores; 6) Notificaciones a empresas [17][19][20][21][22][23]. | Z.A |
| 11/09/26 | E | **Mantenimiento** | Mantenimiento periódico: limpieza de sensores (cámara, ultrasónico), recalibración de celda de carga cada 3 meses, actualización de firmware ESP32 vía OTA. Monitoreo remoto del estado de sensores. Componentes críticos reemplazables [9]. | Z.A |
| 11/09/26 | E | **Costos** | Costo total del prototipo (hardware + software) por debajo del monto que defina el equipo, para accesibilidad a PYMES *(pendiente de cotización — referencia orientativa: S/. 400–800 en componentes, según prototipos IoT académicos comparables)*. Costo de producción en serie por definir tras análisis de escalamiento. Plataforma gratuita en su versión básica, con opciones premium. | Z.A |
| 10/09/26 | E | **Plazos** | El desarrollo debe seguir el cronograma del proyecto integrador: investigación, diseño, selección de componentes, construcción, programación, pruebas y validación. | Z.A |

## Observaciones de la corrección

- Se corrigió la fecha de la fila "Geometría" (8/11/26 → 08/09/26) para mantener la secuencia cronológica.
- Se añadió una nota en "Cinemática" recomendando iniciar con carga manual/guía fija (proyecto de pequeña escala / prototipo universitario); la banda transportadora queda como escalamiento futuro.
- Se incorporaron referencias [24]–[28] y [Patente 25], encontradas para sustentar medición de peso (HX711), clasificación automatizada de residuos y plataformas de intercambio industrial.
- El campo "Costos" seguía con placeholders "X soles"; se dejó como valor pendiente de cotización real del equipo, con una referencia orientativa de prototipos IoT académicos comparables.

## Bibliografía

1. AIDIMME, "Proyecto SYMBINET - Portal de iniciativas de simbiosis industrial," Instituto Tecnológico Metalmecánico, Mueble, Madera, Embalaje y Afines, 2021.
2. AIDIMME, "AIDIMME impulsa la simbiosis industrial en la Comunidad Valenciana," *Actualidad AIDIMME*, 7 de abril de 2026.
3. AIDIMME, "AIDIMME desarrolla un demostrador sobre simbiosis industrial en el marco del proyecto europeo ResC4EU," *Actualidad AIDIMME*, 12 de julio de 2026.
4. ARVET, "Simbiosis industrial," *ARVET Blog*, s. f.
5. C. Davis and G. Aid, "Machine learning-assisted industrial symbiosis: Testing the ability of word vectors to estimate similarity for material substitutions," *Journal of Industrial Ecology*, vol. 26, no. 1, pp. 27–43, 2022, doi: 10.1111/jiec.13245.
6. European Commission, "CircLean: European industrial symbiosis network and label," 2022.
7. Gobierno del Reino Unido, "Industrial symbiosis: Drivers, barriers, benefits and costs," Research Report, 2025.
8. B. Herdianto, "ReFound: An automated redistribution system to turn lost goods into social value," GitHub repository, 2026.
9. IIoT World, "Coreflux: Manufacturing AI on a $35 device," 3 de mayo de 2026.
10. Instituto Tecnológico de Informática (ITI), "Proyecto SYMBINET: Ecosistema digital para simbiosis industrial," 2023.
11. A. M. Pegado et al., "Industry modular operating system: A framework for collaborative manufacturing," *IEEE Access*, vol. 13, 2025.
12. Quatra, "Quatra galardonada con la CircLean Label de Sostenibilidad," 15 de marzo de 2026.
13. R. S. Saputra, Juwari, and M. Y. Asyhari, "Prototype Smart Waste Exchange untuk penukaran botol plastik berbasis Internet of Things dengan ESP32," *Prosiding Seminar Nasional Informatika*, vol. 3, pp. 540–551, 2025.
14. **[24]** J. L. Pua Castro, S. V. Zabala Calderón, Á. A. Rodríguez Aya, y R. H. Polanco Contreras, "Diseño de un prototipo basado en IoT para la medición de residuos orgánicos aprovechables en unidades habitacionales," *Publicaciones e Investigación*, vol. 16, no. Extra 4, 2022. [Enlace](https://dialnet.unirioja.es/ejemplar/626061)
15. **[26]** "Autonomous knowledge-based smart waste collection system," US20240211898A1, 2024. [Enlace](https://patents.google.com/patent/US20240211898A1/en)
16. **[27]** Empresas por el Clima, "Byproductplace: plataforma de intercambio de subproductos y residuos," s. f. [Enlace](https://empresasporelclima.es/actualidad/4722-plataforma-de-intercambio-de-subproductos-y-residuos)
17. **[28]** Eurofins Environment, "Qué es la plataforma eSIR para el traslado de residuos," 2025. [Enlace](https://www.eurofins-environment.es/es/esir-plataforma-residuos/)

## Patentes

1. H. Kim and J. Yoo, "Recyclable waste collection device with AI camera and sorting robot," KR102453849B1, Oct. 14, 2022.
2. F. Waite, D. Sigmund Jr., M. Hatfield, and M. Perry, "Waste container with weight-measurement system," US20170211969A1, Jul. 27, 2017.
3. "Sistema de transporte y clasificación de residuos por tamaño," ES1314846U.
4. Z. Yeo, J. S. C. Low, D. Z. L. Tan, S. Y. Chung, T. B. Tjandra, and J. Ignatius, "A collaboration platform for enabling industrial symbiosis…," *Procedia CIRP*, vol. 80, pp. 643–648, 2019, doi: 10.1016/j.procir.2019.01.015.
5. V. Krishnamurthy et al., "Automatic sorting of waste," US20210371196A1, Dec. 2, 2021.
6. R. S. Saputra, Juwari, and M. Y. Asyhari, "Prototype Smart Waste Exchange…," *Prosiding Seminar Nasional Informatika*, vol. 3, 2025.
7. B. Herdianto, "ReFound: An automated redistribution system…," GitHub repository, 2026.
8. **[25]** G. E. Ebinger, "Automated waste scaling system and method of weighing and documenting waste data," US7511234B1, 2009. [Enlace](https://patents.google.com/patent/US7511234B1/en)
