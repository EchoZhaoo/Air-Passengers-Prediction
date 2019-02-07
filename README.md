# Air-Passengers-Prediction
## Motivation
Since last semester I finished the course **STAT 5511 Time Series Analysis**, I have been thinking to find a proper dataset to do some time series analysis in order to practice what I have learned in this course. After exploring several Kaggle datasets and competitions, this dataset came into my mind. Since the number of observations of this dataset is not quite large, which is just 144, and this also suits my another goal. I would like to do some practice on data visualization as well. If the dataset size is too large, like the previous projects I did before (around one million), it will be troublesome for me to find a good way to illustrate the information from the data, sometimes it could be impossible to plot a readable graph.<br /><br />

## Air Passenger Data
The Kaggle dataset I chose is called Air Passenger Data which contains monthly data on the number of airline passengers from 1949–1961. <br />Raw data could be downloaded from [here](https://www.kaggle.com/rakannimer/air-passengers).<br />
There is what the data set looks like:<br /> ![Figure1](https://github.com/EchoZhaoo/Air-Passengers-Prediction/blob/master/images/Figure1.png)<br /><br />
The change of the number of passengers as time passes by is ploted below:<br />![Figure2](https://github.com/EchoZhaoo/Air-Passengers-Prediction/blob/master/images/Figure2.png)<br /><br />

## SARIMAX
SARIMAX(**S**easonal **A**uto-**R**egressive **I**ntegrated **M**oving **A**verage with e**X**ogenous regressors) is the most complicated time series model I have ever used. During the course STAT 5511, what I usually used are **ARMA**, **ARIMA** and **SARIMA** models. It's my first time to try this SARIMA model. Although it's new to me, it's not that hard to understand at all since all the concepts are based on what I have learned in STAT 5511. Let's dig deep into this model together!<br />

Let's first start with **regression**, which is basically like given a set of points, it calculates the line which will best explain the pattern.<br />

Next up let's review **ARMA model** (Auto-Regression Moving Average). The **autoregressive part** means that it's a regression model which predicts upcoming values based upon previous values. It's similar to saying that it will be warm tomorrow because it's been warm the previous three days. This explains why time-series models are so much more complex than standard regression. The data points are not independent from each other. The **moving average part** simply means that the regression error can be modeled as a linear combination of errors.<br />

Then the following one is **ARIMA model** (Auto-Regressive Integrated Moving Average). As you can see, here the additional **I** stands for **Integrated**. This term accounts for differences between previous values. Intuitively, this means that tomorrow is likely to be the same temperature as today because the past week hasn't varied too much.<br />

Finally, we move on to **SARIMAX**. The **S** stands for **Seasonal** -- it helps to model recurring patterns. These seasonal patterns don't necessarily have to occur annually per se actually. For instance, if we were modeling metro traffic in a busy urban area, the patterns would repeat on a weekly scale. And the **X** is for **eXogenous**. This term allows for external variables to be added to the model, such as weather forecasts. <br />

A **SARIMAX model** takes the form of **SARIMAX(p,d,q) x (P,D,Q)m**, where **p** is the **AR** term, **d** is the **I** term, and **q** is for the **MA** part. As for the capital ones, **P**, **D** and **Q** are the same terms but related to the **seasonal component**. The lowercase m is the number of seasonal periods before the pattern repeats. For example, if you are working with monthly data, like in this project, m will be 12. When implemented, these parameters will be integers, and smaller numbers are usually better, which also means less complex in general. For my model, the parameters I have chosen which best fit the model are **SARIMAX(2,1,2)x(0,2,2)12**.<br />

To get the ideal parameters, I performed a grid search to arrive at these terms. The error I sought to minimize is the [**Akaike Information Criterion**](https://en.wikipedia.org/wiki/Akaike_information_criterion)(**AIC**), which is conveniently returned with SARIMAX model fitted using `statsmodels`. The AIC is a measure of how well the model fits the data, while penalizing comlexity. A model that fits the data very well while using lots of features will be assigned a larger AIC score than a model that uses fewer features to achieve the same goodness-of-fit. In the later part of Tableau dashboards, I report Mean Squared Error because that is much more intuitive.<br />

## SARIMAX in Python
Usually for the course assignment, I was asked to accomplish all the homeworks by **R**. But meanwhile, I'm always curious about how it works using Python. Therefore, in this project, I tried **Python** to do all the time series analysis. <br />
The library I used in Python is called `statsmodels`, which contains model classes and functions that are useful for time series analysis. Basic models include univariate autoregressive models(AR), vector autoregressive models(VAR) and univariate autoregressive moving average models(ARMA). Estimation is either done by exact or conditional Maximum Likelihood or conditional least-squares, either using Kalman Filter or direct filters (not very sure about the last two ones)

## Final Result
### Parameter Selection for the SARIMAX Model
The final model I chose is like this:<br /><br />![Figure3](https://github.com/EchoZhaoo/Air-Passengers-Prediction/blob/master/images/Figure3.png)<br /><br />
I fit the model by Maximum Likelihood via Kalman filter. The output of our code suggests that SARIMAX(2,1,2)x(0,2,2)12 yields the lowest AIC value of 715.157. Therefore, this result should be considered to be optimal option out of all the models I have considered.<br />

### Fit a SARIMAX model
Next step I plug the optimal parameter values into a new SARIMAX model. The `summary` attribute that results from the output of SARIMAX returns a significant amount of information. The table of coefficients shown below:<br /><br />![Figure4](https://github.com/EchoZhaoo/Air-Passengers-Prediction/blob/master/images/Figure4.png)<br /><br />
The `coef` column shows the weight of each feature and how each one impacts the time series. The `P>|z|` column informs us of the significance of each feature weight. Here, each weight has a p-value lower or close to 0.05, so it is reasonable to retain all of them in my model.<br />

When fitting SARIMAX models (and any other models for that matter), it is important to run model diagnostics to ensure that none of the assumptions made by the model have been violated. The `plot_diagnostics` object allows us to quickly generate model diagonostics and investigate for any unusual behavior. <br /><br />![Figure5](https://github.com/EchoZhaoo/Air-Passengers-Prediction/blob/master/images/Figure5.png)<br /><br />
The primary concern for now is to ensure that the residuals of our model are uncorrelated and normally distributed with zero mean. If the SARIMAX mdoel does not satisfy these properties, it is a obvious indication that it can be further improved. <br />
In my case, the model diagnostics suggests that the model residuals are normally distributed based on the following:
* In the top right plot, we can see that the red `KDE` line follows closely with the `N(0,1)` line (where `N(0,1)` is the standard notation for a normal distribution with mean 0 and standard deviation of 1). This is a good indication that the residuals are normally distributed.
* The Q-Q plot on the bottom left shows that the ordered distribution of residuals (those blue dots) follows the linear trend of the samples taken from a standard normal distribution with `N(0,1)`. Again, this is a strong indication that the residuals are normally distributed.
* The residuals over time (top left plot) don't display any obvious seasonally and appear to be white noise. This is confirmed by the autocorrelation plot (shown as correlogram in out plot) on the bottom right, which shows that the time series residuals have low correlation with lagged versiosn of itself.<br />

Those observations lead us to conclude that my model produces a satisfactory fit that could help us understand our time series data and forecast future values.<br />

Although we have a pretty goodlooking fit, some parameters of our SARIMAX model could be changed to improve our model fit. For example, our grid search only considered a restricted set of parameter combinations, so we may find better models if we widened the grid search.<br />

### Validating Forecasts
#### One-step Ahead Forecast
We have obtained a model for our time series that can now be used to produce forecasts. We start by comapring predicted values to real values of the time series. We start by comparing predicted values to real values of the time series, which will help us understand the accuracy of our forecasts. The `get_prediction()` and `conf_int()` attributes allow us to obtain the values and associated confidence intervals for forecasts of the time series. The `dynamic = False` argument ensuers that we produce one-step ahead forecasts, meaning that forecasts at each point are generated using the full history up to that point.<br />

I plot the real and forecasted values of airplane passengers time series to assess how well my model did. <br /><br />![Figure6](https://github.com/EchoZhaoo/Air-Passengers-Prediction/blob/master/images/Figure6.png)<br /><br />
Overall, the forecasts align with the true values very well, showing an overall increase trend.<br />

### Dynamic Forecast
What's more, a better representation of our true predictive power can be obtained using dynamic forecasts. In this case, we only use information from the time series up to a certain point, and after that, forecasts are generated using values from previous forecasts time points.<br /><br />![figure7](https://github.com/EchoZhaoo/Air-Passengers-Prediction/blob/master/images/Figure7.png)<br /><br /> we see that the overall forecasts are accurate even when using dynamic forecasts. All forecasted values (red line) match pretty closely to the ground truth (blue line), and are well within the confidence intervals of our forecast.<br />

Both the one-step ahead and dynamic forecasts confirm that this time series model is valid. However, much of the interest around time series forecasting is the ability to forecast future values way ahead in time.

### 2-year Forecast
In the final step of the project, I leverage the SARIMAX time series model to forecasts future values. The `get_forecast()` attribute of the time series object can compute forecasted values for a specified number of steps ahead.<br /><br />![Figure8](https://github.com/EchoZhaoo/Air-Passengers-Prediction/blob/master/images/Figure8.png)<br /><br />
Both the forecasts and associated confidence interval that we have generated can now be used to further understand the time series and foresee what to expect. My forecasts show that the time series is expected to continue increasing at a steady pace.<br />

As we forecast further out into the future, it is natural for us to become less confident in our values. This is reflected by the confidence intervls generated by our model, which grow larger as we move further out into the future.<br />



## Tableau 
Since Tableau released version 10.2 last year which included integration with Python. I would really like to try this function. It is not super straightforward how to use it though, so I though it's a good opportunity to learn and practise this in this project. ARIMA models are not built into Tableau (Tableau's Forecast module uses exponential smoothing), and in this particular case I really need to use the higher predictive capability of the ARIMA algorithm, so TabPy seemed to be my only option. After I did some searching job, it seems not to be a hard job to use TabPy, let's try together!<br />

At the very first start, one should use `pip install tabpy-server` at the terminal (for MacOS) to install TabPy. After the installation,

 

## Conclusion
In this project, I made extensive use of the `pandas` and `statsmodels` libraries and showed how to run model diagnostics, as well as how to produce forecasts of the Air Passenger time series. <br />
Here are a few other things you could try:
* Change the start data of your dynamic forecasts to see how this affects the overall quality of your forecasts.
* Try more combinations of parameters to see if you can improve the goodness-of-fit of your model. 
* Select a different metric to select the best model. For instance, I used the AIC measure to find the best model, but you could seek to optimize the out-of-sample mean square error instead.<br /><br />

All the code could be obtained from [Jupyter Notebook](https://github.com/EchoZhaoo/Air-Passengers-Prediction/blob/master/SARIMAX.ipynb).







