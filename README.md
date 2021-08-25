CovCoagEmaValidation
======================

Introduction
============
This package contains code to externally validate models for the prediction of coagulopathy in unvaccinated COVID-19 patients developed on CPRD Aurum.

Features
========
  - Applies models developed using the OHDSI PatientLevelPrediction package
  - Evaluates the performance of the models on new data
  - Packages up the results to share with study owner

Technology
==========
  CovCoagEmaValidation is an R package.

System Requirements
===================
  * Requires: OMOP CDM database and connection details
  * Requires: Java runtime enviroment (for the database connection)
  * Requires: R (version 3.3.0 or higher).

Dependencies
============
  * **PatientLevelPrediction:** This validation package relies on experimental features that have not yet been merged into the master branch of OHDSI's PatientLevelPrediction package. Therefore, to run this package you currently require the latest PatientLevelPrediction version from the “develop” branch, which you can find here: https://github.com/OHDSI/PatientLevelPrediction/tree/develop

Building the package inside RStudio
===============
  1. Download or clone the validation package to your execution environment. 
  2. Open the validation package project file (file ending in .Rproj) 
  3. Build the package in RStudio by selecting the 'Build' tab in the top right and then clicking on the 'Install and Restart'

Executing the validation study
===============
  1. In R, run the code in 'extras/codeToRun.R'. The parameters that you have to provide to establish the database connection are `dbms`, `server`, `user`, `password`, `port`. If you have not set the path to your database drivers as an environmental variable, you may have to also provide the `pathToDriver` parameter to the `DatabaseConnector::createConnectionDetails()` function. Moreover, you will have to provide `cdmDatabaseSchema`, `cohortDatabaseSchema`, `databaseName`. The remaining variables and parameters have been set by us.
  2. The results will be packaged into a .zip file if `packageResults = TRUE` as is currently the default.
  3. Unpack and inspect the content of the .zip file once again, before sharing it with us, to make sure you comply with your organization's data sharing policies.

Example Code
===============
```r
library(CovCoagEmaValidation)

# the location to save the models validation results to:
outputFolder <- './Validation'

# add the database connection details
dbms = 'your database management system'
server = 'your server'
user = 'your username'
password = 'top secret'
port = 'your port'
connectionDetails <- DatabaseConnector::createConnectionDetails(dbms = dbms,
                                                                server = server,
                                                                user = user,
                                                                password = pw,
                                                                port = port)

# add cdm database details:
cdmDatabaseSchema <- 'your cdm database schema'

# add a schema you have read/write access to
# this is where the cohorts will be created (or are already created)
cohortDatabaseSchema <- 'your cohort database schema'

# if using oracle specify the temp schema
oracleTempSchema <- NULL

# Add a sharebale name for the database containing the OMOP CDM data
databaseName <- 'your database name'

# table name where the cohorts will be generated
cohortTable <- 'CovCoagEmaValidationCohort'

#===== execution choices =====

# how much details do you want for in progress report?
verbosity <- "INFO"

# create the cohorts using the sql in the package?
createCohorts = T

# apply the models in the package to your data?
runValidation = T
# if you only want to apply models to a sample of
# patients put the number as the sampleSize
sampleSize = NULL
# do you want to recalibrate results?
# NULL means none (see ?CovCoagEmaValidation::execute for options)
recalibrate <- c("recalibrationInTheLarge", "weakRecalibration")

# extract the results to share as a zip file?
packageResults = T
# when extracting results - what is the min cell count?
minCellCount = 5

#=============================

# Now run the study
CovCoagEmaValidation::execute(connectionDetails = connectionDetails,
                                           databaseName = databaseName,
                                           cdmVersion = 5,
                                           cdmDatabaseSchema = cdmDatabaseSchema,
                                           cohortDatabaseSchema = cohortDatabaseSchema,
                                           oracleTempSchema = oracleTempSchema,
                                           cohortTable = cohortTable,
                                           outputFolder = outputFolder,
                                           createCohorts = createCohorts,
                                           recalibrate = recalibrate,
                                           runValidation = runValidation,
                                           packageResults = packageResults,
                                           minCellCount = minCellCount,
                                           sampleSize = sampleSize,
                                           verbosity = verbosity)
```

License
=======
  CovCoagEmaValidation is licensed under Apache License 2.0

Development
===========
  CovCoagEmaValidation is being developed in R Studio.
