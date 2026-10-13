# Actividad 4: sentimiento de reseñas en español

## Objetivo
Comparar BERT multilingüe, DistilBERT multilingüe y XLM-RoBERTa en Google Colab con GPU.

## Ejecución
1. Abrir Actividad_4_PLN_Comparacion_Modelos.ipynb en Colab.
2. Activar GPU T4. Instalar dependencias y reiniciar si se solicita.
3. Agregar HF_TOKEN a Secrets o usar login interactivo. No incluir credenciales en GitHub.
4. Ejecutar celdas en orden. Consultar requirements.txt y condiciones_ejecucion.json.

## Datos
mteb/amazon_reviews_multi, revisión c379a6705fec24a2493fa68e011692605f44e119, archivos es/validation.jsonl y es/test.jsonl.
Etiquetas: 0–4 representan 1–5 estrellas; 1–2 NEG, 3 NEU, 4–5 POS.
Se usan 600 reseñas de validación y 600 de prueba, sin duplicados de texto entre muestras.
No se realizó entrenamiento. Las estrellas son etiquetas indirectas de sentimiento.

## Protocolo
Float32, GPU, max_length=256, batch_size=16, semilla=42.
Selección por validación; prueba separada. Latencia individual p50/p95 y benchmark por lotes.
Las métricas de prueba no se usan para cambiar la selección.

## Resultados y decisión
- **BERT multilingüe**: F1 macro de validación 0.695; F1 macro de prueba 0.771; recall NEG en prueba 0.896; latencia individual p95 34.9 ms.
- **DistilBERT multilingüe**: F1 macro de validación 0.517; F1 macro de prueba 0.536; recall NEG en prueba 0.696; latencia individual p95 5.3 ms.
- **XLM-RoBERTa**: F1 macro de validación 0.587; F1 macro de prueba 0.609; recall NEG en prueba 0.867; latencia individual p95 63.4 ms.

**Modelo elegido con validación: BERT multilingüe.** Ningún modelo cumple todos los umbrales: candidato exploratorio, requiere mejoras. En prueba obtuvo accuracy=0.820, precision macro=0.775, recall macro=0.769 y F1 macro=0.771. Cumple los umbrales de calidad en prueba.

La elección equilibra calidad, detección de opiniones negativas y eficiencia según la regla previa. Revisar los errores, especialmente opiniones mixtas y la clase neutral. Si el desempeño es insuficiente, considerar revisión del etiquetado, datos del dominio y eventualmente fine-tuning con particiones independientes. Las puntuaciones de confianza no sustituyen las métricas. La muestra y el hardware limitan la generalización; se requiere validación con reseñas propias y análisis de posible solapamiento con entrenamiento antes de uso real.

## Archivos
comparacion_modelos.csv: métricas y eficiencia.
predicciones_*.csv y metricas_*.json: salidas reales y métricas por clase.
muestra_*.csv: trazabilidad local de las muestras.
condiciones_ejecucion.json: versiones, GPU, revisiones y hashes.
*.png: gráficas; conclusiones_parciales.md: análisis generado tras ejecutar.

## Repositorio recomendado
Agregar el notebook a notebooks/, documentación a README.md y evidencias a resultados/.
Este paquete no crea ni publica un repositorio. Revisar licencias y permisos del conjunto de datos
antes de redistribuir las muestras; puede publicarse solo el procedimiento de descarga y sus hashes.
No subir tokens, carpetas de caché ni información personal añadida por usuarios.

## Limitaciones
Evaluación exploratoria, sin garantía de ausencia de datos conocidos por modelos de terceros.
Reseñas públicas históricas y etiquetas por estrellas; no equivalen a tickets ni a clientes actuales.
Los tiempos dependen del entorno y excluyen red. Evaluar errores y datos propios antes de despliegue.
