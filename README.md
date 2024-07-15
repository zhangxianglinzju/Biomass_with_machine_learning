# Biomass_with_machine_learning
<!-- This is the markdown template for the final project of the Building AI course, 
created by Reaktor Innovations and University of Helsinki. 
Copy the template, paste it to your GitHub README and edit! -->

# Project Title

Biomass_with_machine_learning

## Summary

Describe briefly in 2-3 sentences what your project is about. About 250 characters is a nice length! 
My project aims to develop a machine learning methods in Google Earth Engine to predicting forest biomass with environmental covariates and remote sensing. The library includes data collecting, feature selection, model training, model testing, and finally we will get a spatial biomass map.

## Background

Biomass is crucial for the carbon situation for the forest. How to quickly map the spatial and temporal distribution patterns is crucial for us to know the carbon, and solve the problem of climate change.

This is how you make a list, if you need one:
* Get the biomass sample dataset
* Fit machine learning model in Google Earth Engine
* Map in Google Earth Engine
* 
![image](https://github.com/user-attachments/assets/4776e46a-ff06-4bb4-90b0-71310955de0f)


## How is it used?

The procedure could be organized in these following parts.

Data Collection and Processing

Data Acquisition: Download open biomass dataset, including sample coordinates and biomass values.

Feature selection: Use some algorithms, like boruta and RFE to select the most infomative environmental covariates and remote sensing bands.

Model fitness Fit model with some machine learning and deep learning methods in Google Earth Engine

Perfromance test Evaluate model performance using some statictic indices such as R², RMSE and MAE

Spatial prediction Predict in the spatial scale

This is how you create code examples:
```
var trainingPoints = dataset.trainingPoints;
var testingPoints = dataset.testingPoints;

// fit the random forest model
// define the covariate list
var environmentalCovariateName = ee.List(['blue', 'green', 'red', 'nir', 'swir1', 
                                  'swir2', "NDVI", "EVI", "BSI", "NBR2"]);
                                  
// fit the model                                  
var rfModel = ee.Classifier.smileRandomForest({numberOfTrees:500})
                           .setOutputMode('REGRESSION')
                           .train({features: trainingPoints,
                                   classProperty:'biomass_aerial_ha4',
                                   inputProperties: environmentalCovariateName});
                                   
// accuracy assessment
var predictTest = testingPoints.classify(rfModel);
var RMSETest = RMSE("biomass_aerial_ha4","classification", predictTest);
var RSquareTest = RSquare("biomass_aerial_ha4","classification", predictTest);
var predictTrain = trainingPoints.classify(rfModel);
var RMSETrain = RMSE("biomass_aerial_ha4","classification", predictTrain);
var RSquareTrain = RSquare("biomass_aerial_ha4","classification", predictTrain);

// get the covaraite importance
var RFImportance = rfModel.explain();

// display the result 
print(RSquareTrain,'R2 training');
print(RMSETrain,'RMSE Training');
print(RSquareTest,'R2 Testing');
print(RMSETest,'RMSE Testing');
print(RFImportance,'importance');

var roi = ee.Geometry.Polygon(
        [[[-7.283728370707339, 43.342424127689846],
          [-7.283728370707339, 39.944769014774224],
          [-1.5598514175823386, 39.944769014774224],
          [-1.5598514175823386, 43.342424127689846]]], null, false);
          
// generate the spatial prediction
var biomassPrediction = maxNDVI.classify(rfModel); 

// display the result
// define the display parameter
var visualizationBiomass = {
  palette: [
    'ffffff', 'ce7e45', 'df923d', 'f1b555', 'fcd163', 
    '99b718', '74a901', '66a000', '529400', '3e8601', 
    '207401', '056201', '004c00', '023b01', '012e01',
    '011d01', '011301'
  ],
  min: 0.0,
  max: 100};

// display in the map
Map.addLayer(biomassPrediction, visualizationBiomass, 'Predicted Biomass in 2019');


```


## Data sources and AI methods
The sample dataset should be downloaded from the open dataset or get by yourselves. The remote sensing and environmental covariate could be get from Google Earth Engine

## Challenges

1. Find and download the target sample data
2. Write Java Script codes in Google Earth Engine
3. Select the most informative varables to get the ideal result

## What next?

You can try to use the deep learning method in this website, and try to use the method to explore the pattern worldwide.


## Acknowledgments
https://earthengine.google.com/
