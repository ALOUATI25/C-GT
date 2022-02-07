library(shinydashboard)
library(shiny)
library(ggplot2)
library(DT)
library(haven)
library(rsconnect)

rsconnect::setAccountInfo(name='mtchy', token='7F62152612C99267ABF41C178B84AAB7', secret='YiNUrwHKG22hx41ddjmIzRZ76sEv+ruQOSADnWYo')



A <-read_sas("I:\\Services\\ClinicalDataManagement\\SAS\\SAS_Prod\\Studies\\ROCC\\ROCC-PH-005\\Outputs\\Rec\\20220207\\ALL_TPs.sas7bdat")
B <-read_sas("I:\\Services\\ClinicalDataManagement\\SAS\\SAS_Prod\\Studies\\ROCC\\ROCC-PH-005\\Outputs\\Rec\\20220207\\TPs_Completor.sas7bdat")
C <-read_sas("I:\\Services\\ClinicalDataManagement\\SAS\\SAS_Prod\\Studies\\ROCC\\ROCC-PH-005\\Outputs\\Rec\\20220207\\Patients_Completor.sas7bdat")


ui <- dashboardPage(
  dashboardHeader(title= "Patients/TPs completor"),
  dashboardSidebar(
    sidebarMenu(
      menuItem("Iris", tabName = "iris", icon = icon("tree")),
      menuItem("ROCC-PH-005", tabName = "ROCC-PH-005", icon = icon("table")),
      menuItem("ROCC-PH-004", tabName = "ROCC-PH-004", icon = icon("table"))
    )
  ),
  dashboardBody(
    
    tabItems(
      tabItem("iris",
              #the iris page
              box(plotOutput("correlation_plot"), width = 6),
              box(
                #input tab to select what axis you'd like to compare to
                selectInput("features", "Features:", 
                            c("Sepal.Width", "Petal.Length",
                              "Petal.Width")), width = 4
              ),
              
              textOutput("textOutput")
      ), 
      tabItem("ROCC-PH-005",
              fluidPage(
                fluidRow(
                  mainPanel(
                    tabsetPanel(
                      id = 'dataset',
                      tabPanel("ALL_TPs", DT::dataTableOutput("mytable1")),
                      tabPanel("TPs_Completor", DT::dataTableOutput("mytable2")),
                      tabPanel("Patients_Completor", DT::dataTableOutput("mytable3"))
                    )
                  )
                  
                  #box(title = "Top Performer", plotOutput("correlation_plot"))
                )
              ),
              
      ),
      tabItem("ROCC-PH-004",
              fluidPage(
                fluidRow(
                  mainPanel(
                    tabsetPanel(
                      id = 'dataset',
                      tabPanel("rs", DT::dataTableOutput("mytable6")),
                      tabPanel("rss", DT::dataTableOutput("mytable7")),
                      tabPanel("iris", DT::dataTableOutput("mytable8"))
                    )
                  )
                  
                  #box(title = "Top Performer", plotOutput("correlation_plot"))
                )
              ),
              
      )
      
      
      
    )
  )
)


server <- function(input, output){
  
  
  
  output$mytable1 <- DT::renderDataTable({
    DT::datatable(A)
  })
  
  
  output$mytable2 <- DT::renderDataTable({
    DT::datatable(B)
  })
  
  output$mytable3 <- DT::renderDataTable({
    DT::datatable(C)
  }) 
}

shinyApp(ui, server)
deployApp()

