# MSB1015_Assignment2
## Welcome!
Welcome to the repository of Assignment 2 for the course MSB1015 'Scientific Programming' 2019, taught at Maastricht University. The assignment was to create a PLS model (Partial Least Squares model) to answer a research question using R Markdown. 

### Research question
#### Background information
Harry Wiener wrote a paper on the correlation between chemical structure of compounds and their physical properties (1). He used the polarity number p, the pairs of carbon atoms which are separated by three C-C bonds, and the path number w, the sum of the distances between any two carbon atoms in the molecule in terms of C-C bonds, to create a prediction model. In this model p and w are descriptors of the molecules that he wanted to predict the boiling points of. Ofcourse back then there were less boiling points and descriptors of compounds known.  
This project was based on his research. The research entailed to create a partial least squares (pls) model to predict the boiling points of alkanes through the use of SPARQL to query for chemical properties of alkanes on Wikidata, in an R markdown notebook. However, nowadays we have the luxury to choose more descriptors and create a model based on larger datasets. 

1. Wiener H. Structural Determination of Paraffin Boiling Points. Journal of theAmerican Chemical Society. 1947 Jan;69(1):17–20.

#### Set up for methods
The SPARQL query asks Wikidata to:   
- Provide a list of alkanes (compounds that are either an entity or subclass of 'alkanes'):   
    - ?comp wdtP31/wdt:P279* wd:Q41581   
- and provide their canonical SMILES (P233) under column name CC  
- and their boiling points (P2102) accompanied by the unit that the boiling point is in.  

Two functions were created to convert all boiling points into unit Kelvin. The for loop goes over all boiling points and converts the units that are in Celsius or Fahrenheit to Kelvin. The data retrieved from the query is then split into a test set (20%) and a training set (80%).  
The SMILES are then parsed to generate a list of IAtomContainer objects, which can be used to evaluate a list of chosen descriptors. The classes of descriptors chosen are:
- Fragment Complexity (the complexity of the compound)
- APoldescriptor (sum of the atomic polarizibilities) 
- Wiener Numbers Descriptors (the previously mentioned p and w)
- MDE Descriptor (Molecular Distance Edge) 
- Atom Count Descriptor (amount of atoms per element)  

The descriptors are used to create a PLS model. However, not all descriptors from the different classes are used. Multiple descriptors from the MDE Descriptor class provide empty vectors, or hardly any values. Therefore, only descriptors which provide values for most of the compounds were used to create the PLS model.   

The PLS model is assessed using a Root Mean Square Error of Prediction (RMSEP) function. Using the RMSEP the number of components to create the model is evaluated. The RMSEP declines until ncomp = 3, after which the RMSEP stagnates, regardless of how many components are added. Using this information, the predict function is used to predict the boiling points of the test set, using the descriptors that were used to create the model with ncomp = 3. 
The correlation coefficient was used to asses the accuracy of the predicted boiling points vs the test set. Depending on which compounds are randomly distributed over the training and test set per run of the file, the correlation coefficient reaches 0.99 usually.

### Reproducibility
To assess the reproducibility of the code written to create the PLS model for the alkane boiling points, a second model was created. Unfortunately, alcanes and alcynes boiling points were difficult to find. Another implementation was created. The code was simply copy pasted, and a few minor adjustments were made:
- The SPARQL query was adjusted to retrieve a list of amino acids and their mass
- The functions for the conversion of Celsius and Fahrenheit to Kelvin were not used
- The names were changed to be able to compare the outputs of the different models (but this was not necessary for the code to run)  

The descriptors were kept the same, to be able to compare both models. This was not ideal, as the descriptors chosen for the boiling point model, were chosen specifically to include the chemical properties that contribute to a compound's boiling point. This second model was created to assess the reproducibility and robustness of the code and not the accuracy of the output. 
The code has shown to be reproducible and robust. The model was created in little time and is able to predict an amino acid's mass with a correlation coefficient of >0.97 (depending on the distribution of the data over test and training set). The fact that the descriptors were not chosen to predict a compound's mass probably contributes to the fact that the RMSEP of the model keeps decreasing up to 5 components instead of 3. 

### How to run the code


### Sources used for template code
The packages RCDK and WikidataQueryServiceR (WDQS) both had a package documentation available online. The documentation files can be found through the links below.
The rcdk package:  
https://cran.r-project.org/web/packages/rcdk/rcdk.pdf  
Version 3.4.7.1 of the rcdk package was used for this project.

The WDQS package:  
https://cran.r-project.org/web/packages/WikidataQueryServiceR/WikidataQueryServiceR.pdf  
Version 0.1.1 of the WDQS package was used for this project.

To create the correct queries, the lectures and computer practicals from the course MSB1015 Scientific Programming were used, as well as suggestions made by E. Willighagen. 

To create the model, the Partial Least Squares (pls) package manual was used for template code.   
The pls package:  
https://cran.r-project.org/web/packages/pls/vignettes/pls-manual.pdf  
The version of this manual used for this project, was last updated on October 1, 2019.

### List of author(s)
Wiep Stikvoort

### Thank you
I would like to thank E. Willighagen for his contribution to the SPARQL code. 
