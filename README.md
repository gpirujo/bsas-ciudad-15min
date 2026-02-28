# Buenos Aires: Ciudad de 15 Minutos

Análisis de accesibilidad urbana para la Ciudad Autónoma de Buenos Aires, desarrollado por [Aretian](https://aretian.com).

El concepto de **ciudad de 15 minutos** (Carlos Moreno, 2016) propone que cada habitante debería poder resolver sus necesidades esenciales caminando o en bicicleta en no más de 15 minutos. Este análisis evalúa cuatro dimensiones medibles con datos abiertos del GCBA:

| Dimensión | Datos utilizados |
|---|---|
| **Cuidarse** (salud) | Hospitales, CESAC, centros de salud privados |
| **Educarse** | Establecimientos educativos |
| **Abastecerse** | Habilitaciones comerciales de alimentos 2024 |
| **Descansar** | Espacios verdes, polideportivos, espacios culturales, clubes |

El análisis construye un índice compuesto (0–100) para cada uno de los **48 barrios** de CABA y lo cruza con el **ingreso per cápita familiar (IPCF)** para evaluar equidad urbana: ¿las zonas con menor accesibilidad son también las de menor ingreso?

## Resultados

El mapa interactivo con los resultados está disponible en [`mapa_ciudad_15min_v3.html`](mapa_ciudad_15min_v3.html).

## Estructura del repositorio

```
├── buenos_aires_ciudad_15_minutos_v3.ipynb   # Notebook principal
├── mapa_ciudad_15min_v3.html                 # Mapa interactivo (Folium)
└── data/                                     # Datasets (ver detalle abajo)
```

## Fuentes de datos

Todos los datasets provienen de portales de datos abiertos oficiales y no requieren registro.

### Buenos Aires Data — [data.buenosaires.gob.ar](https://data.buenosaires.gob.ar)

| Archivo | Dataset | URL |
|---|---|---|
| `barrios.geojson` | Barrios | https://data.buenosaires.gob.ar/dataset/barrios |
| `hospitales.geojson` | Hospitales | https://data.buenosaires.gob.ar/dataset/hospitales |
| `centros_salud_nivel_1_cesac.csv` | Centros de Salud Nivel 1 (CESAC) | https://data.buenosaires.gob.ar/dataset/centros-de-salud |
| `centros_de_salud_privado.csv` | Centros de Salud Privados | https://data.buenosaires.gob.ar/dataset/centros-de-salud |
| `establecimientos_educativos.csv` | Establecimientos Educativos | https://data.buenosaires.gob.ar/dataset/establecimientos-educativos |
| `manzanas_catastrales.csv` | Manzanas Catastrales | https://data.buenosaires.gob.ar/dataset/manzanas-catastrales |
| `habilitaciones-aprobadas2024.csv` | Habilitaciones Comerciales 2024 | https://data.buenosaires.gob.ar/dataset/habilitaciones-comerciales |
| `espacio_verde_publico.geojson` | Espacios Verdes Públicos | https://data.buenosaires.gob.ar/dataset/espacios-verdes |
| `polideportivos.geojson` | Polideportivos | https://data.buenosaires.gob.ar/dataset/polideportivos |
| `espacios-culturales.xlsx` | Espacios Culturales | https://data.buenosaires.gob.ar/dataset/espacios-culturales |
| `clubes.csv` | Clubes de Barrio y Organizaciones Deportivas | https://data.buenosaires.gob.ar/dataset/clubes |

### IDECBA — [estadisticaciudad.gob.ar](https://www.estadisticaciudad.gob.ar)

| Archivo | Dataset | URL |
|---|---|---|
| `MT_eah_2417.xlsx` | Ingreso Per Cápita Familiar (IPCF) por comuna — EAH 2008/2024 | https://www.estadisticaciudad.gob.ar/eyc/?p=82456 |

## Requisitos

```
python >= 3.10
pandas
geopandas
numpy
matplotlib
folium
scikit-learn
scipy
openpyxl
```

Instalación:
```bash
pip install pandas geopandas numpy matplotlib folium scikit-learn scipy openpyxl
```

## Cómo ejecutar

1. Clonar el repositorio
2. Instalar dependencias
3. Abrir `buenos_aires_ciudad_15_minutos_v3.ipynb` con Jupyter
4. Ejecutar todas las celdas en orden

## Metodología

El índice se construye en cuatro pasos:

1. **Conteo espacial**: para cada barrio, se cuentan los servicios de cada dimensión usando spatial joins (puntos dentro del polígono del barrio).
2. **Densidad**: los conteos se normalizan por área (servicios por km²) para hacer comparables barrios de distinto tamaño.
3. **Normalización**: cada dimensión se escala a 0–100 usando MinMaxScaler.
4. **Índice compuesto**: promedio simple de las cuatro dimensiones normalizadas.

El análisis de equidad cruza el índice con el IPCF relativo por comuna (promedio de CABA = 100) y clasifica los barrios en cuatro cuadrantes según su posición en ambas variables.
