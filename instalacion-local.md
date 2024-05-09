# Instalación local

## Requisitos:
  Tener instalado git y mamba.
  
## Descargar el código del repositorio oficial
Dentro de la carpeta donde trabajaremos descargamos la versión mas reciente del código con el siguiente comando.

```git clone https://github.com/torchmd/torchmd-net```

Nos dirigimos a la carpeta donde se descargó el código.

```cd torchmd-net```

Para evitar problemas de compatibilidad con versiones futuras de código, cambiamos a la versión sobre la cúal esta desarrollada esta carpeta.

```git checkout dca66796d00680a79a7b7c85d6704d30d15dc84c```

## Instalar el ambiente virtal con mamba
Creamos el ambiente virtual junto con los módulos necesarios con el comando.

```mamba env create -f environment.yml```

Activamos el ambiente.

```mamba activate torchmd-net```

## Instalar jupyter notebook
 Para poder trabajar con la arquitectura dentro de notebooks instalamos jupyter notebook, ipykernel y registramos el ambiente con ipykernel.

 Instalamos los módulos dentro del ambiente.

 ```pip install notebook ipykernel ```
 
Registramos el ambiente en ipykernel.

 ```python -m ipykernel install --user --name=torchmd-net ```


## Instalar el código en el ambiente virtual mamba

Dentro del ambiente mamba ejecutamos.

```pip install -e .```
