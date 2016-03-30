# 30-de-marzo-de-2016
descomposición de una serie de tiempo y exportar a excel
## 30 de mao de 2016

## descomposicion!!

ICA <- ts(read.csv(("C:\\Users\\SALA-C17\\Documents\\table (1).csv"), header = TRUE), frequency = 252, start = 2000)
ICA
## header encabezado cierto, frecuencia 252 porque es diario y la bolsa solo opera entre semana

## la descomposicion de la serie de tiempo nos permite conocer la estabilidad, la tendencia y la aleatoriedad de la st
## para descomponer una serie de tiempo se utiliza a funcion de decompose()
## para descomponer una serie de tiempo

## existen dos metodos para la descomposicion el modelo aditivo y el multiplicativo

icaaditivo <- decompose(ICA)
plot(decompose(ICA))
plot(icaaditivo)

## en estos casos ns dee de dar los misos resltados
## ahora como vimos la clase pasada hay dos models que podemos descomponer la st
## 1) modelo aditivo -> xt = mt + s + zt
## 2) modeo mutiplicativo -> xt = mt * st + zt
### ahora para descomponer una serie en un modelo aditivo....
icaadi <- decompose(ICA)  ## aqui es para descomponer la serie de tiempo por default nos da
## el modelo aditivo
## ahora para descomponer una ST en un modelo multiplicatvo
 help(decompose)
icamult <- decompose(ICA, type = "mult")

## graficar los dos modelos
plot(icaadi)
plot(icamult)

## ahora bien como habiamos mencionadoa al descomponer un st diversos elementos como la estacionalidad, la tendencia 
## y la aleatoriedad... son elementos que nos interesan, para esto se ocupa names()

names(icaadi) ## con names mandamos llamar los elementos que tiene el objeto
icaadi$type
names(icamult)
## ahora para asignar un obejto para los elementos  que nos interesan se realiza lo siguiente
tendeciaadi <- icaadi$trend # asignamos la tendencia del modelo aditivo
icaadi$trend

tendmut <- icamult$trend ## asignamos la tendencia del modelo multiplicativo
estadi <- icaadi$seasonal # asiagnamos la estacionalidad del modelo aditivo
estmult <- icamult$seasonal # asiagnamos la estacionalidad del modelo multiplicativo

## ahora para graficar la estacionanalidad, tedencia y todo eso
ts.plot(cbind(tendeciaadi, tendeciaadi*estadi), lty = 1:2, col = c(30,60)) ## lty sirve para ver doferentes tipos de lineas

## alunas funciones utilizadas
## ts() genera una serie de iempo de un ojeto
## window () extrae o divide una sere de tiempo
## ts.plot grafica la serie de tiepo
## decompose() descompone una serie de tiempo en sus principales elementos

## ejercicio con la base de gruma descomponer en modelo aditivo y multiplicatvo
## extraer la tendencia, estacionalidad y aleatoriedad
## exportar la tendencia, estacionalidad y aleatoriedad en tres columnas en excel
## graficar tendencia y tedecia pr estacionalidad en R

gruma <- ts(read.csv(("C:\\Users\\SALA-C17\\Downloads\\gruma.csv"), header = TRUE), frequency = 252, start = 2000)
View (gruma)
grumaaditivo <- decompose(gruma)
grumamultiplicativo <- decompose(gruma, type = "mult")

tendmult <- grumamultiplicativo$trend
estmult <- grumamultiplicativo$seasonal
aleamult <- grumamultiplicativo$random

View(tendmult)

tendadi <- grumaaditivo$trend
estadi <- grumaaditivo$seasonal
aleaadi <- grumaaditivo$random

##graficas
ts.plot(cbind(tendmult, tendmult*estmult), main = "Modelo multiplicativo", lty = 1:2, col = c(30,60))
ts.plot(cbind(tendadi, tendadi*estadi), main = "Modelo aditivo", lty = 1:2, col = c(20,40))

##para exportar
install.packages('XLConnect')

library(XLConnect)

vignette('XLConnect') # Manual (viñeta) es muy útil

wb <- loadWorkbook('Libro gruma.xlsx', create = TRUE) 
createSheet(wb, name = "modelo multiplicativo")
writeWorksheet(wb, tendmult, sheet = "modelo multiplicativo", startRow = 1, startCol = 1)
writeWorksheet(wb, estmult, sheet = "modelo multiplicativo", startRow = 1, startCol = 2)
writeWorksheet(wb, aleamult, sheet = "modelo multiplicativo", startRow = 1, startCol = 3)
createSheet(wb, name = "modelo aditivo")
writeWorksheet(wb, tendadi, sheet = "modelo aditivo", startRow = 1, startCol = 1)
writeWorksheet(wb, estadi, sheet = "modelo aditivo", startRow = 1, startCol = 2)
writeWorksheet(wb, aleaadi, sheet = "modelo aditivo", startRow = 1, startCol = 3)
saveWorkbook(wb)
