# Python in Power BI

1. Instalar Miniconda3 en vuestro sistema operativo Windows
Pueden instalar directamente desde el enlace oficial versión 64bits 
[Miniconda Python3.8 Windows64bits](https://docs.conda.io/en/latest/miniconda.html)


2. Crear entorno virtual
Para que funcione correctamente Python en Power BI, creamos un entorno virtual para poder instalar nuestras dependencias (librerías y módulos de programación) en la versión Python 3.x

- buscamos y entramos en **miniconda3/anaconda Prompt** o **miniconda3/PowerShell** para ejecutar secuencias de comandos
- una vez dentro nos encontraremos en 

`(base)PS<C:\Users\<nombre-usuario>>`

desde aquí ejecutaremos:

```{shell}
conda create -name <nombre de nuestro entorno> python -y
``` 

Se realizará una operación de instalación del entorno que tardará unos minutos, sucesivamente activaremos este entorno virtual

```{shell}
conda activate <nombre entorno>
```

y nos encontraremos ahora dentro de esta ruta
`(nombre entorno) PS C:\Users\<nombre usuario>>`

y desde aquí procedemos con el siguiente paso

3. Comprobación de Python
ejecutamos python y nos encontraremos con el entorno de Python3.x

```{shell}
python
```
Y este es el ejemplo:
`
Python 3.8.5 (default, Sep  3 2020, 21:29:08) [MSC v.1916 64 bit (AMD64)] :: Anaconda, Inc. on win32
Type "help", "copyright", "credits" or "license" for more information.
>>>
`
para salir

```{python}
exit()
```

4. Instalamos los paquetes necesarios
Para instalar los paquetes / librerías necesarias para tratar datos en Python / Power BI podemos hacerlo de forma manual:

```{shell}
pip install <nombre paquete>
```

Para instalar los paquetes necesarios podemos crear un fichero *requirements.txt* e incluimos las dependencias que necesitaremos:

```{shell}
notes requirements_pbi.txt
```

Se abrirá el *bloc de notas* y copiar y pegar los que viene a continuación:
```{text}
pandas
numpy
matplotlib
seaborn
scikit-learn
```

guardamos y seguimos

```{shell}
pip install -r requirements_pbi.txt
```

ejecutamos y observaremos las instalaciones de estas librerías

5. Prueba en Power BI

Una vez instaladas las dependencias podemos entramos en **Power BI** y realizar el cambio en las configuraciones:

```
desde File > opciones > Python scripting > 
seleccionar vuestra ruta del entorno virtual creado anteriormente al paso 2 como este ejemplo

*C:\Users\<nombre-usuario>\miniconda3\envs\<nombre-virtual-env>*
```

En caso de disponer **Anaconda** será el equivalente sustituyendo miniconda con el Anaconda.

## Get Data in Python environment

6. Prueba con librería **datasets**

Desde `Get Data` probamos a introducir un pequeño comando de Python y ver si creamos un dataframe desde cero

```{python}
import pandas as pd 
df = pd.DataFrame({ 
    'Fname':['Harry','Sally','Paul','Abe','June','Mike','Tom'], 
    'Age':[21,34,42,18,24,80,22], 
    'Weight': [180, 130, 200, 140, 176, 142, 210], 
    'Gender':['M','F','M','M','F','M','M'], 
    'State':['Washington','Oregon','California','Washington','Nevada','Texas','Nevada'],
    'Children':[4,1,2,3,0,2,0],
    'Pets':[3,2,2,5,0,1,5] 
})
```

Se cargará la tabla y si validamos ya está realizada la primera operación de Python en Power BI.

7. Creamos un gráfico
Seleccionando el elemento de Python procedemos a crear un `scatterplot`

```{python}
import matplotlib.pyplot as plt 
dataset.plot(kind='scatter', x='Age', y='Weight', color='red')
plt.show()
```
y mostraría un gráfico de puntos de dispersión

o probamos con `líneas`

```{python}
import matplotlib.pyplot as plt 
ax = plt.gca() 
dataset.plot(kind='line',x='Fname',y='Children',ax=ax) 
dataset.plot(kind='line',x='Fname',y='Pets', color='red', ax=ax) 
plt.show()
```

o probamos con un gráfico de barras:

```{python}
import matplotlib.pyplot as plt 
dataset.plot(kind='bar',x='Fname',y='Age') 
plt.show()
```

