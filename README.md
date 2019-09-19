Predict Future Sales
======================
This is my solution to the problem posted as part of the  [kaggle competition on predicting futures sales.](https://www.kaggle.com/c/competitive-data-science-predict-future-sales)

 ![image](https://github.com/babinu-uthup-4JESUS/Kaggle-Predict-Future-Sales/blob/master/rel_images/kaggle_comp.png)

## Table of content

- [Overview](#overview)
- [Data](#data)
    - [Data Description](#data-description)
    - [File Descriptions](#file-descriptions)
    - [Data Fields](#data-fields)    
- [Approach](#approach)
    - [Data Validation](#data-validation)
    - [Building simple models](#building-simple-models)
- [More Complex models](#more-complex-models)
    - [Gradient boosting using tensorflow](#gradient-boosting-using-tensorflow)
    - [Gradient boosting using xgboost](#gradient-boosting-using-xgboost)
    - [DNN using tensorflow](#dnn-using-tensorflow)    
    - [Addition of new variables to the xgboost method](#addition-of-new-variables-to-the-xgboost-method)
- [License](#license)
- [Links](#links)

## Overview

We have the following problem description from it's [corresponding kaggle competition link](https://www.kaggle.com/c/competitive-data-science-predict-future-sales/overview/description) :
>In this competition you will work with a challenging time-series dataset consisting of daily sales data, kindly provided by one of the largest Russian software firms - 1C Company. We are asking you to predict total sales for every product and store in the next month. By solving this competition you will be able to apply and enhance your data science skills.

## Data

All the information pasted in this section has been obtained from the [webpage showing data related information of the corresponding kaggle competition.](https://www.kaggle.com/c/competitive-data-science-predict-future-sales/data)


### Data Description
 
You are provided with daily historical sales data. The task is to forecast the total amount of products sold in every shop for the test set. Note that the list of shops and products slightly changes every month. Creating a robust model that can handle such situations is part of the challenge.

### File Descriptions

- [sales_train.csv](https://github.com/babinu-uthup-4JESUS/Kaggle-Predict-Future-Sales/blob/master/input/sales_train.csv) - the training set. Daily historical data from January 2013 to October 2015.

- [test.csv](https://github.com/babinu-uthup-4JESUS/Kaggle-Predict-Future-Sales/blob/master/input/test.csv) - the test set. You need to forecast the sales for these shops and products for November 2015.

- [sample_submission.csv](https://github.com/babinu-uthup-4JESUS/Kaggle-Predict-Future-Sales/blob/master/input/sample_submission.csv) - a sample submission file in the correct format.

- [items.csv](https://github.com/babinu-uthup-4JESUS/Kaggle-Predict-Future-Sales/blob/master/input/items.csv) - supplemental information about the items/products.

- [item_categories.csv](https://github.com/babinu-uthup-4JESUS/Kaggle-Predict-Future-Sales/blob/master/input/item_categories.csv) - supplemental information about the items categories.

- [shops.csv](https://github.com/babinu-uthup-4JESUS/Kaggle-Predict-Future-Sales/blob/master/input/shops.csv) - supplemental information about the shops.

### Data fields

- ID - an Id that represents a (Shop, Item) tuple within the test set
- shop_id - unique identifier of a shop
- item_id - unique identifier of a product
- item_category_id - unique identifier of item category
- item_cnt_day - number of products sold. You are predicting a monthly amount of this measure
- item_price - current price of an item
- date - date in format dd/mm/yyyy
- date_block_num - a consecutive month number, used for convenience. January 2013 is 0, February 2013 is 1,..., October 2015 is 33
- item_name - name of item
- shop_name - name of shop
- item_category_name - name of item category

## Approach

The approach is to have a thorough look at the data, validate the same and try out relevant modeling techiniques. 

### Data Validation

This has been carried out in [data_sanity_check.ipynb](https://github.com/babinu-uthup-4JESUS/Kaggle-Predict-Future-Sales/blob/master/data_sanity_check/data_sanity_check.ipynb). To summarize, we have done the following :

- Data sanity test - to make sure that we do not have unexpected input.
- Conversion to monthly data - to facilitate easier data processing in the future.
- Evaluation of a simple past average model - to serve as a benchmark for later models.

### Building simple models

This has been carried out in [data_analysis_and_models.ipynb](https://github.com/babinu-uthup-4JESUS/Kaggle-Predict-Future-Sales/blob/master/data_analysis_and_models/data_analysis_and_models.ipynb). I am summarizing the models briefly below :

- Model 1 - take the value of the previous month for the corresponding shop and item if present, 1 if absent (Score : 2.37) .
- Model 2 - take the value of the most recent month in the past if available, 1 if not (Score : 2.42).
- Model 3 - bayesian model which takes 1 as the prior and historical value for the most recent month as the posterior, and combines the same using recency weighting (Score : 2.37).
- Model 4 - take the average of the previous month for the corresponding shop and item if present, the average of the item across different shops for the previous month if absent. If both of these are unavaible, value is set to 1 (Score : 2.71).

Thus, though Model 3 was marginally better than the rest, Model 1 score in terms of simplicity.

## More Complex models

In this section, we go ahead with using more complex model architectures such as gradient boosting, random forest and so on.

### Gradient boosting using tensorflow
This has been done in [gradient_boosting_tensorflow.ipynb](https://github.com/babinu-uthup-4JESUS/Kaggle-Predict-Future-Sales/blob/master/gradient_boosting_tensorflow.ipynb). It does not look to offer much benefit as the best score which we were able to obtain was 2.61.

### Gradient boosting using xgboost
This has been done in [gradient_boosting_xgboost.ipynb](https://github.com/babinu-uthup-4JESUS/Kaggle-Predict-Future-Sales/blob/master/gradient_boosting_xgboost.ipynb). It gave us a reasonably good improvement as we got a score of 2.27 on the validation set.

### DNN using tensorflow
This has been done in [dnn_tensorflow.ipynb](https://github.com/babinu-uthup-4JESUS/Kaggle-Predict-Future-Sales/blob/master/dnn_tensorflow.ipynb). It did not help much and gave a mediocre score of 2.60 (NOTE: Since this method involved more processing, this was as part of [kaggle kernels](https://www.kaggle.com/babinu/predict-sales-tensorflow?scriptVersionId=20825161).

### Addition of new variables to the xgboost method
This has been done in [gradient_boosting_using_xgboost_new_variables.ipynb](https://github.com/babinu-uthup-4JESUS/Kaggle-Predict-Future-Sales/blob/master/more_complex_models/gradient_boosting_using_xgboost_new_variables.ipynb). This gave us some improvement to 2.15 and was run as part of  [kaggle kernels](https://www.kaggle.com/babinu/gradient-boosting-using-xgboost-new-variables?scriptVersionId=20826601) to improve the running speed.

### LSTM using keras
This has been done in [lstm_keras.ipynb](https://github.com/babinu-uthup-4JESUS/Kaggle-Predict-Future-Sales/blob/master/more_complex_models/lstm_keras.ipynb). It did not help much and gave a mediocre score of 2.27 (NOTE: Since this method involved more processing, this was as part of [kaggle kernels](https://www.kaggle.com/babinu/lstm-keras?scriptVersionId=20831177).

## License

The Aimeos TYPO3 extension is licensed under the terms of the GPL Open Source
license and is available for free.

## Links

* [Web site](https://aimeos.org/integrations/typo3-shop-extension/)
* [Documentation](https://aimeos.org/docs/TYPO3)
* [Forum](https://aimeos.org/help/typo3-extension-f16/)
* [Issue tracker](https://github.com/aimeos/aimeos-typo3/issues)
* [Source code](https://github.com/aimeos/aimeos-typo3)
