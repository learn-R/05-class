
# Práctico 3: Transformar y seleccionar variables ----------------------


# 0. Instalar paquetes ----------------------------------------------------

install.packages("pacman")
library (pacman)

pacman::p_load(tidyverse,
               magrittr,
               car,
               sjmisc)

# 1. Importar datos -------------------------------------------------------

# En este práctico, se trabajó con CASEN 2020. 

temp <- tempfile()
download.file("http://observatorio.ministeriodesarrollosocial.gob.cl/storage/docs/casen/2020/Casen_en_Pandemia_2020_STATA.dta.zip",temp)
datos <- haven::read_dta(unz(temp, "Casen en Pandemia 2020 STATA.dta"))
unlink(temp); remove(temp)

datos <- read_dta("../Rproject/input/Casen en Pandemia 2020 STATA.dta") 

## 1.1. Explorar los datos  -----------------------------------------------

# De paquete base
dim(datos) # nos entrega las dimensiones, es decir el numero de observaciones y el número de variables
View(datos) # se usa para visualizar la base de datos
names(datos) # entrega los nombres de las variables que componen el dataset
head(datos) # muestra las primeras filas presentes en el marco de datos

# De sjmisc

# find_var(datos, "concepto") # se utiliza para encontrar variables

# 2. Operadores relacionales -------------------------------------------------

# Se usan para hacer comparaciones. Cuando en la *Tabla 1* nos referimos a `un valor`, esto refiere tambien a `variables`
# 
#   | Símbolo  | Función |
#   |---------:|:--------|
#   | `<`      |  Un valor es menor que otro |
#   | `>`      |  Un valor es mayor que otro |
#   | `==`     |  Un valor es igual que otro [^1] |
#   | `<=`     |  Un valor es menor o igual que otro |
#   | `>=`     |  Un valor es mayor o igual que otro |
#   | `!=`     |  Un valor es distinto o diferente que otro|
#   | `%in%`   |  Un valor pertenece al conjunto designado [^2] |
#   | `is.na()`|  El valor es perdido o `NA` |
#   | `!is.na()`| El valor es distinto de  `NA` |


# 3. Operadores aritméticos -----------------------------------------------

# Realizan operaciones, como la suma, resta, división, entre otros.
# 
#   | Símbolo  | Función |
#   |---------:|:--------|
#   | `+`      |  Suma |
#   | `-`      |  Resta|
#   | `*`     |  Multiplicación |
#   | `/`     |  División |
#   | `^`     |  Elevado |

# 4. Operadores de asignación ---------------------------------------------


# Hay dos formas de asignar `objetoA <- objetoB` o `objetoA = objetoB`. Ambas 
# implican que lo que se este realizando en el *objetoB* implica que eso va a 
# producir o generar al *objetoA*


# 5. Operadores booleanos -------------------------------------------------

# Describen relaciones **lógicas** o **condicionales**
# 
# | Símbolo  | Función |
# |---------:|:--------|
# | `&`      |  Indica un *y* lógico |
# | `|`      |  Indica un *o* lógico |
# | `xor()`  |  Excluye la condición  |
# | `!`      |  Distinto de ... |
# | `any`    |  Ninguna de las condiciones serán utilizadas |
# | `all`    |  Todas las condiciones serán ocupadas |


# 6. Operador pipeline (%>%) ----------------------------------------------

# ¡Aquí mucha atención! Este operador `%>%` (llamado `pipe`) no es un operador que este contenido en las funciones base del lenguaje R. Este operador proviene de la función `magrittr` de `tidyverse`, y es de los operadores más útiles y utilizados en R.
# 
# ¿Para qué sirve? Para concatenar múltiples funciones y procesos. *Imagina que quieres filtrar una base de datos a partir de tramos etarios. Pero no tienes esa variable creada. ¿Que hacer? La respuesta: concatenar el proceso de creación de variables y luego filtrar.* Eso se puede hacer gracias a ` %>% ` (ya mostraremos como utilizar esta herramienta), que por lo demás es muy fácil de ejecutar.
# 
# - `Ctrl + shift + M` Para Windows
# - `⌘ + shift + M` Para Mac


# 7. Transformación de variables ---------------------------------------------

## 7.1. Seleccionar variables con dplyr ------------------------------------

### Selección por nombre de columna

# select(datos, variable1, variable2, variable3) #Incluir variables

# select(datos, -variable1) #Para excluir una variable, anteponer un signo menos (-)

### Selección por indexación

select(datos, 1, 2) # la primera y la segunda columna

select(datos, 1:4) # la primera hasta la cuarta columna

select(datos, c(1,4)) # la primera y la cuarta columna

### Selección renombrando

select(datos, edad, sexo, ocupacion = o1)

### Para reordenar variables
# Empleamos el argumento everything() para agregar todo el resto de variables.
# El data frame creado ordenará las variables según se asigne con select.

select(datos, id_persona, sexo, edad, everything())

### Con patrones de texto

# - `starts_with()`: prefijo 
# - `ends_with() `:  sufijo
# - `contains()` : contiene una cadena de texto literal
# - `matches()` : coincide con una expresión regular

select(datos, starts_with("a"), ends_with("preg"))

# También se pueden combinar con operadores logicos

select(datos, starts_with("y1")&ends_with("preg")) 
select(datos, contains("pobre")|contains("vivienda"))
select(datos, matches("pobreza_|vivienda"))

### Con condiciones lógicas

select(datos, where(is.character))

## 7.2. Selección de variables para el ejercicio ---------------------------

# Seleccionamos las siguientes variables
# - `edad`
# - `sexo`
# - `s13`: previsión de salud
# - `tot_per`: número de personas en el hogar
# - `ytoth`: ingresos totales del hogar
# - `o1`: ocupación
# - `y26d_total`: Monto del IFE
# - `y26d_hog`: ¿Alguien recibió el IFE?

datos_proc <- select(datos, edad, sexo, prev = 592, ocupacion = o1, 
                     tot_per, ytoth, starts_with("y26d_")&matches("total|hog"))


## 7.3. Filtrar datos con filter() -----------------------------------------

# filter(datos, condicion_para filtrar)
# Esta condición para filtrar podría ser, por ejemplo
# variable1 >= 3

### a) Con números

filter(datos_proc, edad >= 15)
filter(datos_proc, edad >= 15 & tot_per <7)

filter(datos_proc, ytoth == max(ytoth))

### b) Con caracteres

filter(datos_proc, sexo == "Mujer")
filter(datos_proc, sexo != "Hombre")

#### Dos condiciones con caracter

datos_proc$prev <- as_factor(datos_proc$prev)

filter(datos_proc, prev %in% c("Sistema Público FONASA", "ISAPRE"))


## 7.4. Transformación de variables con mutate() ---------------------------

#mutate(datos, nueva_variable = cálculo o condición)

mutate(datos_proc, nueva_variable = 3+2)
mutate(datos_proc, nueva_variable = 3+2,
       ingreso_percapita = ytoth/tot_per)

datos_proc %>%
  mutate(ingreso_percapita = ytoth/tot_per) %>% 
  filter(ingreso_percapita <= 1000000)


## 7.5. Recodificación con recode() ----------------------------------------

### a) dplyr

datos_proc %>% 
  mutate(sexo = recode(sexo, "Mujer" = "Femenino", "Hombre" = "Masculino"))

### b) car

datos_proc %$% 
  car::recode(.$sexo, c('"Mujer"="Femenino";"Hombre"= "Masculino"'), as.factor = T)


## 7.6. Construcción de variables condicionales con if_else() ---------------

datos_proc %>% 
  mutate(fonasa = if_else(prev == "Sistema Público FONASA", 1, 0))


# 7.7. case_when() para múltiples condiciones -----------------------------

# case_when(variable == condicion ~ valor1,
#           variable == condicion ~ valor2,
#           TRUE ~ NA_real)
# - Donde, TRUE indica "todo el resto", y el NA dependerá de la clase del 
# valor de recodificación

datos_proc %>% 
  mutate(edad_tramo = case_when(edad <=39 ~  "Joven",
                                edad > 39 & edad <=59 ~ "Adulto",
                                edad > 59 ~ "Adulto mayor",
                                TRUE ~ NA_character_)) %>% 
  select(edad, edad_tramo)


# 8. Transformar el set de datos ------------------------------------------

datos_proc <- datos_proc %>% 
  filter(edad >= 15 & tot_per <7) %>%
  mutate(ingreso_percapita = ytoth/tot_per,
         edad_tramo = case_when(edad <=39 ~  "Joven",
                                edad > 39 & edad <=59 ~ "Adulto",
                                edad > 59 ~ "Adulto mayor",
                                TRUE ~ NA_character_),
         fonasa = if_else(prev == "Sistema Público FONASA", 1, 0),
         ocupacion = as_factor(ocupacion)) %>%
  select(sexo, edad_tramo, ocupacion, ingreso_percapita, ife = y26d_hog)


# 9. Visualizar el set de datos -------------------------------------------

sjPlot::view_df(datos_proc)

# 10. Guardar los datos procesados ----------------------------------------

saveRDS(datos_proc, file = "output/data/datos_proc.rds")


