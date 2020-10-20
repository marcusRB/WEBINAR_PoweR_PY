# Python in Power BI

## Get Data

1. Prueba con librería **datasets**

```R
# Creación de una variable dataset
dataset1 = datasets::mtcars
```

2. Prueba con función `read_table`

```R
# Cargamos el fichero desde la nube
dataset_gijon <- read.csv("http://opendata.gijon.es/descargar.php?id=1&tipo=CSV",
                         encoding = 'UTF-8',
                         stringsAsFactors = FALSE)

# Alternativa con las columnas
datasetHeader <- c("EstacionID",
                   "EstacionName","Lat","Lon",
                   "Fecha_UTC",
                   "SO2","NO","NO2","CO","PM10","O3","dd","vv",
                   "TMP","HR","PRB","RS","LL","BEN",
                   "TOL","MXIL","PM25"
                   )
dataset_gijon_head <- read.csv("http://opendata.gijon.es/descargar.php?id=1&tipo=CSV",
                         encoding = 'UTF-8',
                         stringsAsFactors = FALSE,
                              header = FALSE,
                              col.names = datasetHeader)

```

```R
# Comenzaremos con la transformación de la columna fecha y su formato
dataset$Fecha_UTC <- ymd_hms(dataset$Fecha_UTC, quiet = FALSE, tz = "")
dataset$Date <- format(dataset$Fecha_UTC, "%Y-%m-%d")
dataset$Time <- format(dataset$Fecha_UTC, "%T")
dataset$Date <- as.POSIXct(dataset$Date, format = "%Y-%m-%d")

# Añadimos más columnas de fecha, mes, año y hora
gijon_lastWeek_2019$day <- format(gijon_lastWeek_2019$Fecha_UTC, "%d")
gijon_lastWeek_2019$month <- format(gijon_lastWeek_2019$Fecha_UTC, "%m")
gijon_lastWeek_2019$year <- format(gijon_lastWeek_2019$Fecha_UTC, "%Y")
gijon_lastWeek_2019$hour <- format(gijon_lastWeek_2019$Fecha_UTC, "%H")
gijon_lastWeek_2019$weekday <- weekdays(gijon_lastWeek_2019$Date)
```

## Visual con R

```R
## Example R mtcars
require(graphics)
pairs(mtcars, main = "mtcars data", gap = 1/4)
coplot(mpg ~ disp | as.factor(cyl), data = mtcars,
       panel = panel.smooth, rows = 1)
## possibly more meaningful, e.g., for summary() or bivariate plots:
mtcars2 <- within(mtcars, {
   vs <- factor(vs, labels = c("V", "S"))
   am <- factor(am, labels = c("automatic", "manual"))
   cyl  <- ordered(cyl)
   gear <- ordered(gear)
   carb <- ordered(carb)
```

```R
# Distribution mpg
library(ggplot2)
ggplot(mtcars, aes(mpg)) +
  geom_histogram(binwidth = 4) + xlab('Miles per Gallon') + ylab('Number of Cars') + 
   ggtitle('Distribution of Cars by Mileage')
   
# Distribution cylinders
ggplot(mtcars, aes(cyl)) +
  geom_histogram(binwidth=1) + xlab('Cylinders') + ylab('Number of Cars') +
   ggtitle('Distribution of Cars by Cylinders')
   
# Distribution HP
ggplot(mtcars, aes(hp)) +
  geom_histogram(binwidth=20) + xlab('horsepower') + ylab('Number of Cars') +
  ggtitle('Distribution of Cars by Horsepower')

# Correlation
cor(mtcars$mpg, mtcars$hp)

# Fitting the Data - HP vs. MPG
ggplot(mtcars, aes(hp, mpg)) + geom_point() +
  geom_smooth(method = "lm", se = FALSE) +
  ylab("Miles per Gallon") +
  xlab("No. of Horsepower") +
  ggtitle("Impact of Number of Horsepower on MPG")

#apply smoothing since mpg unlikely to hit zero
ggplot(mtcars, aes(hp, mpg)) +
  stat_smooth() + geom_point() +
  ylab("Miles per Gallon") +
  xlab ("No. of Horsepower") +
  ggtitle("Impact of Number of Horsepower on MPG")

## Effect on Number of Cylinders on MPG
qplot(cyl, mpg, data = mtcars, colour = cyl, geom = "point",     
  ylab = "Miles per Gallon", xlab = "No. of Cylinders",
  main = "Impact of Number of Cylinders on MPG")  

# Fitting the Data - Cyl vs. MPG
ggplot(mtcars, aes(cyl, mpg)) + geom_point() +
  geom_smooth(method = "lm", se = FALSE) +
  ylab("Miles per Gallon") + xlab("No. of Cylinders") +
  ggtitle("Impact of Number of Cylinders on MPG")
```



