# Fuentes — modelos de difusión para arte chino

Todas las fuentes se verificaron en su texto primario (artículo, acta o página del autor). Cada número del README tiene aquí su fuente.

1. **Ho, Jain y Abbeel (2020) — "Denoising Diffusion Probabilistic Models", NeurIPS 2020.**
   arXiv:2006.11239 — https://arxiv.org/abs/2006.11239v2 — DOI: 10.48550/arXiv.2006.11239
   Afirma: el proceso directo en 1.000 pasos con β lineal de 10⁻⁴ a 0,02; la red predice el ruido añadido y se entrena con MSE; FID 3,17 e Inception Score 9,46 en CIFAR-10 sin condicionar.

2. **Rombach, Blattmann, Lorenz, Esser y Ommer (2022) — "High-Resolution Image Synthesis with Latent Diffusion Models", CVPR 2022.**
   arXiv:2112.10752 — DOI: 10.48550/arXiv.2112.10752
   Afirma: el denoising corre en el espacio latente de un VAE congelado, con factores de submuestreo de 4 a 32; LDM-4 y LDM-8 fueron las mejores variantes. Es la arquitectura detrás de Stable Diffusion.

3. **Liao, Xia y Wang (2023) — "Calliffusion: Chinese Calligraphy Generation and Style Transfer with Diffusion Modeling", ICCC 2023.**
   arXiv:2305.19124 (enviado 30 may 2023) — DOI: 10.48550/arXiv.2305.19124
   Afirma: primer modelo de caligrafía china basado en difusión; arquitectura DDPM con U-Net; genera caracteres comunes en las cinco escrituras e imita estilos de calígrafos famosos; transferencia LoRA de un solo ejemplo a caracteres no vistos e incluso a letras inglesas y dígitos; los resultados a veces tienen errores de trazo.

4. **Liu, Mei, Zhang, Zhang, Dong y He (2025) — "Moyun: A Diffusion-Based Model for Style-Specific Chinese Calligraphy Generation", McGE '25, 27–28 oct 2025, Dublín.**
   DOI: 10.1145/3746278.3759378 — arXiv:2410.07618
   Afirma: Vision Mamba sustituye a la U-Net; control TripleLabel (calígrafo, escritura, carácter); genera caracteres de 256×256; conjunto de datos "Mobao" con 1.929.393 imágenes, 2.681 calígrafos, 4.660 caracteres, seis tipos de escritura; IoU estructural 0,810 frente a 0,325 de CalliGAN y 0,348 de ZiGAN; ZiGAN usaba 9 subconjuntos de 6.000 imágenes y su conjunto de datos no es público.

5. **Shi y Zhang — "InkDiffuser: High-Fidelity One-shot Chinese Calligraphy via Differentiable Morphological Optimization".**
   arXiv:2605.05865 — https://arxiv.org/html/2605.05865 — código: https://github.com/JingVIPLab/InkDiffuser
   Afirma: marco de difusión imagen a imagen, de un solo ejemplo; realce de alta frecuencia y pérdida de estructura de tinta diferenciable (DIS); Diff-Font fue pionero en usar difusión para fuentes con priors de orden de trazo.

6. **Li y Wang (2025) — "LFMDiff: generation of Chinese traditional landscape paintings based on diffusion model", npj Heritage Science 13:564.**
   https://www.nature.com/articles/s40494-025-02136-5 — DOI: 10.1038/s40494-025-02136-5
   Afirma: LoRA local jerárquico con rango y alfa dinámicos; conjunto de datos CTLPD con 1.449 imágenes; los modelos chinos (Tongyi Wanxiang, RAPHAEL, WenXin) no modelan la estructura fina del shanshui; técnicas de textura como feibai, pima cun y mi dian cun; el paisaje chino no usa perspectiva lineal.

7. **"UniCalli: A Unified Diffusion Framework for Column-Level Generation and Recognition of Chinese Calligraphy".**
   arXiv:2510.13745 — https://arxiv.org/html/2510.13745v2
   Afirma: un solo modelo de difusión (MMDiT) para generación y reconocimiento a nivel de columna; corpus de más de 8.000 obras de 93 calígrafos, más de 4.000 anotadas con tipo de escritura, cajas por carácter y transcripciones; se extiende a huesos oraculares y jeroglíficos egipcios.

8. **"CalliFormer: a structure-aware transformer for Chinese calligraphy generation", Scientific Reports (2025).**
   https://www.nature.com/articles/s41598-025-29262-1
   Afirma: el carácter se descompone en un grafo de componentes y relaciones espaciales; conjunto de datos CCTS-2025 con más de 47.700 imágenes de 35 calígrafos; MSE 12,91 frente a 19,49 de CalliGAN y 26,02 de zi2zi, con un 18,7 % más de parámetros; en CalliGAN la rotura de trazo en caracteres de más de 4 componentes sube hasta un 37 %.

9. **"Generative AI Shanshui Animation Enhancement using Perlin Noise and Diffusion Models" — relato de los autores, Springer Nature Communities, 5 feb 2026.**
   https://communities.springernature.com/amp/posts/bringing-ancient-art-to-life-breathing-life-into-shanshui-art-with-ai-and-perlin-noise
   Afirma: no hay suficientes pinturas shanshui digitalizadas de alta calidad para entrenar desde cero; afinar con pocas imágenes no basta; pipeline Perlin + Stable Diffusion + ControlNet + GPT-4 para los prompts + AnimateDiff para el movimiento.

10. **"Poetry in Pixels: Alibaba's Culturally-Aligned Diffusion LoRA" — nota sobre el modelo alibaba-pai/pai-diffusion-artist-large-zh-lora-poem, 22 abr 2026.**
    https://aichina.news/blog/poetry-in-pixels-alibabas-culturally-aligned-diffusion-lora-arrives-n6xcci/ — página del modelo: https://aichina.news/models/alibaba-pai/pai-diffusion-artist-large-zh-lora-poem/
    Afirma: LoRA afinado sobre la base PAI-Diffusion-Artist-Large-Zh para traducir poesía clásica china (prompts Tang y Song) en arte visual; precisión fp16; optimizado para hardware Ascend de Huawei; la documentación de hiperparámetros de entrenamiento es escasa. Nota: es una nota de prensa sobre el modelo, no el artículo del modelo.
