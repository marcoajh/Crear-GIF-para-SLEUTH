# Preparacion de capas de restricción para SLEUTH

#



En este ejercicio se creará una capa de restricciones a partir de las siguentes capas:

*cuadro.shp*  
*Humedal_2010.shp*  
*z_fed*  
*sia_CdMx.shp*  



Veremos tres elemplos para convertir capas a ráster y crear la capa de restriccion a partir de la combinación de esasa capas, aunque las capas vectoriales de origen tenen características distintas, el procedimiento es el mismo.

## Preparación del extent   
Cuando se convierte una capa vectorial a ráster, la capa resultante tiene la mismma extensión que tienen los polígonos, líneas o puntos del vectorial de origen, de modo tal que si se transforman capas vectoriales con diferentes extensiones, las capas ráster resultantes podrían no tener el mismo tamaño, para evitar esto, se usará la capa cuadro.shp para crear capas dummy con el mismo tamaño y resolución para todas las capas, para ello se convierte a ráster. *Ráster > Conversión > Rasterizar (Vectorial a ráster)*

![Figura 1](https://github.com/marcoajh/Crear-GIF-para-SLEUTH/blob/master/fig1.PNG "figura 1")  

Se crea una capa como plantilla para operaciones entre rasters


Se define la resolución de salida en el cuadro de opciones, ésta debe ser 100 x 100 unidades de mapa (metros), En este momento el archivo se guarda como tif.   

![Figura 2](https://github.com/marcoajh/Crear-GIF-para-SLEUTH/blob/master/fig2.PNG "figura 2")  

gdal_rasterize -a ID -tr 100.0 100.0 -l cuadro D:\CVM_SIG\cuadro.shp D:/CVM_SIG/amanzanadas/raster/cuadro.gif



## Caso 1: La capa vectorial consta de un sólo polígono con un sólo valor


Se crea una capa dummy que es la que determina la extension, se usa el nombre de la capa que se quiera crear

![Figura 3](https://github.com/marcoajh/Crear-GIF-para-SLEUTH/blob/master/fig3.PNG "figura 3")  
gdal_rasterize -a ID -tr 100.0 100.0 -l cuadro D:\CVM_SIG\cuadro.shp D:/CVM_SIG/amanzanadas/raster/Humedal_a.tif   
Ya con la capa dummy creada, se repite el proceso para convertir el vectorial deseado usando el ráster dummy y conservando el tamaño y resolución.

![Figura 4](https://github.com/marcoajh/Crear-GIF-para-SLEUTH/blob/master/fig4.PNG "figura 4")  

gdal_rasterize -a ID -l Humedal_2010 D:/CVM_SIG/amanzanadas/Restricciones/UTM/Humedal_2010.shp D:/CVM_SIG/amanzanadas/raster/Humedal.tif   

Es necesario verificar los datos del ráster generado, en este caso, la caracteristica que se quiere representar tiene el valor 1 y el resto del espacio valor 0.




## Caso 2: La capa vectorial contiene varios polígonos con el mismo valor

Se crea una capa dummy que es la que determina la extension, se usa el nombre de la capa que se quiera crear

![Figura 5](https://github.com/marcoajh/Crear-GIF-para-SLEUTH/blob/master/fig5.PNG "figura 5")  
gdal_rasterize -a ID -tr 100.0 100.0 -l cuadro D:/CVM_SIG/cuadro.shp D:/CVM_SIG/amanzanadas/raster/z_fed_a.tif   
Ya con la capa dummy creada, se repite el proceso para convertir el vectorial deseado usando el ráster dummy y conservando el tamaño y resolución.



![Figura 6](https://github.com/marcoajh/Crear-GIF-para-SLEUTH/blob/master/fig6.PNG "figura 6")  
gdal_rasterize -a ID2 -l z_fed D:/CVM_SIG/amanzanadas/z_fed.shp D:/CVM_SIG/amanzanadas/raster/z_fed_a.tif




## Caso 3: la capa vectorial contitne varios polígono con diferentes valores

 sia_CdMx.shp  
Se guarda la capa cuadro.shp como raster dummy
![Figura 7](https://github.com/marcoajh/Crear-GIF-para-SLEUTH/blob/master/fig7.PNG "figura 7")  
gdal_rasterize -a ID -tr 100.0 100.0 -l cuadro D:\CVM_SIG\cuadro.shp D:/CVM_SIG/amanzanadas/raster/siaCdMx_a.tif   


![Figura 8](https://github.com/marcoajh/Crear-GIF-para-SLEUTH/blob/master/fig8.PNG "figura 8")    
gdal_rasterize -a GEO_ID -l sia_CdMx D:\CVM_SIG\amanzanadas\sia_CdMx.shp D:/CVM_SIG/amanzanadas/raster/siaCdMx_a.tif   


Ya tenemos tres capas para generar una capa de restricciones, los valores de los ráster
significan:  

Humedal_a   

Valor | Significado
------ | ------
1 | Humedal
0 | No humedal

z_fed   

Valor | Significado
------ | ------
1 | Zona federal
0 | Otra

sia_CdMx   

Valor | Significado
------ | ------
1 |	Aeródromo Civil
2 |	Área Verde
3 |	Camellón
4 |	Cementerio
5 |	Centro Comercial
6 |	Centro de Asistencia Médica
7 |	Cuerpo de Agua
8 |	Edificación
9 |	Escuela
10 |	Estación del Metro
11 |	Estanque
12 |	Instalación de Bombeo
13 |	Instalación Deportiva o Recreativa
14 |	Instalación Diversa
15 |	Mercado
16 |	Palacio de Gobierno
17 |	Pista de Aviación
18 |	Pista de Carreras
19 |	Plaza
20 |	Presa
21 |	Rasgo Arqueológico
22 |	Subestación Eléctrica
23 |	Tanque de Agua
24 |	Templo



Generaremos una capa hipotética de restricciones, basados en la siguente tabla de reclasificación


Capa | Categoria | Valor
------:|:------:|:------
Humedal_a	|	1	|	100
Humedal_a	|	0	|	0
z_fed_a	|	1	|	100
z_fed_a	|	0	|	0
sia_CdMx_a	|	1	|	100
sia_CdMx_a	|	2	|	70
sia_CdMx_a	|	3	|	95
sia_CdMx_a	|	4	|	100
sia_CdMx_a	|	5	|	100
sia_CdMx_a	|	6	|	100
sia_CdMx_a	|	7	|	100
sia_CdMx_a	|	8	|	100
sia_CdMx_a	|	9	|	100
sia_CdMx_a	|	10	|	100
sia_CdMx_a	|	11	|	100
sia_CdMx_a	|	12	|	100
sia_CdMx_a	|	13	|	70
sia_CdMx_a	|	14	|	100
sia_CdMx_a	|	15	|	100
sia_CdMx_a	|	16	|	100
sia_CdMx_a	|	17	|	100
sia_CdMx_a	|	18	|	100
sia_CdMx_a	|	19	|	100
sia_CdMx_a	|	20	|	100
sia_CdMx_a	|	21	|	100
sia_CdMx_a	|	22	|	100
sia_CdMx_a	|	23	|	100
sia_CdMx_a	|	24	|	100


Para crear la capa usaremos la calculadora de ráster de QGis con la siguiente expresión:  
`(( "Humedal_a@1" = 1 ) * 100) + (("Humedal_a@1" = 0 AND "z_fed_a@1" = 1) * 100) + ("Humedal_a@1" = 0 AND "z_fed_a@1" = 0 AND "sia_CdMx_a@1" = 1 ) * 100 + ("Humedal_a@1" = 0 AND "z_fed_a@1" = 0 AND "sia_CdMx_a@1" = 2 ) * 70 + ("Humedal_a@1" = 0 AND "z_fed_a@1" = 0 AND "sia_CdMx_a@1" = 3 ) * 95 + ("Humedal_a@1" = 0 AND "z_fed_a@1" = 0 AND "sia_CdMx_a@1" = 4 ) * 100 + ("Humedal_a@1" = 0 AND "z_fed_a@1" = 0 AND "sia_CdMx_a@1" = 5 ) * 100 + ("Humedal_a@1" = 0 AND "z_fed_a@1" = 0 AND "sia_CdMx_a@1" = 6 ) * 100 + ("Humedal_a@1" = 0 AND "z_fed_a@1" = 0 AND "sia_CdMx_a@1" = 7 ) * 100 + ("Humedal_a@1" = 0 AND "z_fed_a@1" = 0 AND "sia_CdMx_a@1" = 8 ) * 100 + ("Humedal_a@1" = 0 AND "z_fed_a@1" = 0 AND "sia_CdMx_a@1" = 9 ) * 100 + ("Humedal_a@1" = 0 AND "z_fed_a@1" = 0 AND "sia_CdMx_a@1" = 10 ) * 100 + ("Humedal_a@1" = 0 AND "z_fed_a@1" = 0 AND "sia_CdMx_a@1" = 11 ) * 100 + ("Humedal_a@1" = 0 AND "z_fed_a@1" = 0 AND "sia_CdMx_a@1" = 12 ) * 100 + ("Humedal_a@1" = 0 AND "z_fed_a@1" = 0 AND "sia_CdMx_a@1" = 13 ) * 70 + ("Humedal_a@1" = 0 AND "z_fed_a@1" = 0 AND "sia_CdMx_a@1" = 14 ) * 100 + ("Humedal_a@1" = 0 AND "z_fed_a@1" = 0 AND "sia_CdMx_a@1" = 15 ) * 100 + ("Humedal_a@1" = 0 AND "z_fed_a@1" = 0 AND "sia_CdMx_a@1" = 16 ) * 100 + ("Humedal_a@1" = 0 AND "z_fed_a@1" = 0 AND "sia_CdMx_a@1" = 17 ) * 100 + ("Humedal_a@1" = 0 AND "z_fed_a@1" = 0 AND "sia_CdMx_a@1" = 18 ) * 100 + ("Humedal_a@1" = 0 AND "z_fed_a@1" = 0 AND "sia_CdMx_a@1" = 19 ) * 100 + ("Humedal_a@1" = 0 AND "z_fed_a@1" = 0 AND "sia_CdMx_a@1" = 20 ) * 100 + ("Humedal_a@1" = 0 AND "z_fed_a@1" = 0 AND "sia_CdMx_a@1" = 21 ) * 100 +  ("Humedal_a@1" = 0 AND "z_fed_a@1" = 0 AND "sia_CdMx_a@1" = 22 ) * 100 + ("Humedal_a@1" = 0 AND "z_fed_a@1" = 0 AND "sia_CdMx_a@1" = 23 ) * 100 + ("Humedal_a@1" = 0 AND "z_fed_a@1" = 0 AND "sia_CdMx_a@1" = 24 ) * 100`   

![Figura 9](https://github.com/marcoajh/Crear-GIF-para-SLEUTH/blob/master/fig9.PNG "figura 9")  

En el comando anterior, la parte de la igualdad `( "Humedal_a@1" = 1 )`, asigna el valor 1 cuando es evaluada como verdadera, por eso al hacer la multiplicación por 100 obtenemos el valor deseado, 100 en este caso. Para asignar valores en la parte complementaria, ésta se agrega en la igualdad `("Humedal_a@1" = 0 AND "z_fed_a@1" = 1)` nuevamente cuando existen las dos condiones, la igualdad se evalúa como verdadera y se asigna el valor 1, que posteriormente es multiplicado por el valor deseado para esos pixeles. Finalmente se agrega la condicional para la última capa, `("Humedal_a@1" = 0 AND "z_fed_a@1" = 0 AND "sia_CdMx_a@1" = 1 )` que cuando es verdadera, se multiplica por el valor desado según la categoría.  


## Exportar la capa de restricciones a GIF para usar en SLEUTH   
Una vez que la capa de restricciones está lista y que se ha verificado que los datos de la capa sean los deseados, hay que exportarla a formato gif de 8 bits (valores de 0 - 255) para ser usada en SLEUTH, para ésto usamos la utilidad de Traducir del menú Ráster: *Ráster > Conversión > Traducir (convertir formato)*

En la ventana de *Traducir*, se establecen los nombres de la capa de entrada y la de salida y se modifica el comando, (click en el ícono con forma de lápiz) para agregar la opción `-ot Byte`

 ![Figura 10](https://github.com/marcoajh/Crear-GIF-para-SLEUTH/blob/master/fig10.PNG "figura 10")    

 Ya está la capa de restricciones.
