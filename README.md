java c
BFC3540 - Sem 1, 2023
Trav’s Final Assessment:
Correlated Portfolio Simulation via Cholesky
Decomposition for Applications in Forecasting
The firm you have just started working in performs scenario analysis and other risk management procedures on various portfolios for clients. Essentially, one of their main analytic tools is to have a portfolio forecasting function that can provide a single, simulated possibility for a portfolio of correlated assets. The main problem right now is that a few of their team just left, and the code isn't working properly.
The spreadsheet you have provided is supposed to take in historical stock price data for several underlying stocks, and via several intermediate steps, produces an output that is a simulated possibility for the relative performance of the stocks in the portfolio over a given time horizon, accounting for each stock’s volatility, expected return, and correlation with the other stocks in the portfolio.
The end goal of this process is to actually run it a large number of times, but as of right now we cannot even get it to work once! Almost like someone has deliberately broken it…
The process is currently broken into 4 main stages, but ideally this would all be contained within one function that could be run numerous times to produce an actual interval forecast. The 4 parts are contained within the 4 numbered sheets in your sample workbook, along with 4 modules containing the broken version of their code base.
Your first task will be to initially focus on these 4 modules and to repair them.
Generally;
1. Filtration - Finding the filtered log returns of a series of stock price histories.
2. Cholesky - finding the Correlation Matrix of the historical log returns of stock price and performing Cholesky Decomposition of the correlation matrix for application in stage 3.
3. Simulation - Using LogNormal Geometric Brownian Motion as a stochastic model for stock price, Simulate a possible future pathway for each asset.
4. Graphs - Transforms future stock price movements into a proportional movement that is more useful for Visualisation and weight-managed portfolio risk analysis.
The sample workbook you have been given also has a few other simple procedures already functioning within it - including the actual visualisations that result from the output of stage 4.
A sample of what a functional version of this workbook would produce is shown here for reference:

1. Optionally Filtered Log Returns and Basic Statistics
Executed on ‘1. FilteredLnRetStats’ using module ‘Filtration’
This function LnReturns(PriceData, Optional Filter) should take as input a series of daily closing stock prices for multiple stocks (PriceData - N prices by M Stocks) and output a series of (N-1) log returns for each of the given stocks. These log returns are determined by the following formula:

Additionally this function should have the optional feature to filter out returns, based on how distributionally unlikely they are. This is achieved by having the user enter a second input argument, Filter, that is the number of standard deviations away from the mean (both of the given stock) a return would need to be to get filtered out. For example;

This means that any return that is greater than Filter standard deviations away from it’s mean is removed and replaced with a “-” blank entry.
This function should be error trapped against non-numeric entry and treat a Filter of 0 to mean that no filtration is needed, which is also the default case.
For this test workbook, in the ‘FILTERED CONTINUOUSLY COMPOUNDED RETURNS’ output space, please use a Filter of 2.5.
2. Correlation Matrix and Cholesky Decomposition
Executed on ‘2. CorrelationCholesky’ using module ‘Cholesky’
A. The first function in this module Matrix_Correl(InputData) should take as input then historical log returns from Part 1. This will be structured as [Number of Returns, Number of Stocks] The function should output the Correlation Matrix of these series of historical log returns:

Currently, this is found via the in-built Excel correlation function, though there has been a suggestion that this could be made more efficient.
B. The second function in this module CHOL(matrix) performs a technique known as Cholesky Decomposition to this correlation matrix, allowing us later to correlate random numbers such that they produce the same correlation matrix as found in Part 2.A.
In essence, we are trying to find 2 triangular matrices of size (MxM) that are the transpose of one another, that multiply together to be our Correlation Matrix:

Given we only need the Upper-Right Cholesky (L T ) for our application, we can rely on the following form.

Which leads to the following general formula:


For automated validation (marking)…
To complete the testing for this sheet, you should output the correlation matrix and its Upper-Right Cholesky Decomposition Matrix for both the unfiltered and filtered historical log returns.
3. Simulations and Correlation Testing
Executed on ‘3. Simulations’ using module ‘Simulation’
This module only contains one function; GBM_sim(S0, mu, sigma, TTM, d_t, Epsilons)
This function performs a single simulation for each stock in the portfolio using LogNormal Geometric Brownian Motion as the process for stock price;

Where;
● S0 - Initial Stock prices for the simulation (Most recent day of historical dataset)
● mu(μ)- Expected Annual log return for each stock (Using Filtered lnRets)
● sigma(σ) - Annual Volatility of log returns for each stock (Using Filtered lnRets)
● TTM - Length in years of simulation
● d_t - Duration of a simulation timestep in years
● Epsilons - Random samples to be used in simulation
The technique utilises random numbers and an observed model for stock price to simulate possible future stock prices. None of these simulations are individually indicative or function as a prediction, but they are plausible.
In order to validate (mark) if this process is working;
A. Run GBM_Sim with the provided uncorrelated random samples
B. Generate Correlated Random Samples by matrix-multiplying L T ᐧϵ
(Feel free to use Excel’s MMULT() here, or code your own!)
C. Test the correlation between your random samples using Matrix_Correl(). Calculate the differences between the historical log returns’ and correlated random samples’ Correlation Matrix;
CorrelL_Epsilons - CorrellnRets_f
Hint: The mean-absolute devia代 写BFC3540 - Sem 1, 2023R
代做程序编程语言nce (calculation given) should be less than 10%
D. Run GBM_Sim with the newly generated correlated random samples
Provided is a set of pre-generated random numbers, as well as a space where you can experiment with newly-generated random numbers.
We are currently generating simulations that are 2 months long for testing purposes, and daily timesteps within the simulation. Ensure to stick to a standardised trading day convention of 252 days per year.
For automated validation (marking)…
Please use the provided set of pre-generated random numbers ‘epsilons’ to complete the testing for this sheet, otherwise your simulations will not match the solution.
Also, when submitting files for validation, ensure the ‘EPSILONS - USING DYNAMIC FUNCTION’ section is cleared of all numerical data.
4. Price Movements and Visualisation
Executed on ‘4. Graphs’ using module ‘Graphs’
This module contains one function; PriceMovement(PriceData)
This function simply scales Stock Price Series such that all prices are a proportion of the initial stock price;

This function serves no statistical purpose, but rather is used aid in the visualisation of the pathways followed by historical stock prices and generated simulations.
Achieved by making all future prices a proportion of initial price displaying the relative movement of each stock price;

So that we can very clearly visualise if the simulation technique is functioning correctly, and that the feature of correlation between stock log returns is necessary, we need to generate three series of movements, and each needs a corresponding plot:
● Historical Stock Price Movement - Discuss if there is correlation visible?
● Uncorrelated Simulation Stock Price Movement - Any undesirable features?
● Correlated Simulation Stock Price Movement - Does it look more realistic?
OTHER SHEETS and MODULES
Provided in the sample workbook is the ‘Unlocked_Presentation’ sheet and ‘StatsFunctions’ module. These may be useful for your modifications and experimentations with the module for use in your presentation.
‘Unlocked_Presentation’ is a sheet where you can edit freely, and has a duplicate of the original stock price data.
‘StatsFunctions’ contains several functions that are not useful for the submission sheets, but might be useful if you are trying to edit or experiment with the function;
● RandNorm() - Produces normally distributed random numbers, epsilons
● RandNorm_Multi() - Produces a 2D set of random numbers by calling RandNorm
● GetArrSize_2D - Returns dimensions of 2D arrays, default is rows, columns by user input
● ArrayMMULT - Matrix Multiplication on 2 array inputs, instead of ranges of cells
Tasks to Complete
SPREADSHEET SUBMISSION: SHEETS 1 to 4
This sheet is where you will simply fix the existing functions detailed above.
■ LnReturns()
■ Matrix_Correl()
■ CHOL()
■ GBM_sim()
■ PriceMovement()
These functions should be tested on the provided sheets and output in the designated and clearly labelled Green Ranges, as well as a few Yellow Ranges where in-built Excel functions are also acceptable.
Please note that these sheets will be automatically marked - this means that you should not be editing the functionality of the model at all, just repairing it.
WEEK 11 QA
Focused on your understanding of the theory behind the model, the existing code and how it works, and the general idea of what progress you are aiming to make.
We will discuss topics the may include but aren’t limited to:
● Demonstrate your understanding of the problem - show me how certain parts work.
● Outline errors in the code and discuss fixes or better ways to write the same code.
● Propose methods to improve the efficiency, efficacy or functionality of the code.
● Discuss ways to improve the usability of the code/model, by making it easier for other people to use. (Documentation / Plans / Commenting / Readability)
● Suggest transformations of the code into something more useful for its current application.
WEEK 12 PRESENTATION (SHEET - UNLOCKED_PRESENTATION)
This is where you are free to edit and transform. the functionality of the model however you would like for the purposes of your presentation.I recommend saving a different version of the file for presentation editing purposes. Here, feel free to use the random samples generated by the RandNorm_Multi() function so that it is truly random - you won’t get the same answer every time you run the function!
Your supervisor has also mentioned that he doesn’t think the output that the model produces is particularly versatile, as it takes too much work to use and can be confusing. Also, when mistakes are made, the model doesn’t always produce useful outputs.
Possible Areas for Improvement/Rework:
Error Trapping on Inputs that will not produce useful outputs, or that would cause the function to fail anyway. This stops us from wasting time computing a useless output.
Improvement of the readability and Meta-Skills of the code. This stuff is nonsense.
Perform. the entire 4-Sheet process as one function;
ForecastPortfolio(PriceData, Optional Filter, Optional Sim_Duration, Optional Price_Mvmnt)
This function should…
○ Only need the Historical Stock Prices as input.
○ Output a complete set of simulated correlated stock prices or movements.
○ Allow the user to specify a filter for returns, similar to LnReturns()
○ Allow the user to specify how many stock trading days to simulate
○ Allow the user to choose between Stock Price or Movement Outputs
Redesign the Volatility Calculation to weight more recent history as more significant.
Implement the Antithetic Variate Technique to improve the precision of the model, and to converge on the expected price faster, if you think it is appropriate.
Implement Moment Matching to ensure our small random sample set isn’t biassed.
Modify ForecastPortfolio() to instead run this process thousands of times…
○ See Step 5. Of the Simulation Slides - we want meaningful forecasts!
○ Using 3D arrays, we can generate thousands of possible portfolio futures.
○ The mean, standard deviation and distribution of this forecast will be very useful.
Visuals for how each particular stock's forecasts are distributed (Histogram).
However…This expanded forecast function will run too slowly to be useful!
○ Perhaps look into ways we could speed up the code. (Investigate ‘Ergodicity’)



         
加QQ：99515681  WX：codinghelp  Email: 99515681@qq.com
