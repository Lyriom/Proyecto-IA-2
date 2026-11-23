# Predicción del rendimiento académico estudiantil

**Inteligencia Artificial 2 · Entregable 2**  
Josue Riera · Jose Perez · Joao Conde  
Docente: Enrique Vinicio Carrera

Evaluamos si una red neuronal pequeña, con las notas de los dos primeros períodos, mejora la detección de estudiantes que reprueban frente a la regla `G2 < 10`. La predicción se sitúa al cierre del segundo período.

## Ejecutar en Google Colab

1. Descarga el cuaderno del repositorio. Si GitHub lo muestra como `Proyecto IA 2` sin extensión, guarda el archivo original con el nombre `Proyecto_IA_2.ipynb`.
2. Abre [Google Colab](https://colab.research.google.com/) y usa **Archivo → Subir cuaderno** para abrir ese `.ipynb`.
3. Descarga [Student Performance de UCI](https://archive.ics.uci.edu/dataset/320/student+performance). Cuando aparezca el selector de la primera celda, sube `student+performance.zip` o el CSV de Portugués `student-por.csv`. Se esperan 649 registros.
4. Ejecuta las celdas en orden. El cuaderno usa pandas, NumPy, scikit-learn, Matplotlib y TensorFlow/Keras. Si el entorno indica que falta TensorFlow, ejecuta esta celda antes del entrenamiento:

   ```python
   %pip install tensorflow==2.21.0
   ```

   Si el entorno solicita un reinicio, reinícialo y ejecuta de nuevo las celdas desde el comienzo.
5. La última celda muestra la tabla en validación, la curva de pérdida y las matrices de confusión.

## Diseño del experimento

| Elemento | Configuración |
| --- | --- |
| Entradas | `G1` y `G2`. Para la red se dividen entre 20. |
| Etiqueta | `G3 < 10` → reprobado (1). Desde 10 → aprobado (0). |
| Partición | 389 entrenamiento, 130 validación y 130 prueba. Estratificación por clase y semilla 42. |
| Líneas base | Clase mayoritaria de entrenamiento y regla `G2 < 10`. |
| Red | 2 entradas, 8 neuronas ReLU y 1 salida sigmoide. Umbral 0,50. |
| Entrenamiento | Adam (0,001), entropía cruzada binaria, lotes de 16 y 100 épocas. Se recuperan los pesos de menor pérdida en validación. |
| Métrica principal | F1 de reprobado, acompañado de exactitud, precisión y sensibilidad. |

## Resultados preliminares

Resultados sobre los mismos 130 registros de validación, 20 de ellos reprobados.

| Método | Exactitud | Precisión | Sensibilidad | F1 |
| --- | ---: | ---: | ---: | ---: |
| Clase mayoritaria | 0,846 | 0,000 | 0,000 | 0,000 |
| Regla `G2 < 10` | 0,923 | 0,679 | 0,950 | 0,792 |
| Red neuronal | 0,854 | 1,000 | 0,050 | 0,095 |

Precisión, sensibilidad y F1 corresponden a la clase reprobado. La regla detectó 19 de 20 reprobados y generó 9 falsas alertas. La red detectó 1 de 20 y emitió una sola alerta. La partición de prueba queda reservada para la evaluación final.

## Datos y alcance

Los datos públicos provienen de dos colegios portugueses. Fuente: P. Cortez, *Student Performance*, UCI Machine Learning Repository, DOI [10.24432/C5TG7T](https://doi.org/10.24432/C5TG7T). El uso es experimental y sus conclusiones se limitan a los datos y al protocolo descritos.

Se utilizó ChatGPT como apoyo para preparar y depurar el código y redactar la documentación. Las métricas incluidas proceden de la ejecución del cuaderno en Google Colab.
