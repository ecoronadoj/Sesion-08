# Ejemplo 4. Dashboard condicional

#### Objetivo}

#### Requisitos

#### Desarrollo

Para el archivo `ui.R` utilizar el siguiente código

```R
#Condicional
library(shiny)


shinyUI(fluidPage(

    # Application title
    titlePanel("Elecciones condicionales"),

    # Sidebar with a slider input for number of bins
    sidebarPanel(
       selectInput("plot_type", "Tipo de Gráfica", 
                   c("Gráfica de dispersión" = "Scatter", 
                     "Histograma" = "histogram")) ,
       
       conditionalPanel(condition = "input.plot_type != 'Scatter' ",
                        ),
       
       conditionalPanel(condition = "input.plot_type != 'histogram' ", 
                        selectInput("x", "Selecciona la variable en eje X", 
                                    choices = names(mtcars)
                                    ))
        
        
        ),

        # Show a plot of the generated distribution
        mainPanel(
            plotOutput("plot")
        )
    )
)
```

Dentro del archivo `Server.R` pegar el siguiente código

```R
#Condicional

library(shiny)


shinyServer(function(input, output) {

 output$plot <- renderPlot({
    if (input$plot_type == "histogram") {
        hist(mtcars[,input$x], xlab =input$x, main = paste("Histograma de",input$x) )
        
 }  else  {
         plot(mtcars[,input$x], mtcars$hp, xlab = input$x, ylab = "hp")
 }      

 })
 
})
```


