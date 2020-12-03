# Ejemplo 2. Creación de un Dashboard con pestañas y data tables

#### Objetivo

#### Requisitos

#### Desarrollo


Continuando con la base del ejemplo anterior, ahora lo que haremos es agregar imágenes dentro de nuestro panel en el archivo **`ui.R`**

```R
library(shiny)
shinyUI(
    pageWithSidebar(
        headerPanel("Aplicacion basica con Shiny"),
        sidebarPanel(
            p("Crear plots con el DF 'auto'"), 
            selectInput("x", "Seleccione el valor de X",
                        choices = names(mtcars))
        ),
        mainPanel(
                    h3(textOutput("output_text")), 
                    plotOutput("output_plot"), 
              
              #Agregamos una imágen
                    img( src = "cor_mtcars.png", 
                    height = 450, width = 450)
              
            )
        )
    )
    

```

Esto podemos organizarlo de mejor manera agregando pestañas con el comando `tabsetPanel` y con `tabPanel`para cada pestaña indivudual

```R
library(shiny)


shinyUI(
    pageWithSidebar(
        headerPanel("Aplicacion basica con Shiny"),
        sidebarPanel(
            p("Crear plots con el DF 'auto'"), 
            selectInput("x", "Seleccione el valor de X",
                        choices = names(mtcars))
        ),
       
       mainPanel(
          
    #Agregando pestañas     # <-----------
tabsetPanel(              # <-----------
        tabPanel("Plots",   #Pestaña de Plots  <-----------
                 h3(textOutput("output_text")), 
                 plotOutput("output_plot"),
                 ),
        
        tabPanel("imágenes",  #Pestaña de imágenes
                 img( src = "cor_mtcars.png", 
                      height = 450, width = 450)
                ), 
        
        #Aprovehamos y agregamos las siguientes pestañas # <-----------
        tabPanel("Summary"),
        tabPanel("Table"),
        tabPanel("Data Table")
    )
)
)

)

```

Ahora hay que agregar la información que desplegará cada una de esas pestañas en el archivo **`server.R`**

library(shiny)

shinyServer(function(input, output) {

 output$output_text <- renderText(paste("mpg~", input$x))   #Titulo del main Panel
 
 #Gráficas                       <----------
 output$output_plot <- renderPlot( plot( as.formula(paste("mpg ~", input$x)),
                                          data = mtcars) )
 
  #imprimiendo el summary       <----------                                  
 output$summary <- renderPrint({
     summary(mtcars)
    })
     
 # Agregando el dataframe       <----------
 output$table <- renderTable({ 
     data.frame(mtcars)
     })
 
 #Agregando el data table       <----------
 output$data_table <- renderDataTable({mtcars}, 
                                      options = list(aLengthMenu = c(5,25,50),
                                                     iDisplayLength = 5))
                                    
       
})

