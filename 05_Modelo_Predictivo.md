# 05_Modelo_Predictivo_y_Predicciones

## 1. Naturaleza del Enfoque Predictivo

Es importante aclarar desde el inicio que este trabajo no realiza **predicción prospectiva** (forecasting de eventos futuros). En su lugar, implementa un **modelo de detección out-of-sample** sobre transiciones críticas históricas ya conocidas. Esto se denomina corroboración histórica.

El objetivo del modelo predictivo es evaluar si el indicador AR1, calculado de forma rolling, contiene información útil para distinguir períodos de transición de períodos de baseline, de manera reproducible y sin data leakage.

## 2. Modelo Utilizado

Se empleó un modelo simple de **Regresión Logística** con las siguientes características:

- **Variable predictora principal**: AR1 (Absorption Ratio) calculado sobre ventanas móviles.
- **Variable objetivo (target)**: Binaria (1 = período de transición crítica documentada, 0 = baseline).
- **Split temporal estricto**: Entrenamiento en Fragmento 1 (pre-transición) y evaluación en Fragmento 2 (transición + post-transición).
- **Balanceo de clases**: `class_weight='balanced'` para manejar el desbalance natural entre períodos normales y de transición.
- **Escalado**: StandardScaler ajustado únicamente en datos de entrenamiento.

Este modelo sirve como baseline interpretable para evaluar la capacidad predictiva del indicador espectral.

## 3. Métricas de Desempeño

El desempeño se midió principalmente con:

- **AUC-ROC** (principal métrica de discriminación)
- **Recall y Precision** en umbral 0.5
- **Validación E3** mediante surrogates de permutación (200 iteraciones) para verificar que el desempeño no sea un artefacto estadístico.

Resultados resumidos por dominio (ver sección de Resultados para detalles completos):

- Mejor desempeño: Epidemiología (AUC 0.89)
- Desempeño sólido: Cryptocurrency, Ecology, Clima, Social Networks (AUC 0.81–0.83)
- Desempeño moderado: Finanzas y Seismology
- Señal más tenue: Migraciones, Guerras y Desigualdad

## 4. Limitaciones del Modelo Predictivo

- No constituye un sistema de alerta temprana en tiempo real.
- La "predicción" es validación histórica sobre eventos ya ocurridos.
- El modelo es univariado (solo AR1) y simple por diseño.
- En dominios con fuerte componente N-tipping o mediación institucional, el desempeño disminuye.
- No se realizaron pruebas de walk-forward ni validación prospectiva.

## 5. Perspectivas Futuras

Un sistema predictivo más avanzado podría incluir:

- Monitoreo multi-dominio de coherencia cruzada en tiempo real.
- Modelos más complejos (Random Forest, LSTM, etc.) sobre múltiples features espectrales.
- Integración de escalas temporales múltiples.
- Validación prospectiva en datos nuevos.

El modelo actual sirve como prueba de concepto de que el indicador AR1 contiene señal útil y reproducible.

---

**Estado**: Contenido base creado. Pendiente de expansión con código reproducible y más detalles técnicos.