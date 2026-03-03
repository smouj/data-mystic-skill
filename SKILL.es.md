name: data-mystic
version: 2.1.0
description: Reconocimiento avanzado de patrones y generación de insights a partir de datos estructurados/no estructurados
author: SMOUJBOT
tags:
  - pattern-recognition
  - data-alchemy
  - insight-generation
  - anomaly-detection
  - correlation-analysis
  - time-series
dependencies:
  - python>=3.9
  - pandas>=2.0.0
  - numpy>=1.24.0
  - scikit-learn>=1.3.0
  - prophet>=1.1.0
  - matplotlib>=3.7.0
  - seaborn>=0.12.0
  - networkx>=3.0.0
  - scipy>=1.10.0
  - imbalanced-learn>=0.10.0
  - ydata-profiling>=4.5.0
system_requirements:
  memory_min: 2GB
  memory_recommended: 8GB
  disk_space: 500MB
  python_packages_install: pip install -r requirements.txt
---

# Data Mystic

Motor de reconocimiento de patrones que revela estructuras, correlaciones y anomalías ocultas en conjuntos de datos aparentemente aleatorios. Transforma datos crudos en insights accionables mediante alquimia estadística y machine learning.

## Propósito

Data Mystic aborda escenarios del mundo real donde los datos parecen caóticos pero contienen patrones latentes:

- **Análisis de comportamiento de usuario**: Detectar cambios sutiles en el comportamiento a partir de logs de uso de aplicaciones antes de que ocurra churn
- **Detección de anomalías financieras**: Identificar patrones de fraude en flujos de transacciones que evaden sistemas basados en reglas
- **Correlación de sensores IoT**: Encontrar dependencias ocultas entre lecturas de sensores que predicen fallos de equipamiento
- **Predicción de series temporales**: Extraer estacionalidad y tendencias de métricas operacionales ruidosas
- **Segmentación de clientes**: Descubrir agrupaciones naturales en datos de transacciones más allá del análisis RFM simple
- **Optimización de supply chain**: Identificar cuellos de botella y efectos en cascada en datos logísticos
- **Minería de logs de seguridad**: Detectar patrones APT en logs de firewall que abarcan semanas

## Alcance

### Comandos

```bash
# Analiza CSV/JSON para patrones de correlación con significance estadística
data-mystic correlate --input data.csv --target conversion_rate --threshold 0.3 --method pearson/spearman/kendall

# Detecta anomalías usando isolation forest o local outlier factor
data-mystic anomalies --input metrics.jsonl --contamination 0.01 --method isolation/LOF --output anomalies.json

# Descomposición y forecast de series temporales con Prophet
data-mystic forecast --input timeseries.csv --date-column timestamp --value-field revenue --periods 30 --interval daily/weekly/monthly

# Análisis de clustering con determinación automática de clusters óptimos
data-mystic cluster --input user_features.csv --method kmeans/dbscan/hierarchical --auto-k true --max-clusters 10

 # Minería de patrones en datos secuenciales/categóricos
data-mystic sequence --input clickstream.log --pattern-length 3-5 --min-support 0.02 --algorithm prefixspan/span

# Análisis de red/grafo para datos de relaciones
data-mystic graph --input relationships.csv --source user_id --target product_id --detect-communities true --centrality pagerank

# Perfilado completo de datos con generación automática de insights
data-mystic profile --input dataset.csv --report-format html/markdown/json --minimal true/false
```

## Proceso de Trabajo

### 1. Ingestión y Validación de Datos
```bash
# Comando real con flags actuales
data-mystic profile --input /var/log/app/events_2024.jsonl \
  --date-column @timestamp \
  --categorical-threshold 0.05 \
  --missing-threshold 0.3
```

Valida existencia de archivo, compatibilidad de formato y esquema básico. Rechaza archivos con valores faltantes excesivos (>30% por defecto). Detecta automáticamente CSV/JSON/JSONL/Parquet.

### 2. Análisis Exploratorio
Calcula:
- Estadísticas de distribución (asimetría, curtosis)
- Patrones de valores faltantes (MCAR/MAR/MNJ)
- Cardinalidad por columna
- Matriz de correlación con p-values
- Span temporal para columnas de fecha

### 3. Fase de Detección de Patrones
Ejecuta el análisis seleccionado:

**Correlation analysis**:
```bash
data-mystic correlate --input sales_data.csv \
  --target revenue \
  --threshold 0.25 \
  --method spearman \
  --p-value 0.05 \
  --partial-control month,region
```
Ejecuta correlación parcial controlando por confounders. Corrección de Bonferroni para pruebas múltiples.

**Anomaly detection**:
```bash
data-mystic anomalies --input server_metrics.jsonl \
  --method isolation \
  --contamination 0.001 \
  --features cpu,memory,network_in,network_out \
  --time-window 5m
```
Retorna scores de anomalía y flags binarias. Exporta a JSON con timestamps.

### 4. Generación de Insights
Aplica plantillas de generación de lenguaje natural:
- "La columna X muestra fuerte correlación (r=0.82, p<0.001) con el target"
- "Detectadas 147 anomalías en últimas 24h (esperadas: 12)"
- "Conteo óptimo de clusters: 5 (score de silueta: 0.73)"
- "Patrón estacional: ciclo semanal con 42% de varianza explicada"

### 5. Salida y Visualización
Genera:
- Reporte resumen (markdown/HTML)
- CSV de anomalías con scores
- Heatmap de correlación (PNG)
- Visualización de clusters (t-SNE/UMAP)
- Gráfico de forecast con intervalos de confianza

## Reglas de Oro

1. **Nunca asumir distribuciones Gaussianas**: Siempre testear normalidad (Shapiro-Wilk) antes de aplicar métodos paramétricos. Por defecto usar no-paramétricos (Spearman, Mann-Whitney) cuando p<0.05 en tests de normalidad.

2. **Corrección de comparaciones múltiples obligatoria**: Cuando se prueban >5 hipótesis, aplicar Benjamini-Hochberg FDR. Nunca reportar p-values sin corregir.

3. **Los umbrales de anomalía deben ser validados**: El parámetro contamination no puede exceder 5% sin flag `--force` explícito. Siempre revisar manualmente top 20 anomalías.

4. **Prevención de temporal leakage**: En forecast, dividir datos temporalmente (no splits aleatorios). Usar `--test-size` como últimos N períodos, no muestra aleatoria.

5. **Contexto de escalado de features**: Estandarizar solo dentro del split de entrenamiento. Nunca escalar con datos futuros. Documentar método de escalado en outputs.

6. **Encoding categórico**: Usar target encoding para alta cardinalidad (>20 niveles) con validación cruzada. Nunca usar label encoding para datos nominales.

7. **Estabilidad de clusters**: Ejecutar clustering con 10 semillas aleatorias; coeficiente de variación < 0.1 requerido para clusters estables. Reportar inestabilidad si se viola.

8. **Muestra viable mínima**: Requerir mínimo 30 muestras por grupo para tests estadísticos. Abortar con error si se viola.

## Ejemplos

### Ejemplo 1: Detectar Fraude en Flujo de Transacciones

```bash
# Input: transactions.csv (10M filas, columnas: user_id, amount, merchant_category, timestamp, ip_country)
data-mystic anomalies \
  --input transactions.csv \
  --features amount,merchant_category_freq \
  --method isolation \
  --contamination 0.001 \
  --time-column timestamp \
  --user-column user_id \
  --output fraud_indicators.csv
```

Output fraud_indicators.csv:
```
timestamp,user_id,amount,anomaly_score,is_anomaly,reason
2024-01-15 14:23:01,user_8892,15000,0.89,true,amount-outlier
2024-01-15 14:25:17,user_1120,8900,0.76,true,geographic-jump
```

Extracto de reporte:
```
=== ANOMALY DETECTION REPORT ===
Total transactions analyzed: 10,234,561
Anomalies detected: 12,345 (0.12%)
Expected under null: 10,234 (0.10%)
P-value (Poisson): 0.03

Top patterns:
- 67% of anomalies involve amounts >5x user's 30-day average
- 23% involve country changes within 2 hours
- 11% involve merchants never previously used

Recommendation: Trigger step-up authentication for transactions scoring >0.75
```

### Ejemplo 2: Forecast de Capacidad de Servidor

```bash
# Input: cpu_metrics.csv (2 años de datos horarios)
data-mystic forecast \
  --input cpu_metrics.csv \
  --date-column hour \
  --value-field cpu_utilization \
  --periods 168 \
  --interval hourly \
  --seasonality-mode multiplicative \
  --changepoint-prior-scale 0.05 \
  --output forecast_2024_Q1.json
```

Output forecast_2024_Q1.json:
```json
{
  "model": "Prophet",
  "metrics": {
    "mape": 8.3,
    "rmse": 4.2,
    "coverage_95": 0.89
  },
  "forecast": [
    {"ds": "2024-01-01 00:00:00", "yhat": 45.2, "yhat_lower": 41.1, "yhat_upper": 49.3},
    {"ds": "2024-01-01 01:00:00", "yhat": 42.1, "yhat_lower": 38.0, "yhat_upper": 46.2}
  ],
  "seasonality": {
    "weekly": {
      "period": 168,
      "mode": "multiplicative",
      "fourier_order": 5
    }
  }
}
```

### Ejemplo 3: Segmentación de Clientes desde Logs de Comportamiento

```bash
# Input: user_features.csv (features: session_count, avg_session_duration, feature_usage_vector)
data-mystic cluster \
  --input user_features.csv \
  --method kmeans \
  --auto-k true \
  --max-clusters 12 \
  --elbow-threshold 0.15 \
  --features session_count,avg_duration,feature_1,feature_2,feature_3 \
  --scale-standard true \
  --output clusters/
```

Genera:
- `clusters/centers.csv`: centroides de clusters
- `clabels assignments.csv`: mapeo user_id → cluster
- ``clusters/visualization.png`: plot t-SNE coloreado por cluster
- `clusters/profile.md`: características de clusters

```
# Perfiles de Clusters

## Cluster 0 (Usuarios Power) - 2,341 usuarios (5.2%)
- Sesiones/día promedio: 8.3 (vs población 2.1)
- Adopción de features: 92% (vs 47%)
- Tasa de churn: 2%/mes (vs 8%)
**Acción**: Candidatos a upsell premium

## Cluster 3 (En Riesgo) - 5,112 usuarios (11.4%)
- Duración de sesión decline 15% WoW
- Uso de features cayó 60% en 30 días
- Último login >14 días: 67%
**Acción**: Campaña de winback
```

## Variables de Entorno

```bash
# Establecer para resultados reproducibles
DATA_MYSTIC_RANDOM_SEED=42

# Configurar límites de memoria (en MB)
DATA_MYSTIC_MEMORY_LIMIT=4096

# Habilitar logging debug
DATA_MYSTIC_LOG_LEVEL=DEBUG

# Threshold de correlación por defecto personalizado
DATA_MYSTIC_DEFAULT_CORRELATION_THRESHOLD=0.2

# Threads de procesamiento paralelo
DATA_MYSTIC_N_JOBS=-1  # todos los cores

# Deshabilitar auto-visualización (más rápido)
DATA_MYSTIC_NO_PLOT=true
```

## Requisitos

```
pandas>=2.0.0
numpy>=1.24.0
scikit-learn>=1.3.0
prophet>=1.1.0
matplotlib>=3.7.0
seaborn>=0.12.0
networkx>=3.0.0
scipy>=1.10.0
imbalanced-learn>=0.10.0
ydata-profiling>=4.5.0
joblib>=1.3.0
```

Instalación:
```bash
pip install data-mystic
```

## Verificación

Comprobar instalación y capacidades:
```bash
# Test de humo básico
data-mystic --version

# Validar dependencias
data-mystic doctor

# Ejecutar benchmark en datos de ejemplo
data-mystic profile --input tests/sample/sales_sample.csv --report-format markdown --minimal true

# Verificar todos los subcomandos disponibles
data-mystic list-commands
```

Salida esperada para `data-mystic doctor`:
```
✓ Python 3.11.5 detectado
✓ pandas 2.0.3 (OK)
✓ scikit-learn 1.3.0 (OK)
✓ prophet 1.1.5 (OK)
✓ matplotlib 3.7.1 (OK)
✓ Todas las dependencias satisfechas
✓ Memoria disponible: 15.2GB (mín: 2GB)
✓ Espacio en disco: 142GB (mín: 500MB)
Listo para alquimia de datos.
```

## Solución de Problemas

### "MemoryError: Unable to allocate array"
**Causa**: Conjunto de datos demasiado grande para memoria.  
**Fix**: Usar sampling o chunking:
```bash
data-mystic profile --input huge.csv --sample 100000  # primeras 100k filas
data-mystic correlate --input huge.csv --chunk-size 50000
```

### "ValueError: Cannot convert non-finite values"
**Causa**: Valores infinitos o NaN en columnas numéricas.  
**Fix**: Habilitar imputación automática:
```bash
data-mystic profile --input dirty.csv --impute-strategy median
```

### "RuntimeError: Distributed scheduler failed"
**Causa**: Procesamiento paralelo con demasiados threads.  
**Fix**: Limitar threads:
```bash
export DATA_MYSTIC_N_JOBS=2
```

### "ImportError: Prophet dependencies missing"
**Causa**: Fallo en instalación de PyStan.  
**Fix**: Instalar via conda o usar backend cmdstanpy:
```bash
pip install prophet
# OR
conda install -c conda-forge prophet
```

### "No patterns detected (all correlations < threshold)"
**Causa**: Threshold muy alto o realmente no hay relaciones lineales.  
**Fix**: Bajar threshold o cambiar método:
```bash
data-mystic correlate --input data.csv --threshold 0.1 --method spearman
```

### "Cluster instability detected"
**Causa**: Datos no se agrupan naturalmente o features no escalados.  
**Fix**: Probar método diferente o verificar escalado:
```bash
data-mystic cluster --input data.csv --method dbscan --eps 0.5 --min-samples 10
```

## Comandos de Rollback

Data Mystic es de solo lectura por diseño. No se necesita rollback. Sin embargo, para deshacer outputs de análisis:

```bash
# Eliminar reportes generados
rm -rf analysis_output/
rm analysis_report.html

# Limpiar datasets procesados en caché
rm ~/.cache/data-mystic/*.parquet

# Resetear configuración a defaults
unset DATA_MYSTIC_RANDOM_SEED
unset DATA_MYSTIC_LOG_LEVEL

# Si se crearon archivos temporales grandes, limpiarlos
find /tmp -name "data-mystic-*" -type f -mtime +1 -delete
```

Para modificación accidental de datos (si se usó `--overwrite` en archivo fuente):
```bash
# Fuentes versionadas: git restore data.csv
# No versionadas: Restaurar desde backup si se creó via flag --backup
data-mystic restore-backup --input data.csv --backup-file data.csv.backup_20240115_142300
```

## Ajuste de Performance

Para datasets >1GB:
```bash
# Usar todos los cores de CPU
export DATA_MYSTIC_N_JOBS=-1

# Deshabilitar plots para 40% de speedup
export DATA_MYSTIC_NO_PLOT=true

# Aumentar chunk size para lecturas secuenciales
data-mystic profile --input massive.csv --chunk-size 100000
```

## Uso Avanzado

Pipear comandos:
```bash
# Extraer anomalías, luego clusterizarlas
data-mystic anomalies --input events.csv --method isolation | \
  data-mystic cluster --method kmeans --auto-k true
```

Combinar con jq para filtrado JSON:
```bash
data-mystic forecast --input metrics.jsonl | \
  jq '.forecast[] | select(.yhat_upper > 90)'
```

Programar detección de anomalías cada hora:
```bash
cron: 0 * * * * data-mystic anomalies --input /var/log/hourly.jsonl --output /alerts/$(date +\%Y\%m\%d_\%H).json
```

---

Data Mystic opera bajo constricción: correlaciones ≠ causalidad. Todos los insights requieren validación de dominio antes de acción.
```