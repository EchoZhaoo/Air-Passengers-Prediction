# Air-Passengers-Prediction
## Motivation
Since last semester I finished the course STAT 5511 Time Series Analysis, I have been thinking to find a proper dataset to do some time series analysis in order to practice what I have learned in this course. After exploring several Kaggle datasets and competitions, this dataset came into my mind. Since the number of observations of this dataset is not quite large, which is just 144, and this also suits my another goal. I would like to do some practice on data visualization as well. If the dataset size is too large, like the previous projects I did before (around one million), it will be troublesome for me to find a good way to illustrate the information from the data, sometimes it will be impossible to plot a readable graph.<br /><br />

## Air Passenger Data
There we go the dataset: the Kaggle dataset I chose is called Air Passenger Data which contains monthly data on the number of airline passengers from 1949–1961. Raw data could be downloaded from [here](https://www.kaggle.com/rakannimer/air-passengers). Let’s take a look at what’s going on! <br />
There is what the data set looks like:<br /> ![Figure1](https://github.com/EchoZhaoo/Air-Passengers-Prediction/blob/master/images/Figure1.png)<br /><br />
The change of the number of passengers as time passes by is ploted below:<br />![Figure2](https://github.com/EchoZhaoo/Air-Passengers-Prediction/blob/master/images/Figure2.png)<br /><br />

## SARIMAX
SARIMAX(**S**easonal **A**uto-**R**egressive **I**ntegrated **Moving **A**verage with e**X**ogenous regressors) is the most complicated time series model I have ever used. During the course STAT 5511, what I usually used are ARMA, ARIMA and SARIMA models. It's my first time to try this SARIMA model. Although it's new to me, but it's not hard to understand at all since all the concepts are based on what I have learned in STAT 5511. Let's dig deep into this model together!<br />

Let's first start with regression, which is basically that given a set of points, it calculates the line which will best explain the pattern.<br />

Next up let's review ARMA model (Auto-Regression Moving Average). The autoregressive part means that it's a regression model which predicts upcoming values based upon previous values. It's similar to saying that it will be warm tomorrow because it's been warm the previous three days. This explains why time-series models are so much more complex than standard regression. The data points are not independent from each other. The moving average part simply means that the regression error can be modeled as a linear combination of errors.





## statsmodels.tsa.statespace.sarimax

## Best Result 

## Tableau 

All the code could be obtained from [Jupyter Notebook](https://github.com/EchoZhaoo/Air-Passengers-Prediction/blob/master/SARIMAX.ipynb). 
