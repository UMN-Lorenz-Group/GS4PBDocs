# Complete Manual
## Welcome to the GS4PB wiki!

### The GS4PB (Previously SOYGEN2) App implements a genomic selection pipeline to integrate genotyping, phenotyping, and envirotyping data. The pipeline supports GS for single traits and multi-traits across single and multiple environments.
 


![GS4PB_Intro](https://github.com/user-attachments/assets/6fb38883-0348-4fbc-a70f-e2130a6b148c)

# Recommended Method to Run the Application

## I. Using the Docker Container

1. **Install Docker Engine**  
   - Download and install the Docker engine for your operating system from [here](https://docs.docker.com/engine/install/).  
   - Ensure the Docker engine is running when executing Docker commands.  
   - For a quick introduction to running Docker containers, check out this short video: [Docker Desktop Introduction](https://docs.docker.com/get-started/introduction/get-docker-desktop/).

2. **Pull the Docker Image**  
   ```bash
   docker pull ivanvishnu/gs4pb:updated
   ```

3. **Run the Docker Container**  
   a) **On Git Bash (Windows)**  
   - **With Local Directory Binding**  
     Replace `source` with your target directory:
     ```bash
     winpty docker run --mount type=bind,source=$HOME/Downloads/,target=/root/Results -it -p 3838:3838 ivanvishnu/gs4pb:updated
     ```

   - **Without Local Directory Binding**  
     ```bash
     winpty docker run -it -p 3838:3838 ivanvishnu/gs4pb:updated
     ```
     After execution, copy the results to your local directory using:
     ```bash
     docker cp <container_id>:/root/Results/ $HOME/Downloads/
     ```

   b) **On Other Systems**  
   Replace `source` with your target directory:
   ```bash
   docker run --mount type=bind,source=$HOME/Downloads/,target=/root/Results -it -p 3838:3838 ivanvishnu/gs4pb:updated
   ```

4. **Access the Application**  
   Open your browser and navigate to [http://localhost:3838/](http://localhost:3838/).

---

## II. Installing and Running the Application in RStudio

1. Clone the repository using Git:  
   ```bash
   git clone https://github.com/UMN-Lorenz-Group/GS4PB.git
   ```

2. Open the `App/app.R` file in RStudio and run the application.

3. Follow the app’s guided instructions and move through the tabs for sequential implementation of pipeline steps.

---
## Introduction 
### The GS pipeline involves steps starting from data management to making selection decisions based on genome estimated breeding values. The schematic below depicts the steps in the pipeline. 

![PipelinePic_for_Wiki_V2](https://github.com/UMN-Lorenz-Group/GS4PB/assets/12753252/4d19f5ca-1443-461b-a837-78b9e813ae93)


## Genotypic Data Processing

### Data Upload

Genotypic data can be loaded in VCF file format. The genotypic data file should contain both the training data set and target data set, if any. It is also possible to upload just the file with training data set. If the data contains target set data, you also need to upload target line IDs in .csv format. Once uploaded, you need to select the column containing the strain IDs for the target set.

### Filter Data

Once uploaded the genotypic data can be processed further, sites (markers) in the genotype table can be filtered on minimum site fraction and minor allele frequency. Minimum site fraction corresponds to the threshold fraction of lines for which the markers can have missing values. For example, if you set it to 0.8, all the markers that have missing values in more than 80% of the lines will be removed. Taxa (lines/genotypes) can be filtered on minimum not missing, which corresponds to the fraction of markers for which the lines can have missing values. 

### Impute missing genotype scores
Missing genotypic scores can be imputed using three methods. 
1. LD-k-Nearest Neighbor Imputation (LDKNN) method, which has two parameters : number of high LD sites and number of neighboring samples. The default values are set to 30 and 10 respectively. 

2. Numeric imputation method, which has two parameters: number of nearest neighbors (default value of 5) and distance method that includes 'euclidean', 'manhattan' and 'cosine'.

3. AlpaPlantImpute method, which is a more involved method. It has two steps that includes a haplotype library build step and imputation step. The haplotype build step has three parameters: number of haplotypes, number of sample rounds, and non-missing threshold for high density genotypes. 
Once the haplotype library is built, the user can perform pedigree based or population based imputation. For pedigree based imputation, the user needs to upload a founders file. If no founder file is uploaded, the program implements a population based imputation method. 

 The imputed genotype table can be exported for further exploratory analysis, if required.  

  
Note: There are checkboxes in the filter and imputation page that allows user to select the table that must be used for the remaining steps. 

## Phenotypic Data Processing

### Data Upload

Phenotypic data from single and multi-environmental trials can be uploaded in CSV format. Tick the checkbox to load data from multi-environmental trials. Once you upload the file, the input tabs in the sidebar will be filled with information. The user needs to select the trait columns and columns with strain IDs and 'uniqID' column in the data table. The unique ID column is generally a combination of multiple factors such as Strain, Location and others such as Test, etc.. Once you set it, you can check if the unique ID column has as many unique IDs as the number of unique entries. 

After that, the user must select one or more traits for further analysis including cross validation and genomic prediction. A summary range of trait values are printed in the message box. For multi-env trial data, a plot with distribution of trait values across all the locations in the data is generated. 

Once you select a trait, the genotypic and phenotypic data are merged to create a merged data set. 

### Enviromics Data 
This is an optional step for users who want to use enviromics covariates in their GP models that include GxE effects.  
In this panel, the user can extract several environmental variables for temperature, precipitation, radiation and also soil variables from public databases. The current version uses the pipeline implemented in the 'EnvRtype' package to extract this data from NASA POWER database and altitude is extracted from SRTM. 

The user needs to upload the coordinates file for the locations in the following format: 

<html xmlns:m="http://schemas.microsoft.com/office/2004/12/omml"
xmlns="http://www.w3.org/TR/REC-html40">

<head>

<meta name=ProgId content=PowerPoint.Slide>
<meta name=Generator content="Microsoft PowerPoint 15">
<style>
<!--tr
	{mso-height-source:auto;}
col
	{mso-width-source:auto;}
td
	{padding-top:1.0px;
	padding-right:1.0px;
	padding-left:1.0px;
	mso-ignore:padding;
	color:windowtext;
	font-size:18.0pt;
	font-weight:400;
	font-style:normal;
	text-decoration:none;
	font-family:Arial;
	mso-generic-font-family:auto;
	mso-font-charset:0;
	text-align:general;
	vertical-align:bottom;
	border:none;
	mso-background-source:auto;
	mso-pattern:auto;}
.oa1
	{border-top:1.0pt solid white;
	border-right:1.0pt solid white;
	border-bottom:3.0pt solid white;
	border-left:1.0pt solid white;
	background:#156082;
	mso-pattern:auto none;
	text-align:center;
	padding-left:.5pt;
	padding-top:.5pt;
	padding-right:.5pt;}
.oa2
	{border-top:3.0pt solid white;
	border-right:1.0pt solid white;
	border-bottom:1.0pt solid white;
	border-left:1.0pt solid white;
	background:#CCD2D8;
	mso-pattern:auto none;
	text-align:center;
	padding-left:.5pt;
	padding-top:.5pt;
	padding-right:.5pt;}
.oa3
	{border:1.0pt solid white;
	background:#E7EAED;
	mso-pattern:auto none;
	text-align:center;
	padding-left:.5pt;
	padding-top:.5pt;
	padding-right:.5pt;}
-->
</style>
</head>

<body>
<!--StartFragment-->


Location | Country | Lat | Long | OtherLocName | LocName
-- | -- | -- | -- | -- | --
Becker-MN | USA1 | 45.393 | -93.878 | BE | Becker
Crookston-MN | USA1 | 47.775 | -96.606 | CR | Crookston



<!--EndFragment-->
</body>

</html>

The user also needs to set the start and end date for daily weather data collection. In addition, the user can choose to get processed weather data and choose the kernel (linear or gaussian) for environmental relationship. If the user wants to use the location names in the OtherLocName column in the environmental relationship heatmap, check the use otherlocnames checkbox. 























































## Training Set Optimization using STPGA (Selection of Training Sets using Genetic Algorithms) 

A training population for genomic prediction is selected by optimizing an objective function using a genetic algorithm.
The optimization method uses the 'GenAlgForSubsetSelection' function implemented in the 'STPGA' R package (Akdemir 2017).
In this implementation, the parameters for the genetic algorithm are set to default values. The user needs to set the candidate and training population size. Candidate population refers to the set of genotypes with phenotypic data. The default size is set to the number of genotypes in the input genotypic data that are not in the target population set. Training population size refers to the number of genotypes that are to be selected in the training population. The default value is set to 80% of the candidate set. 




## Cross Validation (CV)

### Single Trait CV
For single trait genomic prediction models, the user can k-fold cross validation for single trait using the 'bWGR' package. We've preselected three models out of the many available in the package. 

### Multi-trait CV

For multi-trait genomic prediction models, k-fold CV is performed for three models implemented in the 'BGLR' package including BRR, RKHS, and Spikes-slab. This allows the user to evaluate the available models and select the model with the best prediction accuracy for genomic prediction. The parameters to set include the number of folds 'k' and the number of replicates. 

### Multi-environmental CV
For multi-environmental data, the user can perform five cross validation schemes to select the best model for his/her objective. Currently, the GP models implemented in the BGGE/EnvRtype packages are compared in the cross validation schemes. Three main models with "Main Effect (G+E)", "Homogeneous Variance (G+E+GxE)","Heterogeneous Variance (G+E+GxEi)" with and without enviromic covariates are compared.  

The five CV schemes include: 

1. CV1, where novel genotypes in tested environments are predicted.
2. CV2, where tested genotypes in tested environments are predicted.
3. CV0, where tested genotypes in untested novel environments are predicted.
4. CV00, where novel genotypes in novel environments are predicted.
5. CV LOFO (Leave One Factor Out), eg: Leave One Test Out/ Leave One Line Out cross validation. 

                                       
 
 

## Genomic Prediction 

### Single trait genomic prediction

Single trait genomic prediction is performed using methods implemented in the 'bWGR' package. rrBLUP method is implemented using the 'rrBLUP' (Endelman 2011) package. Expectation maximization based Ridge Regression, BayesB and BayesLASSO methods are implemented using the bWGR package (Xavier et al. 2020). The output table contains sorted predicted values and an upper bound of reliability (Yu et al., 2016).  

### Multi-trait genomic prediction 

Currently, multi-trait genomic prediction is performed using methods implemented in the 'BGLR' package. The Multitrait function in BGLR implements Bayesian Ridge Regression, RKHS, and Spike-Slab methods (Perez-Rodriguez P and de Los Campos G, 2022). Multi-trait GBLUP is implemented using the 'mmer' function in 'sommer' package (Covarrubias-Pazaran G 2016). 


### Multi-environmental genomic prediction 

Environments are defined as combinations of year and locations in which phenotypic data are collected.                                                 Multi-environmental models are implemented using the EnvRType/BGGE pipeline as well as 'mmer' function in 'sommer' package.
Fit genomic prediction models taking into account only the main effects or the main effects + GxE effects. The user needs to select one or many years and locations and the type of variance-covariance structure for fitting the model.








