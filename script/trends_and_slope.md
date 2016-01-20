En primer lugar leemos los datos

``` r
################################################################
# Load packages 
library('wq')
```

    ## Loading required package: zoo
    ## 
    ## Attaching package: 'zoo'
    ## 
    ## The following objects are masked from 'package:base':
    ## 
    ##     as.Date, as.Date.numeric

``` r
library('reshape2')
library('dplyr')
```

    ## 
    ## Attaching package: 'dplyr'
    ## 
    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag
    ## 
    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
################################################################

################################################################
# Read data
iv <- read.csv(file=paste(di, '/data/raw_iv.csv', sep=''), header=T, sep=',')
snow <- read.csv(file=paste(di, '/data/raw_snow.csv', sep=''), header=T, sep=',')
################################################################
```

A continuación aplicamos la técnica de Mann-Kendall-Theil-Sen para computar la tendencia y la pendiente de cada variable.

> iv

``` r
################################################################
# Loop to compute the MKT by indicator

# Define name of indicators (see variables names)
indicadores <- c('ndvi_i_invierno','ndvi_i_primavera', 'ndvi_i_verano', 'ndvi_i_otono')

# subset by indicator
for (j in indicadores){ 
  # Create a subset by indicator
  subdf <- iv[, names(iv) %in% c('iv_malla_modi_id', 'ano', j )]
  
  # Manipule data. Transpose 
  # la funcion aggregate es porque tenemos duplicados 
  aux <- dcast(subdf, ano ~ iv_malla_modi_id, value.var = j, fun.aggregate = mean) 

  # Create a zoo object 
  auxts <- zoo(aux[-1],aux[,1])
  
  # Compute Mann Kendall and Theil 
  theil <- mannKen(as.ts(auxts))
  
  # Prepare data output
  theil <- as.data.frame(theil)
  theil$iv_malla_modi_id <- rownames(theil)
  
  # Round the output
  theil$sen.slope <- round(theil$sen.slope, 3)
  theil$sen.slope.pct <- round(theil$sen.slope.pct,2)
  theil$p.value <- round(theil$p.value, 5)
  theil$varS <- round(theil$varS, 2)
  theil$tau <- round(theil$tau, 3)
  
  # Add variable name 
  theil$variable <- rep(j, nrow(theil))
  
  # Assign output to an dataframe
  assign(j, theil)
  
  }
```

> snow

``` r
################################################################
# Loop to compute the MKT by indicator

# Define name of indicators (see variables names)
indicadores <- c('scd', 'scod', 'scmd')

# subset by indicator
for (j in indicadores){ 
  # Create a subset by indicator
  subdf <- snow[, names(snow) %in% c('nie_malla_modi_id', 'ano', j )]
  
  # Manipule data. Transpose 
  # la funcion aggregate es porque tenemos duplicados 
  aux <- dcast(subdf, ano ~ nie_malla_modi_id, value.var = j, fun.aggregate = mean) 

  # Create a zoo object 
  auxts <- zoo(aux[-1],aux[,1])
  
  # Compute Mann Kendall and Theil 
  theil <- mannKen(as.ts(auxts))
  
  # Prepare data output
  theil <- as.data.frame(theil)
  theil$nie_malla_modi_id <- rownames(theil)
  
  # Round the output
  theil$sen.slope <- round(theil$sen.slope, 3)
  theil$sen.slope.pct <- round(theil$sen.slope.pct,2)
  theil$p.value <- round(theil$p.value, 5)
  theil$varS <- round(theil$varS, 2)
  theil$tau <- round(theil$tau, 3)
  
  # Add variable name 
  theil$variable <- rep(j, nrow(theil))
  
  # Assign output to an dataframe
  assign(j, theil)
  
  }
```

### Análisis de datos para comentarios MS\_ECOSISTEMAS

-   *Los resultados mas destacables muestran que el 75 % de los pixeles que cubren los bosques de Q. pyrenaica presentan una tendencia positiva significativa en el valor del NDVI de verano*

``` r
# Porcentaje de pixeles con tau positivo  
nrow(ndvi_i_verano[ndvi_i_verano$tau > 0.25,]) / nrow(ndvi_i_verano)
```

    ## [1] 0.7502756

``` r
adelantados <- ndvi_i_verano[ndvi_i_verano$tau > 0.25,]
mean(adelantados$sen.slope)
```

    ## [1] 0.04265467

``` r
mean(adelantados$sen.slope.pct)
```

    ## [1] 1.339816
