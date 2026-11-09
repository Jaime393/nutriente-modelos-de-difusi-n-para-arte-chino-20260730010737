# nutriente-modelos-de-difusi-n-para-arte-chino-20260730010737
Cazado en China. IA exótica 2026

## 1. Qué es un modelo de difusión

Un modelo de difusión genera una imagen al revés de como se destruye: se parte de ruido gaussiano y se quita el ruido paso a paso hasta que aparece la imagen. El modelo de referencia es el DDPM de Ho, Jain y Abbeel (2020): el proceso directo añade ruido en 1.000 pasos, con un programa lineal de β desde 10⁻⁴ hasta 0,02, y la red aprende a predecir el ruido añadido en cada paso, entrenada con error cuadrático medio. En CIFAR-10 sin condicionar obtuvo un FID de 3,17 y un Inception Score de 9,46, la primera vez que un modelo de difusión igualaba a las GAN sin entrenamiento adversarial. [1]

La variante que lo convirtió en herramienta práctica es la difusión latente de Rombach y colaboradores (2022): el bucle de denoising no corre sobre píxeles sino en el espacio latente de un autoencoder variacional congelado, con factores de submuestreo de 4 a 32 (las variantes LDM-4 y LDM-8 fueron las mejores en el artículo). Esa es la arquitectura detrás de Stable Diffusion. [2]

Para el arte chino esto importa por una razón simple: un modelo de difusión no dibuja trazos, genera píxeles. Todo lo que sabe de un trazo lo aprende de los datos. Si los datos no enseñan la estructura del carácter o la composición del paisaje, el modelo la inventa, y se nota.

## 2. Por qué el arte chino es un caso duro

La caligrafía china tiene miles de años de historia y su dificultad para un generador está en que un carácter no es una forma, es una estructura: se compone de trazos en un orden, agrupados en radicales y componentes con relaciones espaciales precisas. Un trazo de más o de menos, o un componente fuera de lugar, no es un defecto de estilo: es otro carácter, o ninguno. Los modelos que tratan el carácter como una imagen plana rompen la estructura; por eso los trabajos serios condicionan la generación con el orden de los trazos, la descomposición en radicales o la posición de los componentes. [3][4][7]

Hay cinco escrituras clásicas — sello, clerical, regular, semicursiva y cursiva — y cada una es un sistema de trazos distinto: lo que en regular es un trazo recto, en cursiva puede ser una curva continua que une varios trazos. Un modelo que genera "caligrafía" sin controlar la escritura no genera caligrafía: genera manchas con aspecto de caligrafía. Calliffusion fue el primer modelo de difusión que generó en las cinco escrituras. [3]

La pintura tradicional, sobre todo el paisaje shanshui (montaña-agua), plantea el problema opuesto: no hay un carácter que deba salir exacto, pero sí una composición con reglas. No usa perspectiva lineal como la pintura occidental: la profundidad se construye con trazos de textura (cun), lavados de tinta y vacío (el blanco que se deja sin pintar). Técnicas como el feibai (blanco volante) o los cun pima y mi dian no son adornos: son la forma en que el pintor dice qué textura tiene la montaña. Un modelo entrenado solo con imágenes modernas no aprende esto; por eso los trabajos sobre shanshui construyen conjuntos de datos propios y mecanismos de control de la composición. [6]

## 3. Caligrafía: la línea de trabajo

El punto de partida es **Calliffusion** (Liao, Xia y Wang, 2023), el primer modelo de generación de caligrafía china basado en difusión. Su arquitectura se basa en DDPM con U-Net y se controla con texto descriptivo que especifica carácter, escritura y calígrafo; genera caracteres comunes en las cinco escrituras e imita los estilos de calígrafos famosos. Su resultado más llamativo es la transferencia con LoRA en un solo ejemplo: el estilo de un calígrafo se transfiere a caracteres que el modelo no vio, e incluso a símbolos fuera del dominio, como letras inglesas y dígitos. El propio artículo reconoce el límite: los resultados a veces tienen errores de trazo, con trazos que faltan o sobran. [3]

**Moyun** (Liu y colaboradores, 2025) sustituye la U-Net por Vision Mamba y añade el mecanismo de control TripleLabel: calígrafo, escritura y carácter como tres etiquetas que condicionan la generación. Genera caracteres individuales de 256×256. Lo entrenaron con su propio conjunto de datos, "Mobao": más de 1,9 millones de imágenes binarizadas de alta calidad (1.929.393 imágenes, de 2.681 calígrafos y 4.660 caracteres distintos, en seis tipos de escritura), construidas con SAM a partir de imágenes ruidosas de la web. En precisión estructural midieron un IoU de 0,810, frente a 0,325 de CalliGAN y 0,348 de ZiGAN. El antecedente GAN, ZiGAN, trabajaba con solo 9 subconjuntos de 6.000 imágenes cada uno, un estilo por subconjunto, y su conjunto de datos no es público. [4]

**InkDiffuser** (Shi y Zhang) ataca el problema de la tinta: los métodos actuales rinden mal el trazo y la morfología de la tinta, y el resultado se ve poco fluido. Es un marco de difusión de imagen a imagen, de un solo ejemplo, con un mecanismo de realce de alta frecuencia y una pérdida de estructura de tinta diferenciable (DIS) que regulariza la morfología de la tinta dentro del proceso de difusión. Su código está publicado en GitHub. [5]

La línea continúa en dos direcciones. **UniCalli** unifica en un solo modelo de difusión la generación y el reconocimiento a nivel de columna: entrenar ambas tareas juntas obliga al generador a respetar la estructura del carácter y le da al reconocedor los priors de estilo y composición. Su corpus tiene más de 8.000 obras digitalizadas de 93 calígrafos clásicos (entre ellos Wang Xizhi, Mi Fu y Ouyang Xun), con más de 4.000 obras anotadas con tipo de escritura, cajas por carácter y transcripciones. El mismo marco se extiende a escrituras antiguas: huesos oraculares y jeroglíficos egipcios. [7]

**CalliFormer** (Scientific Reports, 2025) pone el foco en la estructura: descompone el carácter en un grafo donde los nodos son componentes y las aristas sus relaciones espaciales, y guía la atención del transformador con esa estructura. Su conjunto de datos, CCTS-2025, tiene más de 47.700 imágenes de caracteres de 35 calígrafos, con la secuencia de componentes de cada carácter. En su evaluación obtuvo un MSE de 12,91, frente a 19,49 de CalliGAN y 26,02 de zi2zi, con solo un 18,7 % más de parámetros. El artículo documenta el problema que resuelve: en CalliGAN, basada en LSTM, la tasa de rotura de trazo en caracteres de más de 4 componentes sube hasta un 37 %. [8]

## 4. Pintura: el paisaje shanshui

**LFMDiff** (Li y Wang, 2025, npj Heritage Science) es el marco de difusión específico para paisaje tradicional chino. Usa un mecanismo jerárquico de LoRA local para conservar la composición global y mejorar los detalles locales por jerarquías, con rango dinámico y alfa dinámico que se adaptan. Construyeron su propio conjunto de datos, CTLPD, con 1.449 imágenes para generación condicionada por texto. El artículo deja claro el contexto: los grandes modelos chinos (Tongyi Wanxiang de Alibaba, RAPHAEL de SenseTime, WenXin de Baidu) mejoran la alineación entre semántica china e imagen, pero están entrenados sobre estilos modernos y no modelan la estructura fina del shanshui; la investigación específica sobre síntesis de paisaje chino sigue siendo escasa, en parte por la dificultad y en parte por la falta de datos. [6]

La otra vía es no entrenar desde cero sino guiar un modelo base. Un equipo describe un pipeline híbrido para animar shanshui: ruido de Perlin para generar el esqueleto del paisaje (variaciones continuas que imitan el flujo orgánico de la tinta), Stable Diffusion con ControlNet para rellenar el esqueleto con estilo y detalle sin perder la estructura, GPT-4 para redactar los prompts con el vocabulario del género, y AnimateDiff para el movimiento. Su punto de partida es la escasez: no hay suficientes pinturas shanshui digitalizadas de alta calidad para entrenar un modelo desde cero, y afinar un modelo de difusión con pocas imágenes no basta. [9]

La tercera vía es el LoRA cultural: Alibaba publicó un LoRA (pai-diffusion-artist-large-zh-lora-poem) afinado sobre su base PAI-Diffusion-Artist-Large-Zh para traducir la imaginería de la poesía clásica china —con prompts de las dinastías Tang y Song— en arte visual, en precisión fp16 y optimizado para hardware Ascend de Huawei. La propia documentación reconoce que los hiperparámetros de entrenamiento están poco documentados. [10]

## 5. Cómo se arma la pila

El patrón que se repite en todos los trabajos es el mismo, en tres capas:

1. **Base**: un modelo de difusión latente (la arquitectura de Stable Diffusion). Nadie entrena un modelo de difusión desde cero para arte chino: no hay datos suficientes y el coste no se justifica.
2. **Estilo**: un LoRA afinado sobre el corpus del estilo — un calígrafo, una escritura, un género de paisaje. Es lo que hace Calliffusion con un solo ejemplo y lo que hace el LoRA de poesía de Alibaba.
3. **Estructura**: un control que obligue a respetar la forma — ControlNet sobre un boceto, el orden de los trazos o la descomposición en radicales como condición, o la pérdida de estructura de tinta de InkDiffuser.

El cuello de botella no es el modelo, es el dato. Los conjuntos de datos que existen son pequeños (CTLPD: 1.449 imágenes; CCTS-2025: 47.700 caracteres de 35 calígrafos) o no públicos (el de ZiGAN), y el único grande (Mobao: 1,9 millones de imágenes) se construyó a medida con SAM a partir de imágenes ruidosas de la web. Sin un corpus limpio y anotado, el modelo aprende la apariencia del trazo y no su estructura.

## 6. Límites

- **Errores de trazo.** Es el fallo que todos los artículos reconocen: trazos que faltan, trazos de más, componentes rotos. Calliffusion lo admite en su propio resumen; CalliFormer mide hasta un 37 % de rotura en caracteres complejos con la arquitectura anterior. Un carácter con un trazo mal no es arte con un defecto: es un carácter mal escrito.
- **La evaluación mide píxeles, no caligrafía.** Los números que se publican — IoU de 0,810 en Moyun, MSE de 12,91 en CalliFormer — miden similitud de imagen contra una referencia. Ninguno mide si un calígrafo consideraría buena la pieza. La calidad artística sigue evaluándose a ojo.
- **El estilo se imita, el gesto no se entiende.** Los modelos condicionan por calígrafo, escritura y carácter, y generan caracteres que un calígrafo nunca escribió con su estilo (Moyun lo demuestra). Pero eso es interpolación estadística sobre un corpus: el modelo no sabe por qué Wang Xizhi alargaba un trazo, solo que lo hacía.
- **Los datos no son públicos en su mayoría.** Varios conjuntos de datos de estos trabajos no están publicados o no se indica dónde descargarlos. Reproducir un resultado exige reconstruir el corpus, y eso es trabajo de meses, no de días.
- **Lo generado no es caligrafía.** Un modelo de difusión produce una imagen que se parece a la caligrafía. La caligrafía es un acto: un cuerpo, un pincel, una respiración, una decisión por trazo. La imagen generada puede ser bella y puede ser útil — para restaurar inscripciones dañadas, para enseñar, para preservar — pero no es el acto. Confundir ambas cosas es el error que estos trabajos no cometen y que quien los use sí puede cometer.

## 7. NO SÉ

- NO SÉ si Mobao, CCTS-2025 o CTLPD se pueden descargar hoy: los artículos describen cómo se construyeron, no dónde están publicados.
- NO SÉ si el código de Calliffusion o de Moyun está publicado: InkDiffuser sí publica su código en GitHub; de los otros dos no verifiqué repositorio.
- NO SÉ qué significa "Cazado en China. IA exótica 2026", la línea original de este repo: se conserva tal cual, sin interpretar.
- NO SÉ si existe un modelo de difusión en producción que genere arte chino con aceptación de calígrafos o pintores profesionales: los trabajos citados son investigación académica, no productos.
- NO SÉ cuánto cuesta entrenar un LoRA de estilo de caligrafía ni cuántas imágenes hacen falta como mínimo: ningún artículo de los citados publica ese coste.
