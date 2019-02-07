# Air-Passengers-Prediction
## Motivation
Since last semester I finished the course STAT 5511 Time Series Analysis, I have been thinking to find a proper dataset to do some time series analysis in order to practice what I have learned in this course. After exploring several Kaggle datasets and competitions, this dataset came into my mind. Since the number of observations of this dataset is not quite large, which is just 144, and this also suits my another goal. I would like to do some practice on data visualization as well. If the dataset size is too large, like the previous projects I did before (around one million), it will be troublesome for me to find a good way to illustrate the information from the data, sometimes it could be impossible to plot a readable graph.<br /><br />

## Air Passenger Data
The Kaggle dataset I chose is called Air Passenger Data which contains monthly data on the number of airline passengers from 1949–1961. <br />Raw data could be downloaded from [here](https://www.kaggle.com/rakannimer/air-passengers).  Let’s take a look at what’s going on! <br />
There is what the data set looks like:<br /> ![Figure1](https://github.com/EchoZhaoo/Air-Passengers-Prediction/blob/master/images/Figure1.png)<br /><br />
The change of the number of passengers as time passes by is ploted below:<br />![Figure2](https://github.com/EchoZhaoo/Air-Passengers-Prediction/blob/master/images/Figure2.png)<br /><br />

## SARIMAX
SARIMAX(**S**easonal **A**uto-**R**egressive **I**ntegrated **M**oving **A**verage with e**X**ogenous regressors) is the most complicated time series model I have ever used. During the course STAT 5511, what I usually used are ARMA, ARIMA and SARIMA models. It's my first time to try this SARIMA model. Although it's new to me, but it's not hard to understand at all since all the concepts are based on what I have learned in STAT 5511. Let's dig deep into this model together!<br />

Let's first start with regression, which is basically that given a set of points, it calculates the line which will best explain the pattern.<br />

Next up let's review ARMA model (Auto-Regression Moving Average). The autoregressive part means that it's a regression model which predicts upcoming values based upon previous values. It's similar to saying that it will be warm tomorrow because it's been warm the previous three days. This explains why time-series models are so much more complex than standard regression. The data points are not independent from each other. The moving average part simply means that the regression error can be modeled as a linear combination of errors.<br />

Then the following one is ARIMA model (Auto-Regressive Integrated Moving Average). As you can see, here the additional I stands for Integrated. This term accounts for differences between previous values. Intuitively, this means that tomorrow is likely to be the same temperature as today because the past week hasn't varied too much.<br />

Finally, we move on to SARIMAX. The S stands for Seasonal -- it helps to model recurring patterns. These seasonal patterns don't necessarily have to occur annually per se actually. For instance, if we were modeling metro traffic in a busy urban area, the patterns would repeat on a weekly scale. And the X is for eXogenous. This term allows for external variables to be added to the model, such as weather forecasts. <br />

A SARIMAX model takes the form of SARIMAX(p,d,q) x (P,D,Q)m, where p is the AR term, d is the I term, and q is for the MA part. As for the capital ones, P,D and Q are the same terms but related to the seasonal component. The lowercase m is the number of seasonal periods before the pattern repeats. For example, if you are working with monthly data, like in this project, m will be 12. When implemented, these parameters will be integers, and smaller numbers are usually better, which also means less complex in general. For my model, the parameters I have chosen which best fit the model are SARIMAX(2,1,2)x(0,2,2)12.<br />![Figure3](https://github.com/EchoZhaoo/Air-Passengers-Prediction/blob/master/images/Figure3.png)<br /><br />

To get the ideal parameters, I performed a grid search to arrive at these terms. The error I sought to minimize is the [Akaike Information Criterion](https://en.wikipedia.org/wiki/Akaike_information_criterion)(AIC). The AIC is a measure of how well the model fits the data, while penalizing comlexity. In the later part of Tableau dashboards, I report Mean Squared Error because that is much more intuitive.<br />

## SARIMAX in Python
Usually for the course assignment, I was asked to accomplish all the homeworks by R. I'm always curious about how it works using Python. Therefore, in this project, I tried Python to do all the time series analysis. 

## Final Result

## Tableau 

All the code could be obtained from [Jupyter Notebook](https://github.com/EchoZhaoo/Air-Passengers-Prediction/blob/master/SARIMAX.ipynb). 
