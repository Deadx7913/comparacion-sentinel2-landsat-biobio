# Sentinel-2 y Landsat en el Biobío medio

[![Abrir en Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Deadx7913/comparacion-sentinel2-landsat-biobio/blob/main/comparacion_sentinel2_landsat.ipynb)

Notebook que compara **Sentinel-2** y **Landsat 8/9** en el río Biobío entre Nacimiento y aguas arriba de Negrete, con las
confluencias del Vergara, el Duqueco y el Bureo. Mira primero una imagen de cada satélite, después construye la serie
temporal de cada uno en 9 puntos del río, compara los dos sensores y busca anomalías y eventos extremos.

Es un híbrido de los bloques 03, 04, 05 y 07 del taller
[*Datos FAIR, series temporales satelitales y detección de anomalías con Python*](https://github.com/alvaroparedesl/geomatica-aplicada/tree/main/talleres/fair-series-temporales)
del curso Geomática Aplicada.

## Qué hace cada bloque

| Bloque | Basado en | Contenido |
|---|---|---|
| 0 | todos | Entorno (descarga las funciones `utils/` del curso), parámetros y los 9 puntos |
| A | 03 | Un par de imágenes casi del mismo día: color verdadero, NDWI, NDVI, turbidez, temperatura (solo Landsat), zoom 10 m frente a 30 m y valor en cada punto |
| B | 04 | Serie temporal en los puntos con cada satélite: ventanas pequeñas, máscara de nubes, calibración y parquet con metadatos |
| C | nuevo | Comparación directa en los días que pasaron los dos satélites: sesgo, RMSE y correlación |
| D | 05 | Estadísticos, medias móviles, ciclo anual, tendencia y anomalías (z-score) |
| E | 07 | Eventos extremos (opcional): climatología por ventana de días, episodios de temperatura y de turbidez, Isolation Forest |

El resumen de cada bloque para presentar está en [`docs/resumen_codigo.pdf`](docs/resumen_codigo.pdf).

## Cómo ejecutarlo

**En Colab:** botón de arriba y *Entorno de ejecución > Ejecutar todas*. Todo se instala solo.

**En tu computador (Anaconda / Miniconda):**

```bash
conda env create -f environment.yml
conda activate sentinel2-landsat
jupyter lab
```

Las imágenes se leen desde [Microsoft Planetary Computer](https://planetarycomputer.microsoft.com/) (no se descarga ninguna
escena completa, solo la zona de trabajo).

**Los datos del satélite se descargan una sola vez.** Las series quedan en archivos `.parquet` (uno por satélite y por año)
y las dos escenas del bloque A en un `.npz`. Al volver a ejecutar, el notebook lee esos archivos y no descarga nada: los
bloques C, D y E trabajan solo con los parquet. En Colab se guardan en tu Google Drive
(`MyDrive/comparacion_sentinel2_landsat/salidas`, con `USAR_DRIVE = True`), porque la carpeta `/content` se borra al
cerrar la sesión; en tu computador quedan en `salidas/`. Con los parámetros por defecto (Sentinel-2 2024-2025 y Landsat
2013-2025) la primera ejecución toma del orden de una hora, casi todo en la serie de Landsat; las siguientes, segundos.

## Archivos

```
comparacion_sentinel2_landsat.ipynb   el notebook
datos/puntos_biobio_rio.csv           los 9 puntos (id, nombre, lat, lon, tipo + sección, río y orden)
docs/resumen_codigo.pdf               resumen de cada bloque, para presentar
environment.yml                       entorno de conda para ejecutarlo en local
salidas/                              (se crea al ejecutar) parquet, npz, CSV y gráficos
```

## Datos y créditos

* Sentinel-2 L2A: ESA Copernicus, acceso libre. Contiene datos Copernicus Sentinel modificados.
* Landsat 8 y 9 Collection 2 Level-2: USGS, dominio público.
* Funciones de apoyo (`utils/`) y métodos: curso Geomática Aplicada de Álvaro Paredes
  ([alvaroparedesl/geomatica-aplicada](https://github.com/alvaroparedesl/geomatica-aplicada), licencia GPL-3.0). El
  notebook las descarga fijadas en una versión (`COMMIT_CURSO`) para que no cambien.
