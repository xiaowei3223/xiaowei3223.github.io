# Copy and paste the following code to R in order to install all required dependencies:
if (!requireNamespace("BiocManager", quietly = TRUE))
install.packages("BiocManager", dependencies = TRUE)
requiredPackages <- c("devtools", "Biostrings", "Rsamtools", "GenomicAlignments", "polspline", "GenomicRanges", "shiny", "shinythemes", "shinyjs", "tidyr", "ggplot2", "markdown", "shinydashboard", "shinycssloaders", "seq2pathway", "chipenrich", "htmlwidgets", "chipenrich.data")
newPackages <- requiredPackages[!(requiredPackages %in% installed.packages()[,"Package"])]
if(length(newPackages)) BiocManager::install(newPackages, ask = TRUE)
