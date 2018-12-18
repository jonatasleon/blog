---
layout: post
title: Manipulação de Dados Multidimensionais com Python
date: '2018-11-13 16:14'
categories: python meteorologia netcdf
---
Conteúdo rápido para consultas posteriores sobre manipulação de dados multidimensionais com Python.

Dependências:

 - Python 3.x;
 - Jupyter Notebooks;
 - xarray;
 - matplotlib;

O trecho de código abaixo realiza o download dos dados de Ventos do Mar Misturados ([Blended Sea Winds][98702aed]) para o ano de 2017. Para alterar o período de tempo, altere a string de _matching_ `'uv2017????rt.nc'`. 

```python
import os
import sys
import ftplib
import fnmatch

ftp = ftplib.FTP('eclipse.ncdc.noaa.gov') # Define servidor FTP a ser acessado
ftp.login() # Realiza autenticação perante o servidor
ftp.cwd('/pub/seawinds/SI/uv/daily/netcdf/2000s') # Define diretório atual

filenames = ftp.nlst() # Lista diretórios e arquivos no diretório atual
filenames = fnmatch.filter(filenames, 'uv2017????rt.nc') # Filtra arquivos desejados
filenames = sorted(filenames) # Coloca os arquivos em ordem alfabética

for filename in filenames:
  f = open(filename, 'wb').write
  ftp.retrbinary('RETR {}'.format(filename), f)
```

  [98702aed]: https://www.ncdc.noaa.gov/data-access/marineocean-data/blended-global/blended-sea-winds "Blended Sea Winds"
