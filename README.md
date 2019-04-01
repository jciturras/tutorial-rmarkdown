
## Cómo usar Rmarkdown: versión versión html
### *Julio César Iturra Sanhueza - jciturra@uc.cl*

------
# YAML header

El YAML header corresponde al lugar donde ustedes pueden determinar las opciones de su documento. Generalmente comenzamos poniendo el nombre, autoría y fecha de su documento. Agregué otras opciones que son útiles para dar formato a su documento, tales como el tamaño de la fuentes (fontsize), el interlineado (linestretch), márgenes (geometry) y abstract.

```r
---
title: "Cómo usar Rmarkdown"
author: "Julio Iturra - jciturra@uc.cl"
date: '`r format(Sys.time(), "%d %B, %Y")`'  
fontsize: 11pt
linestretch: "1.0"
geometry: margin=0.78in
output:
  html_document: default
header-includes:
   - \usepackage[spanish,es-tabla,es-nodecimaldot]{babel} #Para "Tabla" y "Figura" en español. Punto para decimal.
   - \usepackage[utf8]{inputenc}
   - \usepackage{booktabs}
abstract: "El presente documento tiene por objetivo facilitarles la vida  ..."
---
```

La opción `output` tiene tres opciones para convertir su documento en `.pdf`, `.html` y `.doc`. A mi me gusta realizarlo en pdf para los informes y html para mis reportes de datos y exploración, dado que los trabajos en html pueden ser abiertos en cualquier sistema operativo sin la necesidad de un software especializado como Adove o Nitro reader (por dar algunos ejemplos).

# Introducción

Este documento está enfocado al reporte en formato .pdf, dado que me parece más adecuado para el reporte de informes y trabajos académicos. A modo general, la figura a continuación muestra cómo funciona Rmarkdown cuando estamos empleando formato .pdf.

<img src="images/rmarkdownflow.png" width="70%" style="display: block; margin: auto;" /><img src="images/markdown2.png" width="70%" style="display: block; margin: auto;" />

El primer cuadro es su documento en Rmarkdown, donde escriben su código en R y lo combinan con texto. [Knitr](https://yihui.name/knitr/) es el paquete que convierte todo lo que ustedes escriben a formato Markdown (.md), lo cua posteriormente es transformado por [Pandoc](https://pandoc.org/) en cualquiera de los formatos que ustedes necesiten (.pdf, .html o .doc). El resultado de este proceso es su documento final.

## Software necesario para compilar en .pdf

Cuando estamos empleando documentos en .pdf, Pandoc requiere de que ustedes tengan instalado en su computador alguno de los paquetes base para escribir documentos en \LaTeX. Para esto deben instalar alguno de los paquetes disponibles, en mi caso uso [Miktex](https://miktex.org/), el cual tiene soporte para Windows, Mac y Linux. Hasta ahora no me ha dado problemas y lo recomiendo.

1. Ir a la web de [Miktex](https://miktex.org/)
2. Descargar el instalador y ejecutarlo
3. Esperar que termine la descarga de los paquetes, lo cual puede demorar varios minutos.
4. Cuando esté todo listo, reinicien su computador.

Finalizado este paso, pueden comenza a trabajar con Rmarkdown sin problemas.

# Elaborar títulos y enumeración

`#Título grande`

`##Título mediano`

`##Título pequeño`


`@.`  Para crear numeración con interrupciones (autonumerado)

`1.` Para crear numeración según sus necesidades

`a.` Para crear sub-numeración con letras, también se puede realizar con números.

`*` Una viñeta cuadrada

## Ecuaciones y símbolos matemáticos

`$e=mc^2$`  $e=mc^2$

`$\alpha \chi^2 \beta$` $\alpha \ \chi \ \beta$



Hay una hoja de consejos para usar Rmarkdwon que pueden encontrar [aquí](https://rmarkdown.rstudio.com/lesson-1.html). Aparecen muchos más detalles de lo que se señala en este documento.

1. **Usar chunks**

    * el ctrl + alt + i



<img src="images/chunk1.png" width="95%" style="display: block; margin: auto;" />


* `eval=TRUE` Sirve para determinar si queremos que se vean nuestros resultados.
* `include=TRUE` Sirve para determinar si queremos que se incluya nuestro código.
* `message=FALSE` Sirve para determinar si queremos los mensajes emergenter.
* `warning=FALSE` Sirve para determinar si queremos las advertencias de R.
* `results='asis'` Permite que el código creado por las funciones de R sea empleado en la compilación.
* ¡run! (flecha verde o ctrl+shift+enter)


Ejemplo:


```r
#{r, results='hold'}
revolucion <- "Marx"
revolucion

velocirraptor <- "rawr"
velocirraptor
```

```
## [1] "Marx"
## [1] "rawr"
```

2. **Paquetes especializados para reporte de tablas**

  * `knitr`
  * `kableExtra`
  * `xtable`
  * `texreg`
  * `stargazer`

Instalemos los paquetes:


```r
install.packages("knitr")
install.packages("kableExtra")
install.packages("xtable")
install.packages("texreg")
install.packages("stargazer")
```

Carguemos los paquetes:


```r
library(knitr)
library(kableExtra)
library(xtable)
library(texreg)
library(stargazer)
library(dplyr) #principalmente para usar el operador %>%
```


#Ejemplo con regresión usando texreg

El paquete `texreg` es muy útil cuando se trata de reportar modelos de regresión, tiene bastantes funcionalidades, pero las principales son el hecho de que permite reportar modelos anidados en una sola tabla.




```r
#Un logit fome...
pl <- lm(voto ~ sexo + edad + educon + ecivil + ppol+ socconf+confl, data=coes, link="logit")
```


```r
texreg::htmlreg(pl, #Si son más modelos ponemos list(m1,m2,m3)
               digits = 3, #dígitos de la tabla
               float.pos="h!", #permite dejar la tabla fija en su lugar
               scalebox = 0.70, #indica que la tabla tiene una proporción del 75% c
               caption = "Modelo Logit", #título del modelo
               custom.coef.names=c("(Intercepto)", #Podemos asignar nombres a los coeficientes
                              "Mujer","Edad","Educación",
                              "Soltero/a","Viudo/a","Sep/Div/Anu",
                              "Centro","Derecha",
                              "No sabe/No responde",
                              "Confianza Social",
                              "Per.Conflicto"),
               custom.model.names = "Modelo 1") #si son más modelos = c("Modelo1","Modelo2")
```



<table cellspacing="0" align="center" style="border: none;">
<caption align="bottom" style="margin-top:0.3em;">Modelo Logit</caption>
<tr>
<th style="text-align: left; border-top: 2px solid black; border-bottom: 1px solid black; padding-right: 12px;"><b></b></th>
<th style="text-align: left; border-top: 2px solid black; border-bottom: 1px solid black; padding-right: 12px;"><b>Modelo 1</b></th>
</tr>
<tr>
<td style="padding-right: 12px; border: none;">(Intercepto)</td>
<td style="padding-right: 12px; border: none;">0.063</td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;"></td>
<td style="padding-right: 12px; border: none;">(0.077)</td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;">Mujer</td>
<td style="padding-right: 12px; border: none;">-0.004</td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;"></td>
<td style="padding-right: 12px; border: none;">(0.023)</td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;">Edad</td>
<td style="padding-right: 12px; border: none;">0.009<sup style="vertical-align: 0px;">***</sup></td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;"></td>
<td style="padding-right: 12px; border: none;">(0.001)</td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;">Educación</td>
<td style="padding-right: 12px; border: none;">0.037<sup style="vertical-align: 0px;">***</sup></td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;"></td>
<td style="padding-right: 12px; border: none;">(0.006)</td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;">Soltero/a</td>
<td style="padding-right: 12px; border: none;">-0.090<sup style="vertical-align: 0px;">**</sup></td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;"></td>
<td style="padding-right: 12px; border: none;">(0.029)</td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;">Viudo/a</td>
<td style="padding-right: 12px; border: none;">-0.004</td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;"></td>
<td style="padding-right: 12px; border: none;">(0.047)</td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;">Sep/Div/Anu</td>
<td style="padding-right: 12px; border: none;">-0.055</td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;"></td>
<td style="padding-right: 12px; border: none;">(0.038)</td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;">Centro</td>
<td style="padding-right: 12px; border: none;">-0.041</td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;"></td>
<td style="padding-right: 12px; border: none;">(0.033)</td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;">Derecha</td>
<td style="padding-right: 12px; border: none;">0.100<sup style="vertical-align: 0px;">**</sup></td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;"></td>
<td style="padding-right: 12px; border: none;">(0.036)</td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;">No sabe/No responde</td>
<td style="padding-right: 12px; border: none;">-0.069<sup style="vertical-align: 0px;">*</sup></td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;"></td>
<td style="padding-right: 12px; border: none;">(0.033)</td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;">Confianza Social</td>
<td style="padding-right: 12px; border: none;">0.002</td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;"></td>
<td style="padding-right: 12px; border: none;">(0.011)</td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;">Per.Conflicto</td>
<td style="padding-right: 12px; border: none;">0.033<sup style="vertical-align: 0px;">*</sup></td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;"></td>
<td style="padding-right: 12px; border: none;">(0.015)</td>
</tr>
<tr>
<td style="border-top: 1px solid black;">R<sup style="vertical-align: 0px;">2</sup></td>
<td style="border-top: 1px solid black;">0.130</td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;">Adj. R<sup style="vertical-align: 0px;">2</sup></td>
<td style="padding-right: 12px; border: none;">0.124</td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;">Num. obs.</td>
<td style="padding-right: 12px; border: none;">1538</td>
</tr>
<tr>
<td style="border-bottom: 2px solid black;">RMSE</td>
<td style="border-bottom: 2px solid black;">0.432</td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;" colspan="3"><span style="font-size:0.8em"><sup style="vertical-align: 0px;">***</sup>p &lt; 0.001, <sup style="vertical-align: 0px;">**</sup>p &lt; 0.01, <sup style="vertical-align: 0px;">*</sup>p &lt; 0.05</span></td>
</tr>
</table>

<br>

## Con modelos anidados


```r
#```{r, eval=TRUE, include=TRUE, message=FALSE, warning=FALSE, results='asis'}
m1=glm(voto~sexo+edad+educon+ecivil+ppol+socconf,
       data=coes, family=binomial) #Educación continua
m2=glm(voto~sexo+edad+educ  +ecivil+ppol+socconf,
       data=coes, family="binomial") #Educación categórica
m3=glm(voto~sexo+edad+educon+ecivil+ppol+socconf+ppol*socconf,
       data=coes, family="binomial") #Interacción Posición Política*Confianza Social
texreg::htmlreg(list(m1,m2,m3), digits = 3,float.pos="h!",scalebox=0.50,
               caption = "Modelos Logit",
               custom.model.names = c("Modelo 1", "Modelo 2", "Modelo 3"),
               custom.coef.names = c("(Intercepto)","Mujer","Edad",
                                     "Educación","Soltero/a","Viudo/a",
                                     "Sep/Div/Anu","Centro","Derecha",
                                     "No sabe/No responde","Confianza Social",
                                     "Primaria","Secundaria","Técnica superior",
                                     "Universitaria","Centro:Conf.Social",
                                     "Derecha:Conf.Social","NS/NR:Conf.Social"))
```

<br>

<table cellspacing="0" align="center" style="border: none;">
<caption align="bottom" style="margin-top:0.3em;">Modelos Logit</caption>
<tr>
<th style="text-align: left; border-top: 2px solid black; border-bottom: 1px solid black; padding-right: 12px;"><b></b></th>
<th style="text-align: left; border-top: 2px solid black; border-bottom: 1px solid black; padding-right: 12px;"><b>Modelo 1</b></th>
<th style="text-align: left; border-top: 2px solid black; border-bottom: 1px solid black; padding-right: 12px;"><b>Modelo 2</b></th>
<th style="text-align: left; border-top: 2px solid black; border-bottom: 1px solid black; padding-right: 12px;"><b>Modelo 3</b></th>
</tr>
<tr>
<td style="padding-right: 12px; border: none;">(Intercepto)</td>
<td style="padding-right: 12px; border: none;">-2.158<sup style="vertical-align: 0px;">***</sup></td>
<td style="padding-right: 12px; border: none;">-1.458<sup style="vertical-align: 0px;">***</sup></td>
<td style="padding-right: 12px; border: none;">-1.957<sup style="vertical-align: 0px;">***</sup></td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;"></td>
<td style="padding-right: 12px; border: none;">(0.383)</td>
<td style="padding-right: 12px; border: none;">(0.390)</td>
<td style="padding-right: 12px; border: none;">(0.395)</td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;">Mujer</td>
<td style="padding-right: 12px; border: none;">-0.045</td>
<td style="padding-right: 12px; border: none;">-0.054</td>
<td style="padding-right: 12px; border: none;">-0.042</td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;"></td>
<td style="padding-right: 12px; border: none;">(0.124)</td>
<td style="padding-right: 12px; border: none;">(0.124)</td>
<td style="padding-right: 12px; border: none;">(0.125)</td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;">Edad</td>
<td style="padding-right: 12px; border: none;">0.047<sup style="vertical-align: 0px;">***</sup></td>
<td style="padding-right: 12px; border: none;">0.044<sup style="vertical-align: 0px;">***</sup></td>
<td style="padding-right: 12px; border: none;">0.047<sup style="vertical-align: 0px;">***</sup></td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;"></td>
<td style="padding-right: 12px; border: none;">(0.005)</td>
<td style="padding-right: 12px; border: none;">(0.005)</td>
<td style="padding-right: 12px; border: none;">(0.005)</td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;">Educación</td>
<td style="padding-right: 12px; border: none;">0.211<sup style="vertical-align: 0px;">***</sup></td>
<td style="padding-right: 12px; border: none;"></td>
<td style="padding-right: 12px; border: none;">0.218<sup style="vertical-align: 0px;">***</sup></td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;"></td>
<td style="padding-right: 12px; border: none;">(0.034)</td>
<td style="padding-right: 12px; border: none;"></td>
<td style="padding-right: 12px; border: none;">(0.034)</td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;">Soltero/a</td>
<td style="padding-right: 12px; border: none;">-0.410<sup style="vertical-align: 0px;">**</sup></td>
<td style="padding-right: 12px; border: none;">-0.476<sup style="vertical-align: 0px;">**</sup></td>
<td style="padding-right: 12px; border: none;">-0.393<sup style="vertical-align: 0px;">**</sup></td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;"></td>
<td style="padding-right: 12px; border: none;">(0.147)</td>
<td style="padding-right: 12px; border: none;">(0.148)</td>
<td style="padding-right: 12px; border: none;">(0.148)</td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;">Viudo/a</td>
<td style="padding-right: 12px; border: none;">0.057</td>
<td style="padding-right: 12px; border: none;">0.003</td>
<td style="padding-right: 12px; border: none;">0.039</td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;"></td>
<td style="padding-right: 12px; border: none;">(0.290)</td>
<td style="padding-right: 12px; border: none;">(0.290)</td>
<td style="padding-right: 12px; border: none;">(0.291)</td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;">Sep/Div/Anu</td>
<td style="padding-right: 12px; border: none;">-0.336</td>
<td style="padding-right: 12px; border: none;">-0.323</td>
<td style="padding-right: 12px; border: none;">-0.347</td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;"></td>
<td style="padding-right: 12px; border: none;">(0.205)</td>
<td style="padding-right: 12px; border: none;">(0.205)</td>
<td style="padding-right: 12px; border: none;">(0.206)</td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;">Centro</td>
<td style="padding-right: 12px; border: none;">-0.191</td>
<td style="padding-right: 12px; border: none;">-0.194</td>
<td style="padding-right: 12px; border: none;">-0.507<sup style="vertical-align: 0px;">*</sup></td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;"></td>
<td style="padding-right: 12px; border: none;">(0.175)</td>
<td style="padding-right: 12px; border: none;">(0.174)</td>
<td style="padding-right: 12px; border: none;">(0.234)</td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;">Derecha</td>
<td style="padding-right: 12px; border: none;">0.633<sup style="vertical-align: 0px;">**</sup></td>
<td style="padding-right: 12px; border: none;">0.653<sup style="vertical-align: 0px;">**</sup></td>
<td style="padding-right: 12px; border: none;">0.105</td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;"></td>
<td style="padding-right: 12px; border: none;">(0.214)</td>
<td style="padding-right: 12px; border: none;">(0.213)</td>
<td style="padding-right: 12px; border: none;">(0.278)</td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;">No sabe/No responde</td>
<td style="padding-right: 12px; border: none;">-0.338</td>
<td style="padding-right: 12px; border: none;">-0.358<sup style="vertical-align: 0px;">*</sup></td>
<td style="padding-right: 12px; border: none;">-0.598<sup style="vertical-align: 0px;">**</sup></td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;"></td>
<td style="padding-right: 12px; border: none;">(0.174)</td>
<td style="padding-right: 12px; border: none;">(0.174)</td>
<td style="padding-right: 12px; border: none;">(0.232)</td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;">Confianza Social</td>
<td style="padding-right: 12px; border: none;">0.021</td>
<td style="padding-right: 12px; border: none;">0.027</td>
<td style="padding-right: 12px; border: none;">-0.287<sup style="vertical-align: 0px;">*</sup></td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;"></td>
<td style="padding-right: 12px; border: none;">(0.062)</td>
<td style="padding-right: 12px; border: none;">(0.062)</td>
<td style="padding-right: 12px; border: none;">(0.139)</td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;">Primaria</td>
<td style="padding-right: 12px; border: none;"></td>
<td style="padding-right: 12px; border: none;">0.309</td>
<td style="padding-right: 12px; border: none;"></td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;"></td>
<td style="padding-right: 12px; border: none;"></td>
<td style="padding-right: 12px; border: none;">(0.273)</td>
<td style="padding-right: 12px; border: none;"></td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;">Secundaria</td>
<td style="padding-right: 12px; border: none;"></td>
<td style="padding-right: 12px; border: none;">0.385</td>
<td style="padding-right: 12px; border: none;"></td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;"></td>
<td style="padding-right: 12px; border: none;"></td>
<td style="padding-right: 12px; border: none;">(0.219)</td>
<td style="padding-right: 12px; border: none;"></td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;">Técnica superior</td>
<td style="padding-right: 12px; border: none;"></td>
<td style="padding-right: 12px; border: none;">0.602<sup style="vertical-align: 0px;">*</sup></td>
<td style="padding-right: 12px; border: none;"></td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;"></td>
<td style="padding-right: 12px; border: none;"></td>
<td style="padding-right: 12px; border: none;">(0.257)</td>
<td style="padding-right: 12px; border: none;"></td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;">Universitaria</td>
<td style="padding-right: 12px; border: none;"></td>
<td style="padding-right: 12px; border: none;">1.319<sup style="vertical-align: 0px;">***</sup></td>
<td style="padding-right: 12px; border: none;"></td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;"></td>
<td style="padding-right: 12px; border: none;"></td>
<td style="padding-right: 12px; border: none;">(0.265)</td>
<td style="padding-right: 12px; border: none;"></td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;">Centro:Conf.Social</td>
<td style="padding-right: 12px; border: none;"></td>
<td style="padding-right: 12px; border: none;"></td>
<td style="padding-right: 12px; border: none;">0.360<sup style="vertical-align: 0px;">*</sup></td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;"></td>
<td style="padding-right: 12px; border: none;"></td>
<td style="padding-right: 12px; border: none;"></td>
<td style="padding-right: 12px; border: none;">(0.174)</td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;">Derecha:Conf.Social</td>
<td style="padding-right: 12px; border: none;"></td>
<td style="padding-right: 12px; border: none;"></td>
<td style="padding-right: 12px; border: none;">0.643<sup style="vertical-align: 0px;">**</sup></td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;"></td>
<td style="padding-right: 12px; border: none;"></td>
<td style="padding-right: 12px; border: none;"></td>
<td style="padding-right: 12px; border: none;">(0.226)</td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;">NS/NR:Conf.Social</td>
<td style="padding-right: 12px; border: none;"></td>
<td style="padding-right: 12px; border: none;"></td>
<td style="padding-right: 12px; border: none;">0.290</td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;"></td>
<td style="padding-right: 12px; border: none;"></td>
<td style="padding-right: 12px; border: none;"></td>
<td style="padding-right: 12px; border: none;">(0.179)</td>
</tr>
<tr>
<td style="border-top: 1px solid black;">AIC</td>
<td style="border-top: 1px solid black;">1712.793</td>
<td style="border-top: 1px solid black;">1723.704</td>
<td style="border-top: 1px solid black;">1709.801</td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;">BIC</td>
<td style="padding-right: 12px; border: none;">1771.514</td>
<td style="padding-right: 12px; border: none;">1798.439</td>
<td style="padding-right: 12px; border: none;">1784.536</td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;">Log Likelihood</td>
<td style="padding-right: 12px; border: none;">-845.396</td>
<td style="padding-right: 12px; border: none;">-847.852</td>
<td style="padding-right: 12px; border: none;">-840.900</td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;">Deviance</td>
<td style="padding-right: 12px; border: none;">1690.793</td>
<td style="padding-right: 12px; border: none;">1695.704</td>
<td style="padding-right: 12px; border: none;">1681.801</td>
</tr>
<tr>
<td style="border-bottom: 2px solid black;">Num. obs.</td>
<td style="border-bottom: 2px solid black;">1538</td>
<td style="border-bottom: 2px solid black;">1538</td>
<td style="border-bottom: 2px solid black;">1538</td>
</tr>
<tr>
<td style="padding-right: 12px; border: none;" colspan="5"><span style="font-size:0.8em"><sup style="vertical-align: 0px;">***</sup>p &lt; 0.001, <sup style="vertical-align: 0px;">**</sup>p &lt; 0.01, <sup style="vertical-align: 0px;">*</sup>p &lt; 0.05</span></td>
</tr>
</table>

## Kable y KableExtra


```r
#```{r tabla, echo=TRUE, message=FALSE, results='asis'}
age = c(1,2,3,4,5)
ageF = factor(age, labels=c("35-44","45-54","55-64","65-74","75-84"))
deaths1 = c(32,104,206,186,102)
deaths2 = c(2,12,28,28,31)

#Person-Years
py1 = c(52407,43248,28612,12663,5317)
py2 = c(18790,10673,5710,2585,1462)

tabla <- as.data.frame(cbind(ageF,deaths1,py1,deaths2,py2)) #creamos el data.frame
tabla$ageF = factor(age, labels=c("35 - 44","45 - 54","55 -  64","65 - 74","75 - 84"))

#----nuestra tabla------
kable(tabla,format = "html", booktabs =TRUE, escape = FALSE, align = c("lcccc"),
      caption = "Muertes por enfermedad cardiaca después de 10 años",
      col.names = linebreak(c("Grupo de\n edad", "Muertes\n ", "Persona-años\n ",
                              "Muertes\n", "Persona-años\n"), align = "c")) %>%
  kable_styling(latex_options =c("hold_position")) %>%
  add_header_above(c(" " = 1, "Fumadores" = 2, "No fuma" = 2))
```

<table class="table" style="margin-left: auto; margin-right: auto;">
<caption>Muertes por enfermedad cardiaca después de 10 años</caption>
 <thead>
<tr>
<th style="border-bottom:hidden" colspan="1"></th>
<th style="border-bottom:hidden; padding-bottom:0; padding-left:3px;padding-right:3px;text-align: center; " colspan="2"><div style="border-bottom: 1px solid #ddd; padding-bottom: 5px;">Fumadores</div></th>
<th style="border-bottom:hidden; padding-bottom:0; padding-left:3px;padding-right:3px;text-align: center; " colspan="2"><div style="border-bottom: 1px solid #ddd; padding-bottom: 5px;">No fuma</div></th>
</tr>
  <tr>
   <th style="text-align:left;"> Grupo <br>  de   edad </th>
   <th style="text-align:center;"> Muertes  </th>
   <th style="text-align:center;"> Persona-años  </th>
   <th style="text-align:center;"> Muertes </th>
   <th style="text-align:center;"> Persona-años </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> 35 - 44 </td>
   <td style="text-align:center;"> 32 </td>
   <td style="text-align:center;"> 52407 </td>
   <td style="text-align:center;"> 2 </td>
   <td style="text-align:center;"> 18790 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 45 - 54 </td>
   <td style="text-align:center;"> 104 </td>
   <td style="text-align:center;"> 43248 </td>
   <td style="text-align:center;"> 12 </td>
   <td style="text-align:center;"> 10673 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 55 -  64 </td>
   <td style="text-align:center;"> 206 </td>
   <td style="text-align:center;"> 28612 </td>
   <td style="text-align:center;"> 28 </td>
   <td style="text-align:center;"> 5710 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 65 - 74 </td>
   <td style="text-align:center;"> 186 </td>
   <td style="text-align:center;"> 12663 </td>
   <td style="text-align:center;"> 28 </td>
   <td style="text-align:center;"> 2585 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 75 - 84 </td>
   <td style="text-align:center;"> 102 </td>
   <td style="text-align:center;"> 5317 </td>
   <td style="text-align:center;"> 31 </td>
   <td style="text-align:center;"> 1462 </td>
  </tr>
</tbody>
</table>


# Algunos tutoriales que me gustan:

* [Escribir una Tesis en Rmarkdown](https://rosannavanhespenresearch.wordpress.com/2016/03/30/writing-your-thesis-with-r-markdown-5-the-thesis-layout/)(Van Espen, 2017)
* [Rmarkdown ultimate Guide](https://bookdown.org/yihui/rmarkdown/) (Xie et al. 2018)
* [KableExtra y Latex](https://haozhu233.github.io/kableExtra/awesome_table_in_pdf.pdf) (Zhu, 2018)
