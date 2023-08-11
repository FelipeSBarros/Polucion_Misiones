# Polución en Misiones, Argentina

1. [Ambiente de trabajo](#creando-ambiente-de-trabajo)
1. [Configuración acceso](#configuración-de-acceso)
   1. Creación de usuário en [Atmospheric Data Store (ADS)](https://ads.atmosphere.copernicus.eu/#!/home). 
   1. [llave de acceso](https://api.ecmwf.int/v1/key/).
   1. Instalación [cdsapi](#instalación-cdsapi)
1. [usando `cdsapi`](#usando-cdsapi)
1. [Organización de los datos descargados](#organización-de-los-datos-descargados)

[Acceso datos publicos](https://confluence.ecmwf.int/display/WEBAPI/Access+ECMWF+Public+Datasets)

## Creando ambiente de trabajo

```commandline
#mkdir Polucion_Misiones
#cd Polucion_Misiones
#git init
#touch README.md
git clone git@github.com:FelipeSBarros/Polucion_Misiones.git
cd Polucion_Misiones
poetry install
```

## Configuración de acceso

1. Crear un archivo `.cdsapirc` y añadir los [datos de acceso](https://ads.atmosphere.copernicus.eu/api-how-to) a ele:
2. [Más informaciones](https://confluence.ecmwf.int/display/WEBAPI/Access+ECMWF+Public+Datasets)

```commandline
touch ~/.cdsapirc
# colocar infos de chave no arquivo
```

### Instalación cdsapi

```commandline
pip install --upgrade pip
pip install cdsapi 
pip install python-dateutil
```

### Usando cdsapi

Exemplo de requisição usando o `cdsapi`:

```python
import cdsapi

c = cdsapi.Client()

c.retrieve(
    'cams-global-atmospheric-composition-forecasts',
    {
        'type': 'forecast',
        'format': 'netcdf_zip',
        'variable': 'particulate_matter_2.5um',
        'date': '2023-05-01/2023-05-02',
        'time': [
            '00:00', '12:00',
        ],
        'leadtime_hour': [
            '0', '12', '6',
        ],
    },
    'download.netcdf_zip')
```

[**Script python usado**](./Python/Download_pm25_monthly.py)

## Organización de los dados descargados

* unzip;
[Script de organização dos dados](./Python/organinzing_cams_data.py)

## Conversão em objeto R

* Consolidación de los datos `netCDF` en `rds` mensual;
* Consolidación de los datos `netCDF` en `rds` mensual con valor medio diário de pm<2.5;
* Extracción de los calores de pm>2.5 medios diários por município;
 
[Processo desenvolvido em R](./R/1_organize_extract_pm25.R)
