# utl-altair-slc-supplementing-rapid-miner-with-genetic-modeling-using-r-and-python
altair slc supplementing rapid miner with genetic modeling using r and python
    %let pgm=utl-altair-slc-supplementing-rapid-miner-with-genetic-modeling-using-r-and-python;

    %stop_submission;

    altair slc supplementing rapid miner with genetic modeling using r and python

    Too long to post on a list, see github
    https://github.com/rogerjdeangelis/utl-altair-slc-supplementing-rapid-miner-with-genetic-modeling-using-r-and-python

    graphics
    https://github.com/rogerjdeangelis/utl-altair-slc-supplementing-rapid-miner-with-genetic-modeling-using-r-ga-package/blob/main/plot1_timeseries_merge.pdf

    Rapid Miner Post
    https://community.altair.com/discussion/44504/rapid-miner?tab=all

    TWO SOLUTIONS
       1. r package GA
       2. python

    Note: This is a very simple example, close to use fitting AR(2) model.

    But GA(genetic algorithm) becomes more useful when: (none demonstrated here)

     1 Constraints: Force parameters to sum to 1 (for integrated processes)
     2 Non-linear objectives: Minimize VaR instead of MSE
     3 Multiple objectives: Balance forecast accuracy with parameter parsimony
     4 Complex structures: AR with neural network components

    Problem (what you can expect from this example of genetic algorithm iterative processing)

      Stock Trading

        Strong lag(1) price momentum: 0.61(lag1) coefficient means 61% of today's return persists tomorrow
        Moderate lag(2) price momentum 0.32(lag2)  32% of day 1 return persists
        Mean reversion: -0.04 at lag 3 suggests slight reversal after 3 days

        Trading implication: Momentum strategy might work for 1-2 days

      Weather

       If its sunny today there is a 61% chance it will be sunny tomorrow
       If its sunny today there is a 31% chance it will be sunny the day after tomorrow
       Possible cloudy in two days


    INPUT AR(2) DATA

      # AR(2) parameters: y_t = 0.6*y_{t-1} + 0.3*y_{t-2} + noise
      ar_params = [0.6, 0.3]

    I generated the input using an AR(2) model so we expect
      a. strong lag 1 corrrelation
      b. moderate lag2 correlation
      c. weak lag 3-5  correlations

    Deepseek
    https://chat.deepseek.com/a/chat/s/86b89724-f096-4c1b-ae98-9fa31545cdde

    /*                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */

    proc datasets lib=workx kill;
    run;

    /*--- I generated the input using an AR(2 ) modek so we expect       ---*/
    /*---   a. strong lag 1 corrrelation                                 ---*/
    /*----  b. moderate lag2 correlation                                 ---*/
    /*----  c. weak lag 3-5  correlations                                ---*/
    /*----                                                               ---*/
    /*----  # AR(2) parameters: y_t = 0.6*y_{t-1} + 0.3*y_{t-2} + noise  ---*/
    /*----  ar_params = [0.6, 0.3]                                       ---*/

    /*----  Training ~160                                                ---*/
    /*----  Holdout(Test) ~40                                            ---*/


    options validvarname=v7;
    data workx.ar2model;
    informat date date10.;
    input date value @@;
    cards4;
    01JAN2020  0.21780  10FEB2020 -0.94601  21MAR2020  0.62989  30APR2020 -1.07256  09JUN2020 -1.00198
    02JAN2020  0.36241  11FEB2020 -1.37940  22MAR2020  0.49093  01MAY2020 -1.55714  10JUN2020 -1.18005
    03JAN2020  0.48492  12FEB2020 -1.79558  23MAR2020 -0.11364  02MAY2020 -1.25696  11JUN2020 -0.71362
    04JAN2020  0.34661  13FEB2020 -1.27476  24MAR2020  0.38510  03MAY2020 -1.43545  12JUN2020 -0.06598
    05JAN2020  1.10920  14FEB2020 -1.70923  25MAR2020  0.08840  04MAY2020 -1.54519  13JUN2020 -0.75002
    06JAN2020  0.72217  15FEB2020 -0.68591  26MAR2020  0.07719  05MAY2020 -2.37009  14JUN2020 -0.24248
    07JAN2020  1.77528  16FEB2020 -1.14004  27MAR2020  0.53950  06MAY2020 -2.49798  15JUN2020 -0.32804
    08JAN2020  1.25046  17FEB2020 -0.56197  28MAR2020  0.75775  07MAY2020 -2.12006  16JUN2020  0.17821
    09JAN2020  1.93529  18FEB2020 -0.51823  29MAR2020  1.31256  08MAY2020 -1.73762  17JUN2020 -0.10638
    10JAN2020  2.67964  19FEB2020 -0.87145  30MAR2020  0.77677  09MAY2020 -1.92503  18JUN2020  0.40795
    11JAN2020  1.49394  20FEB2020  0.10952  31MAR2020  1.18500  10MAY2020 -1.67627  19JUN2020 -0.65967
    12JAN2020  1.56086  21FEB2020  0.12573  01APR2020  1.63959  11MAY2020 -1.02183  20JUN2020  0.57131
    13JAN2020  1.31804  22FEB2020  0.15317  02APR2020  0.78386  12MAY2020 -0.39605  21JUN2020  0.57727
    14JAN2020  1.57706  23FEB2020  0.26790  03APR2020  0.53180  13MAY2020 -1.09274  22JUN2020  0.44237
    15JAN2020  1.19952  24FEB2020  0.54633  04APR2020 -0.01163  14MAY2020 -0.83312  23JUN2020 -0.28590
    16JAN2020 -0.13540  25FEB2020  0.45309  05APR2020 -0.57705  15MAY2020 -0.22694  24JUN2020  0.28268
    17JAN2020 -0.94162  26FEB2020 -1.06079  06APR2020 -0.30973  16MAY2020 -0.62096  25JUN2020  0.32543
    18JAN2020  0.05447  27FEB2020 -0.35811  07APR2020 -0.03235  17MAY2020 -0.46690  26JUN2020  0.27688
    19JAN2020 -0.40312  28FEB2020 -0.71672  08APR2020  0.48816  18MAY2020 -0.50948  27JUN2020  0.33949
    20JAN2020 -1.11619  29FEB2020 -0.44485  09APR2020  0.80556  19MAY2020 -0.88960  28JUN2020 -0.00530
    21JAN2020 -0.87661  01MAR2020 -0.19101  10APR2020  0.12818  20MAY2020 -0.90894  29JUN2020  0.28307
    22JAN2020 -0.25348  02MAR2020  0.45181  11APR2020  1.24282  21MAY2020 -0.82697  30JUN2020  0.31558
    23JAN2020  0.53252  03MAR2020 -0.14987  12APR2020  0.45076  22MAY2020 -0.97580  01JUL2020  0.13464
    24JAN2020  0.02823  04MAR2020  0.69689  13APR2020  0.69606  23MAY2020 -0.27688  02JUL2020 -0.49266
    25JAN2020  0.04806  05MAR2020  0.54110  14APR2020  0.34173  24MAY2020 -0.69936  03JUL2020  0.09517
    26JAN2020 -0.84427  06MAR2020  1.05298  15APR2020  0.35268  25MAY2020 -0.71926  04JUL2020  0.18640
    27JAN2020 -0.26210  07MAR2020  1.25448  16APR2020  0.40823  26MAY2020 -0.29294  05JUL2020 -0.27776
    28JAN2020 -0.73054  08MAR2020  1.42902  17APR2020  0.41032  27MAY2020 -0.91972  06JUL2020 -0.90803
    29JAN2020 -0.28923  09MAR2020  0.71220  18APR2020  0.35611  28MAY2020 -0.66006  07JUL2020 -0.52567
    30JAN2020 -0.04028  10MAR2020  0.81093  19APR2020  0.39080  29MAY2020 -1.44773  08JUL2020 -0.76035
    31JAN2020  0.40662  11MAR2020  1.01198  20APR2020  0.09860  30MAY2020 -0.48307  09JUL2020 -0.48761
    01FEB2020 -0.07258  12MAR2020  0.37371  21APR2020 -0.07571  31MAY2020 -0.86098  10JUL2020 -1.16767
    02FEB2020  0.33092  13MAR2020  0.25640  22APR2020 -0.84640  01JUN2020 -0.89544  11JUL2020 -1.32647
    03FEB2020 -0.68173  14MAR2020  0.55645  23APR2020 -0.72172  02JUN2020 -1.41468  12JUL2020 -0.60330
    04FEB2020 -0.70199  15MAR2020  0.79488  24APR2020 -0.94327  03JUN2020 -1.12132  13JUL2020 -0.55803
    05FEB2020 -1.05117  16MAR2020  0.87575  25APR2020  0.56847  04JUN2020 -1.49734  14JUL2020 -0.22256
    06FEB2020 -2.04840  17MAR2020  0.32102  26APR2020 -0.62296  05JUN2020 -1.50155  15JUL2020  0.60667
    07FEB2020 -1.52633  18MAR2020 -0.09455  27APR2020 -0.13461  06JUN2020 -0.70629  16JUL2020  0.36164
    08FEB2020 -1.42732  19MAR2020  0.79593  28APR2020 -1.01447  07JUN2020 -0.96200  17JUL2020 -0.60148
    09FEB2020 -1.49482  20MAR2020  0.57815  29APR2020 -1.38428  08JUN2020 -1.32498  18JUL2020 -0.08551
    ;;;;
    run;quit;

    /*--- put days in order ---*/
    proc sort data=workx.ar2model;
    by date;
    run;quit;

    /**************************************************************************************************************************/
    /*  WORKX.AR2MODEL total obs=200                                                                                          */
    /*                                                                                                                        */
    /*  Obs     date      value                                                                                               */
    /*                                                                                                                        */
    /*    1    01JAN2020  0.21780                                                                                             */
    /*    2    02JAN2020  0.36241                                                                                             */
    /*    3    03JAN2020  0.48492                                                                                             */
    /*    4    04JAN2020  0.34661                                                                                             */
    /*    5    05JAN2020  1.10920                                                                                             */
    /*  ...                                                                                                                   */
    /*  196    14JUL2020 -0.22256                                                                                             */
    /*  197    15JUL2020  0.60667                                                                                             */
    /*  198    16JUL2020  0.36164                                                                                             */
    /*  199    17JUL2020 -0.60148                                                                                             */
    /*  200    18JUL2020 -0.08551                                                                                             */
    /**************************************************************************************************************************/*/

    /*                           _
    / |  _ __   _ __   __ _  ___| | ____ _  __ _  ___    __ _  __ _
    | | | `__| | `_ \ / _` |/ __| |/ / _` |/ _` |/ _ \  / _` |/ _` |
    | | | |    | |_) | (_| | (__|   < (_| | (_| |  __/ | (_| | (_| |
    |_| |_|    | .__/ \__,_|\___|_|\_\__,_|\__, |\___|  \__, |\__,_|
               |_|                         |___/        |___/
     _ __  _ __ ___   ___ ___  ___ ___
    | `_ \| `__/ _ \ / __/ _ \/ __/ __|
    | |_) | | | (_) | (_|  __/\__ \__ \
    | .__/|_|  \___/ \___\___||___/___/
    |_|
    */

    %utlfkil(d:/pdf/plot1_timeseries.pdf);
    %utlfkil(d:/pdf/plot2_convergence.pdf);
    %utlfkil(d:/pdf/plot3_predictions.pdf);
    %utlfkil(d:/pdf/plot4_parameters.pdf);
    %utlfkil(d:/pdf/plot5_residual.pdf);

    options set=RHOME "C:\Progra~1\R\R-4.5.2\bin\r";
    proc r;
    export data=workx.ar2model r=df;
    submit;
    library(ggplot2)
    library(dplyr)
    library(tidyr)
    library(GA)
    library(gridExtra)
    library(reshape2)
    # Create lagged features
    create_lagged_features <- function(data, n_lags = 5) {
      df_lagged <- data.frame(target = data)

      for (i in 1:n_lags) {
        df_lagged[[paste0("lag_", i)]] <- dplyr::lag(data, i)
      }

      # Remove rows with NA
      df_lagged <- na.omit(df_lagged)
      return(df_lagged)
    }

    # Prepare data for modeling
    n_lags <- 5
    df_lagged <- create_lagged_features(df$value, n_lags)
    cat("\nLagged features dimensions:", dim(df_lagged), "\n")

    # Split into training and test sets
    train_size <- floor(0.8 * nrow(df_lagged))
    train_data <- df_lagged[1:train_size, ]
    test_data <- df_lagged[(train_size + 1):nrow(df_lagged), ]

    # Prepare matrices for modeling
    X_train <- as.matrix(train_data[, -1])
    y_train <- train_data$target
    X_test <- as.matrix(test_data[, -1])
    y_test <- test_data$target

    # Define fitness function for GA
    fitness_function <- function(params, X, y) {
      # Make predictions
      y_pred <- X %*% params

      # Calculate MSE
      mse <- mean((y - y_pred)^2)

      # Return negative MSE (for maximization)
      return(-mse)
    }

    # Run Genetic Algorithm
    cat("\n", rep("=", 50), "\n")
    cat("Running Genetic Algorithm for AR Parameter Optimization\n")
    cat(rep("=", 50), "\n")

    # Create empty vectors to store history
    best_fitness_history <- numeric(100)
    avg_fitness_history <- numeric(100)

    # Define monitoring function that stores history
    monitor <- function(obj) {
      if (obj@iter <= 100) {
        best_fitness_history[obj@iter] <<- -max(obj@fitness)
        avg_fitness_history[obj@iter] <<- -mean(obj@fitness)
      }
      if (obj@iter %% 10 == 0) {
        best_mse <- -max(obj@fitness)
        avg_mse <- -mean(obj@fitness)
        cat(sprintf("Generation %d: Best MSE = %.4f, Avg MSE = %.4f\n",
                    obj@iter, best_mse, avg_mse))
      }
    }

    # Run GA
    ga_result <- ga(
      type = "real-valued",
      fitness = function(x) fitness_function(x, X_train, y_train),
      lower = rep(-1, n_lags),
      upper = rep(1, n_lags),
      popSize = 100,
      maxiter = 100,
      run = 50,
      pmutation = 0.15,
      pcrossover = 0.8,
      elitism = 10,
      monitor = monitor,
      seed = 123
    )

    # Get best solution
    best_params <- ga_result@solution[1, ]
    cat("\nBest AR parameters found:\n")
    print(best_params)

    # Evaluate on test set
    y_pred_test <- X_test %*% best_params
    test_mse <- mean((y_test - y_pred_test)^2)
    cat(sprintf("\nTest MSE: %.4f\n", test_mse))
    cat(sprintf("Test RMSE: %.4f\n", sqrt(test_mse)))

    # Compare with standard linear regression
    lm_model <- lm(target ~ . - 1, data = train_data)  # -1 to remove intercept
    lm_coef <- coef(lm_model)
    lm_pred <- predict(lm_model, newdata = test_data)
    lm_mse <- mean((y_test - lm_pred)^2)
    cat(sprintf("\nLinear Regression Test MSE: %.4f\n", lm_mse))
    cat("Linear Regression coefficients:\n")
    print(lm_coef)

    # Create all four plots separately

    # Plot 1: Original time series
    plot1 <- ggplot(df[1:200, ], aes(x = date, y = value)) +
      geom_line(color = "blue", size = 0.7) +
      labs(title = "Original AR Time Series",
           x = "Date", y = "Value") +
      theme_minimal() +
      theme(plot.title = element_text(hjust = 0.5)) +
      geom_smooth(method = "loess", se = FALSE, color = "red", linetype = "dashed")

    #print(plot1)

    # Plot 2: GA Convergence
    convergence_df <- data.frame(
      Generation = 1:100,
      Best_MSE = best_fitness_history[1:100],
      Average_MSE = avg_fitness_history[1:100]
    )

    plot2 <- ggplot(convergence_df) +
      geom_line(aes(x = Generation, y = Best_MSE, color = "Best MSE"), size = 1) +
      geom_line(aes(x = Generation, y = Average_MSE, color = "Average MSE"), size = 1, linetype = "dashed") +
      scale_color_manual(values = c("Best MSE" = "red", "Average MSE" = "blue")) +
      labs(title = "GA Convergence",
           x = "Generation", y = "MSE",
           color = "Legend") +
      theme_minimal() +
      theme(plot.title = element_text(hjust = 0.5)) +
      scale_y_log10()

    #print(plot2)

    # Plot 3: Test Set Predictions
    test_indices <- 1:length(y_test)
    prediction_df <- data.frame(
      Index = rep(test_indices, 3),
      Value = c(y_test, as.vector(y_pred_test), as.vector(lm_pred)),
      Type = factor(rep(c("Actual", "GA Predicted", "OLS Predicted"),
                        each = length(y_test)))
    )

    plot3 <- ggplot(prediction_df, aes(x = Index, y = Value, color = Type)) +
      geom_line(size = 0.8) +
      labs(title = "Test Set Predictions",
           x = "Time Index", y = "Value") +
      theme_minimal() +
      theme(plot.title = element_text(hjust = 0.5)) +
      scale_color_manual(values = c("Actual" = "blue",
                                     "GA Predicted" = "red",
                                     "OLS Predicted" = "green"))

    #print(plot3)

    # Plot 4: Parameter Comparison
    params_df <- data.frame(
      Lag = factor(rep(1:n_lags, 2), levels = 1:n_lags),
      Method = rep(c("GA", "OLS"), each = n_lags),
      Coefficient = c(best_params, lm_coef)
    )

    plot4 <- ggplot(params_df, aes(x = Lag, y = Coefficient, fill = Method)) +
      geom_bar(stat = "identity", position = position_dodge(width = 0.9), width = 0.7) +
      labs(title = "AR Parameter Comparison",
           x = "Lag", y = "Coefficient Value") +
      theme_minimal() +
      theme(plot.title = element_text(hjust = 0.5)) +
      scale_fill_manual(values = c("GA" = "red", "OLS" = "green")) +
      geom_hline(yintercept = 0, linetype = "dashed", alpha = 0.5) +
      geom_text(aes(label = sprintf("%.3f", Coefficient), y = Coefficient + 0.05 * sign(Coefficient)),
                position = position_dodge(width = 0.9), size = 3)

    #print(plot4)

    # Arrange all plots in a 2x2 grid
    # grid.arrange(plot1, plot2, plot3, plot4, ncol = 2, nrow = 2)


    # Additional diagnostic plots

    # Residual analysis
    residuals_ga <- y_test - y_pred_test
    residuals_lm <- y_test - lm_pred

    residual_df <- data.frame(
      Index = rep(test_indices, 2),
      Residuals = c(as.vector(residuals_ga), as.vector(residuals_lm)),
      Model = factor(rep(c("GA", "OLS"), each = length(y_test)))
    )

    # Residual plot
    plot5 <- ggplot(residual_df, aes(x = Index, y = Residuals, color = Model)) +
      geom_point(alpha = 0.6) +
      geom_hline(yintercept = 0, linetype = "dashed") +
      labs(title = "Residuals Comparison",
           x = "Time Index", y = "Residuals") +
      theme_minimal() +
      scale_color_manual(values = c("GA" = "red", "OLS" = "green"))

    #print(plot5)

    ggsave("d:/pdf/plot1_timeseries.pdf", plot1, width = 8, height = 6)
    ggsave("d:/pdf/plot2_convergence.pdf", plot2, width = 8, height = 6)
    ggsave("d:/pdf/plot3_predictions.pdf", plot3, width = 8, height = 6)
    ggsave("d:/pdf/plot4_parameters.pdf", plot4, width = 8, height = 6)
    ggsave("d:/pdf/plot5_residual.pdf", plot5, width = 8, height = 6)

    pdf("d:/pdf/plot6_qq.pdf");
    # Q-Q plot for normality check
    par(mfrow = c(1, 2))
    qqnorm(residuals_ga, main = "Q-Q Plot: GA Residuals")
    qqline(residuals_ga, col = "red")
    qqnorm(residuals_lm, main = "Q-Q Plot: OLS Residuals")
    qqline(residuals_lm, col = "green")
    par(mfrow = c(1, 1))
    dev.off()

    # Print summary statistics
    cat("\n", rep("=", 50), "\n")
    cat("Summary Statistics\n")
    cat(rep("=", 50), "\n")
    cat(sprintf("Training data points: %d\n", nrow(train_data)))
    cat(sprintf("Test data points: %d\n", nrow(test_data)))
    cat(sprintf("Number of lags used: %d\n", n_lags))
    cat(sprintf("GA final best fitness: %.4f\n", max(ga_result@fitness)))
    cat(sprintf("GA final best MSE: %.4f\n", -max(ga_result@fitness)))

    # Check stationarity condition
    cat("\nStationarity Check:\n")
    cat("Sum of absolute GA parameters:", sum(abs(best_params)), "\n")
    if (sum(abs(best_params)) < 1) {
      cat("? GA model satisfies stationarity condition\n")
    } else {
      cat("? GA model may not be stationary\n")
    }

    # Calculate additional metrics
    ss_res <- sum((y_test - y_pred_test)^2)
    ss_tot <- sum((y_test - mean(y_test))^2)
    r_squared <- 1 - ss_res/ss_tot
    cat(sprintf("\nR-squared: %.4f\n", r_squared))

    # AIC and BIC approximations
    n <- length(y_test)
    k <- n_lags
    aic <- n * log(test_mse) + 2 * k
    bic <- n * log(test_mse) + k * log(n)
    cat(sprintf("AIC: %.4f\n", aic))
    cat(sprintf("BIC: %.4f\n", bic))

    # Create a comprehensive report
    cat("\n", rep("=", 50), "\n")
    cat("MODEL INTERPRETATION\n")
    cat(rep("=", 50), "\n")

    cat("\n1. Parameter Significance:\n")
    for (i in 1:n_lags) {
      sig_level <- ifelse(abs(best_params[i]) > 0.1, "Strong",
                          ifelse(abs(best_params[i]) > 0.05, "Moderate", "Weak"))
      cat(sprintf("   Lag %d: %.4f (%s influence)\n", i, best_params[i], sig_level))
    }

    cat("\n2. Model Characteristics:\n")
    if (sum(best_params) > 0) {
      cat("   Positive persistence: Shocks persist in the same direction\n")
    } else {
      cat("   Negative persistence: Shocks tend to reverse\n")
    }

    if (best_params[1] > 0.5) {
      cat("   Strong short-term momentum detected\n")
    }

    cat("\n3. Forecast Implications:\n")
    cat(sprintf("   - {%.1f}%% of today's value carries to tomorrow\n", best_params[1] * 100))
    cat(sprintf("   - Half-life of shocks: approximately %.1f periods\n",
                log(0.5)/log(abs(best_params[1]))))
    endsubmit;
    import data=workx.prediction_df  r=prediction_df  ;
    import data=workx.convergence_df r=convergence_df ;
    import data=workx.residual_df    r=residual_df    ;
    run;

    /*           _               _
      ___  _   _| |_ _ __  _   _| |_
     / _ \| | | | __| `_ \| | | | __|
    | (_) | |_| | |_| |_) | |_| | |_
     \___/ \__,_|\__| .__/ \__,_|\__|
                    |_|
    */

    /**************************************************************************************************************************/
    /* R Listing                                                             |   SLC (OUTPUT TABLES)                          */
    /*                                                                       |                                                */
    /* Altair SLC                                                            |   WORKX.PREDICTION_DF total obs=117            */
    /*                                                                       |                                                */
    /* Lagged features dimensions: 195 6                                     |    Index      Value     Type                   */
    /*  = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = =    |                                                */
    /*                                                                       |       1     -1.18005    Actual                 */
    /* Running Genetic Algorithm for AR Parameter Optimization               |       2     -0.71362    Actual                 */
    /* = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = =     |       3     -0.06598    Actual                 */
    /* Generation 10: Best MSE = 0.2506, Avg MSE = 0.3774                    |       4     -0.75002    Actual                 */
    /* Generation 20: Best MSE = 0.2486, Avg MSE = 0.3701                    |       5     -0.24248    Actual                 */
    /* Generation 30: Best MSE = 0.2483, Avg MSE = 0.3296                    |                                                */
    /* Generation 40: Best MSE = 0.2480, Avg MSE = 0.3439                    |      35     -0.47160    OLS Predicted          */
    /* Generation 50: Best MSE = 0.2478, Avg MSE = 0.3090                    |      36     -0.27066    OLS Predicted          */
    /* Generation 60: Best MSE = 0.2478, Avg MSE = 0.3307                    |      37      0.32429    OLS Predicted          */
    /* Generation 70: Best MSE = 0.2478, Avg MSE = 0.3131                    |      38      0.43279    OLS Predicted          */
    /* Generation 80: Best MSE = 0.2478, Avg MSE = 0.3678                    |      39     -0.27062    OLS Predicted          */
    /* Generation 90: Best MSE = 0.2478, Avg MSE = 0.3171                    |                                                */
    /* Generation 100: Best MSE = 0.2478, Avg MSE = 0.3169                   |                                                */
    /*                                                                       |   WORKX.RESIDUAL_DF total obs=78               */
    /* Best AR parameters found:                                             |                                                */
    /*           x1           x2           x3           x4           x5      |    Index    Residuals    Model                 */
    /*  0.609203854  0.322736231 -0.022362468 -0.027251434  0.002368279      |                                                */
    /*                                                                       |       1      -0.17922     GA                   */
    /* Test MSE: 0.2158                                                      |       2       0.27447     GA                   */
    /* Test RMSE: 0.4645                                                     |       3       0.69337     GA                   */
    /* Linear Regression Test MSE: 0.2167                                    |       4      -0.53007     GA                   */
    /*                                                                       |       5       0.18999     GA                   */
    /* Linear Regression coefficients:                                       |   ...                                          */
    /*       lag_1       lag_2       lag_3       lag_4       lag_5           |      35       0.24904     OLS                  */
    /*  0.61702285  0.31795292 -0.02649836 -0.02875581  0.00859929           |      36       0.87733     OLS                  */
    /*  = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = =    |      37       0.03735     OLS                  */
    /*                                                                       |      38      -1.03427     OLS                  */
    /* Summary Statistics                                                    |      39       0.18511     OLS                  */
    /* = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = =     |                                                */
    /* Training data points: 156                                             |                                                */
    /* Test data points: 39                                                  |   WORKX.CONVERGENCE_DF total obs=100           */
    /* Number of lags used: 5                                                |                                                */
    /* GA final best fitness: -0.2478                                        |                           Average_             */
    /* GA final best MSE: 0.2478                                             |    Generation Best_MSE       MSE               */
    /* Stationarity Check:                                                   |                                                */
    /* Sum of absolute GA parameters: 0.9839223                              |         1     0.32139     2.44350              */
    /* ? GA model satisfies stationarity condition                           |         2     0.30211     1.73870              */
    /* R-squared: 0.2019                                                     |         3     0.27981     1.09967              */
    /* AIC: -49.8113                                                         |         4     0.27737     0.80143              */
    /* BIC: -41.4935                                                         |         5     0.27737     0.61453              */
    /*  = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = =    |        96     0.24780     0.30219              */
    /*                                                                       |        97     0.24780     0.29424              */
    /* MODEL INTERPRETATION                                                  |        98     0.24780     0.36490              */
    /* = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = =     |        99     0.24780     0.30299              */
    /* 1. Parameter Significance:                                            |       100     0.24780     0.31690              */
    /*    Lag 1: 0.6092 (Strong influence)                                   |                                                */
    /*    Lag 2: 0.3227 (Strong influence)                                   |                                                */
    /*    Lag 3: -0.0224 (Weak influence)                                    |                                                */
    /*    Lag 4: -0.0273 (Weak influence)                                    |                                                */
    /*    Lag 5: 0.0024 (Weak influence)                                     |                                                */
    /*                                                                       |                                                */
    /* 2. Model Characteristics:                                             |                                                */
    /*    Positive persistence: Shocks persist in the same direction         |                                                */
    /*    Strong short-term momentum detected                                |                                                */
    /*                                                                       |                                                */
    /* 3. Forecast Implications:                                             |                                                */
    /*    - {60.9}% of today's value carries to tomorrow                     |                                                */
    /*    - Half-life of shocks: approximately 1.4 periods                   |                                                */
    /**************************************************************************************************************************/

    /*
    | | ___   __ _
    | |/ _ \ / _` |
    | | (_) | (_| |
    |_|\___/ \__, |
             |___/
    */

    1                                          Altair SLC       12:04 Wednesday, March 11, 2026

    NOTE: Copyright 2002-2025 World Programming, an Altair Company
    NOTE: Altair SLC 2026 (05.26.01.00.000758)
          Licensed to Roger DeAngelis
    NOTE: This session is executing on the X64_WIN11PRO platform and is running in 64 bit mode

    NOTE: AUTOEXEC processing beginning; file is C:\wpsoto\autoexec.sas
    NOTE: AUTOEXEC source line
    1       +  ï»¿ods _all_ close;
               ^
    ERROR: Expected a statement keyword : found "?"
    NOTE: Library workx assigned as follows:
          Engine:        SAS7BDAT
          Physical Name: d:\wpswrkx

    NOTE: Library slchelp assigned as follows:
          Engine:        WPD
          Physical Name: C:\Progra~1\Altair\SLC\2026\sashelp

    NOTE: Library worksas assigned as follows:
          Engine:        SAS7BDAT
          Physical Name: d:\worksas

    NOTE: Library workwpd assigned as follows:
          Engine:        WPD
          Physical Name: d:\workwpd


    LOG:  12:04:26
    NOTE: 1 record was written to file PRINT

    NOTE: The data step took :
          real time : 0.015
          cpu time  : 0.000


    NOTE: AUTOEXEC processing completed

    1          options validvarname=v7;
    2         data workx.ar2model;
    3         informat date date10.;
    4         input date value @@;
    5         cards4;

    NOTE: A new line was read when INPUT statement read past the end of a line
    NOTE: Data set "WORKX.ar2model" has 200 observation(s) and 2 variable(s)
    NOTE: The data step took :
          real time : 0.015
          cpu time  : 0.000


    6         01JAN2020  0.21780  10FEB2020 -0.94601  21MAR2020  0.62989  30APR2020 -1.07256  09JUN2020 -1.00198
    7         02JAN2020  0.36241  11FEB2020 -1.37940  22MAR2020  0.49093  01MAY2020 -1.55714  10JUN2020 -1.18005
    8         03JAN2020  0.48492  12FEB2020 -1.79558  23MAR2020 -0.11364  02MAY2020 -1.25696  11JUN2020 -0.71362
    9         04JAN2020  0.34661  13FEB2020 -1.27476  24MAR2020  0.38510  03MAY2020 -1.43545  12JUN2020 -0.06598
    10        05JAN2020  1.10920  14FEB2020 -1.70923  25MAR2020  0.08840  04MAY2020 -1.54519  13JUN2020 -0.75002
    11        06JAN2020  0.72217  15FEB2020 -0.68591  26MAR2020  0.07719  05MAY2020 -2.37009  14JUN2020 -0.24248
    12        07JAN2020  1.77528  16FEB2020 -1.14004  27MAR2020  0.53950  06MAY2020 -2.49798  15JUN2020 -0.32804
    13        08JAN2020  1.25046  17FEB2020 -0.56197  28MAR2020  0.75775  07MAY2020 -2.12006  16JUN2020  0.17821
    14        09JAN2020  1.93529  18FEB2020 -0.51823  29MAR2020  1.31256  08MAY2020 -1.73762  17JUN2020 -0.10638
    15        10JAN2020  2.67964  19FEB2020 -0.87145  30MAR2020  0.77677  09MAY2020 -1.92503  18JUN2020  0.40795
    16        11JAN2020  1.49394  20FEB2020  0.10952  31MAR2020  1.18500  10MAY2020 -1.67627  19JUN2020 -0.65967
    17        12JAN2020  1.56086  21FEB2020  0.12573  01APR2020  1.63959  11MAY2020 -1.02183  20JUN2020  0.57131
    18        13JAN2020  1.31804  22FEB2020  0.15317  02APR2020  0.78386  12MAY2020 -0.39605  21JUN2020  0.57727
    19        14JAN2020  1.57706  23FEB2020  0.26790  03APR2020  0.53180  13MAY2020 -1.09274  22JUN2020  0.44237
    20        15JAN2020  1.19952  24FEB2020  0.54633  04APR2020 -0.01163  14MAY2020 -0.83312  23JUN2020 -0.28590
    21        16JAN2020 -0.13540  25FEB2020  0.45309  05APR2020 -0.57705  15MAY2020 -0.22694  24JUN2020  0.28268
    22        17JAN2020 -0.94162  26FEB2020 -1.06079  06APR2020 -0.30973  16MAY2020 -0.62096  25JUN2020  0.32543
    23        18JAN2020  0.05447  27FEB2020 -0.35811  07APR2020 -0.03235  17MAY2020 -0.46690  26JUN2020  0.27688
    24        19JAN2020 -0.40312  28FEB2020 -0.71672  08APR2020  0.48816  18MAY2020 -0.50948  27JUN2020  0.33949
    25        20JAN2020 -1.11619  29FEB2020 -0.44485  09APR2020  0.80556  19MAY2020 -0.88960  28JUN2020 -0.00530
    26        21JAN2020 -0.87661  01MAR2020 -0.19101  10APR2020  0.12818  20MAY2020 -0.90894  29JUN2020  0.28307
    27        22JAN2020 -0.25348  02MAR2020  0.45181  11APR2020  1.24282  21MAY2020 -0.82697  30JUN2020  0.31558
    28        23JAN2020  0.53252  03MAR2020 -0.14987  12APR2020  0.45076  22MAY2020 -0.97580  01JUL2020  0.13464
    29        24JAN2020  0.02823  04MAR2020  0.69689  13APR2020  0.69606  23MAY2020 -0.27688  02JUL2020 -0.49266
    30        25JAN2020  0.04806  05MAR2020  0.54110  14APR2020  0.34173  24MAY2020 -0.69936  03JUL2020  0.09517
    31        26JAN2020 -0.84427  06MAR2020  1.05298  15APR2020  0.35268  25MAY2020 -0.71926  04JUL2020  0.18640
    32        27JAN2020 -0.26210  07MAR2020  1.25448  16APR2020  0.40823  26MAY2020 -0.29294  05JUL2020 -0.27776
    33        28JAN2020 -0.73054  08MAR2020  1.42902  17APR2020  0.41032  27MAY2020 -0.91972  06JUL2020 -0.90803
    34        29JAN2020 -0.28923  09MAR2020  0.71220  18APR2020  0.35611  28MAY2020 -0.66006  07JUL2020 -0.52567
    35        30JAN2020 -0.04028  10MAR2020  0.81093  19APR2020  0.39080  29MAY2020 -1.44773  08JUL2020 -0.76035
    36        31JAN2020  0.40662  11MAR2020  1.01198  20APR2020  0.09860  30MAY2020 -0.48307  09JUL2020 -0.48761
    37        01FEB2020 -0.07258  12MAR2020  0.37371  21APR2020 -0.07571  31MAY2020 -0.86098  10JUL2020 -1.16767
    38        02FEB2020  0.33092  13MAR2020  0.25640  22APR2020 -0.84640  01JUN2020 -0.89544  11JUL2020 -1.32647
    39        03FEB2020 -0.68173  14MAR2020  0.55645  23APR2020 -0.72172  02JUN2020 -1.41468  12JUL2020 -0.60330
    40        04FEB2020 -0.70199  15MAR2020  0.79488  24APR2020 -0.94327  03JUN2020 -1.12132  13JUL2020 -0.55803
    41        05FEB2020 -1.05117  16MAR2020  0.87575  25APR2020  0.56847  04JUN2020 -1.49734  14JUL2020 -0.22256
    42        06FEB2020 -2.04840  17MAR2020  0.32102  26APR2020 -0.62296  05JUN2020 -1.50155  15JUL2020  0.60667
    43        07FEB2020 -1.52633  18MAR2020 -0.09455  27APR2020 -0.13461  06JUN2020 -0.70629  16JUL2020  0.36164
    44        08FEB2020 -1.42732  19MAR2020  0.79593  28APR2020 -1.01447  07JUN2020 -0.96200  17JUL2020 -0.60148
    45        09FEB2020 -1.49482  20MAR2020  0.57815  29APR2020 -1.38428  08JUN2020 -1.32498  18JUL2020 -0.08551
    46        ;;;;
    47        run;quit;
    48
    49        /*--- put days in order ---*/
    50        proc sort data=workx.ar2model;
    51        by date;
    52        run;quit;
    NOTE: Automatically set SORTSIZE to 10240MiB
    NOTE: 200 observations were read from "WORKX.ar2model"
    NOTE: Data set "WORKX.ar2model" has 200 observation(s) and 2 variable(s)
    NOTE: Procedure sort step took :
          real time : 0.029
          cpu time  : 0.015



    55        %utlfkil(d:/pdf/plot1_timeseries.pdf);
    56        %utlfkil(d:/pdf/plot2_convergence.pdf);
    57        %utlfkil(d:/pdf/plot3_predictions.pdf);
    58        %utlfkil(d:/pdf/plot4_parameters.pdf);
    59        %utlfkil(d:/pdf/plot5_residual.pdf);
    60
    61        options set=RHOME "C:\Progra~1\R\R-4.5.2\bin\r";
    62        proc r;
    NOTE: Using R version 4.5.2 (2025-10-31 ucrt) from C:\Program Files\R\R-4.5.2
    63        export data=workx.ar2model r=df;
    NOTE: Creating R data frame 'df' from data set 'WORKX.ar2model'

    64        submit;
    65        library(ggplot2)
    66        library(dplyr)
    67        library(tidyr)
    68        library(GA)
    69        library(gridExtra)
    70        library(reshape2)
    71        # Create lagged features
    72        create_lagged_features <- function(data, n_lags = 5) {
    73          df_lagged <- data.frame(target = data)
    74
    75          for (i in 1:n_lags) {
    76            df_lagged[[paste0("lag_", i)]] <- dplyr::lag(data, i)
    77          }
    78
    79          # Remove rows with NA
    80          df_lagged <- na.omit(df_lagged)
    81          return(df_lagged)
    82        }
    83
    84        # Prepare data for modeling
    85        n_lags <- 5
    86        df_lagged <- create_lagged_features(df$value, n_lags)
    87        cat("\nLagged features dimensions:", dim(df_lagged), "\n")
    88
    89        # Split into training and test sets
    90        train_size <- floor(0.8 * nrow(df_lagged))
    91        train_data <- df_lagged[1:train_size, ]
    92        test_data <- df_lagged[(train_size + 1):nrow(df_lagged), ]
    93
    94        # Prepare matrices for modeling
    95        X_train <- as.matrix(train_data[, -1])
    96        y_train <- train_data$target
    97        X_test <- as.matrix(test_data[, -1])
    98        y_test <- test_data$target
    99
    100       # Define fitness function for GA
    101       fitness_function <- function(params, X, y) {
    102         # Make predictions
    103         y_pred <- X %*% params
    104
    105         # Calculate MSE
    106         mse <- mean((y - y_pred)^2)
    107
    108         # Return negative MSE (for maximization)
    109         return(-mse)
    110       }
    111
    112       # Run Genetic Algorithm
    113       cat("\n", rep("=", 50), "\n")
    114       cat("Running Genetic Algorithm for AR Parameter Optimization\n")
    115       cat(rep("=", 50), "\n")
    116
    117       # Create empty vectors to store history
    118       best_fitness_history <- numeric(100)
    119       avg_fitness_history <- numeric(100)
    120
    121       # Define monitoring function that stores history
    122       monitor <- function(obj) {
    123         if (obj@iter <= 100) {
    124           best_fitness_history[obj@iter] <<- -max(obj@fitness)
    125           avg_fitness_history[obj@iter] <<- -mean(obj@fitness)
    126         }
    127         if (obj@iter %% 10 == 0) {
    128           best_mse <- -max(obj@fitness)
    129           avg_mse <- -mean(obj@fitness)
    130           cat(sprintf("Generation %d: Best MSE = %.4f, Avg MSE = %.4f\n",
    131                       obj@iter, best_mse, avg_mse))
    132         }
    133       }
    134
    135       # Run GA
    136       ga_result <- ga(
    137         type = "real-valued",
    138         fitness = function(x) fitness_function(x, X_train, y_train),
    139         lower = rep(-1, n_lags),
    140         upper = rep(1, n_lags),
    141         popSize = 100,
    142         maxiter = 100,
    143         run = 50,
    144         pmutation = 0.15,
    145         pcrossover = 0.8,
    146         elitism = 10,
    147         monitor = monitor,
    148         seed = 123
    149       )
    150
    151       # Get best solution
    152       best_params <- ga_result@solution[1, ]
    153       cat("\nBest AR parameters found:\n")
    154       print(best_params)
    155
    156       # Evaluate on test set
    157       y_pred_test <- X_test %*% best_params
    158       test_mse <- mean((y_test - y_pred_test)^2)
    159       cat(sprintf("\nTest MSE: %.4f\n", test_mse))
    160       cat(sprintf("Test RMSE: %.4f\n", sqrt(test_mse)))
    161
    162       # Compare with standard linear regression
    163       lm_model <- lm(target ~ . - 1, data = train_data)  # -1 to remove intercept
    164       lm_coef <- coef(lm_model)
    165       lm_pred <- predict(lm_model, newdata = test_data)
    166       lm_mse <- mean((y_test - lm_pred)^2)
    167       cat(sprintf("\nLinear Regression Test MSE: %.4f\n", lm_mse))
    168       cat("Linear Regression coefficients:\n")
    169       print(lm_coef)
    170
    171       # Create all four plots separately
    172
    173       # Plot 1: Original time series
    174       plot1 <- ggplot(df[1:200, ], aes(x = date, y = value)) +
    175         geom_line(color = "blue", size = 0.7) +
    176         labs(title = "Original AR Time Series",
    177              x = "Date", y = "Value") +
    178         theme_minimal() +
    179         theme(plot.title = element_text(hjust = 0.5)) +
    180         geom_smooth(method = "loess", se = FALSE, color = "red", linetype = "dashed")
    181
    182       #print(plot1)
    183
    184       # Plot 2: GA Convergence
    185       convergence_df <- data.frame(
    186         Generation = 1:100,
    187         Best_MSE = best_fitness_history[1:100],
    188         Average_MSE = avg_fitness_history[1:100]
    189       )
    190
    191       plot2 <- ggplot(convergence_df) +
    192         geom_line(aes(x = Generation, y = Best_MSE, color = "Best MSE"), size = 1) +
    193         geom_line(aes(x = Generation, y = Average_MSE, color = "Average MSE"), size = 1, linetype = "dashed") +
    194         scale_color_manual(values = c("Best MSE" = "red", "Average MSE" = "blue")) +
    195         labs(title = "GA Convergence",
    196              x = "Generation", y = "MSE",
    197              color = "Legend") +
    198         theme_minimal() +
    199         theme(plot.title = element_text(hjust = 0.5)) +
    200         scale_y_log10()
    201
    202       #print(plot2)
    203
    204       # Plot 3: Test Set Predictions
    205       test_indices <- 1:length(y_test)
    206       prediction_df <- data.frame(
    207         Index = rep(test_indices, 3),
    208         Value = c(y_test, as.vector(y_pred_test), as.vector(lm_pred)),
    209         Type = factor(rep(c("Actual", "GA Predicted", "OLS Predicted"),
    210                           each = length(y_test)))
    211       )
    212
    213       plot3 <- ggplot(prediction_df, aes(x = Index, y = Value, color = Type)) +
    214         geom_line(size = 0.8) +
    215         labs(title = "Test Set Predictions",
    216              x = "Time Index", y = "Value") +
    217         theme_minimal() +
    218         theme(plot.title = element_text(hjust = 0.5)) +
    219         scale_color_manual(values = c("Actual" = "blue",
    220                                        "GA Predicted" = "red",
    221                                        "OLS Predicted" = "green"))
    222
    223       #print(plot3)
    224
    225       # Plot 4: Parameter Comparison
    226       params_df <- data.frame(
    227         Lag = factor(rep(1:n_lags, 2), levels = 1:n_lags),
    228         Method = rep(c("GA", "OLS"), each = n_lags),
    229         Coefficient = c(best_params, lm_coef)
    230       )
    231
    232       plot4 <- ggplot(params_df, aes(x = Lag, y = Coefficient, fill = Method)) +
    233         geom_bar(stat = "identity", position = position_dodge(width = 0.9), width = 0.7) +
    234         labs(title = "AR Parameter Comparison",
    235              x = "Lag", y = "Coefficient Value") +
    236         theme_minimal() +
    237         theme(plot.title = element_text(hjust = 0.5)) +
    238         scale_fill_manual(values = c("GA" = "red", "OLS" = "green")) +
    239         geom_hline(yintercept = 0, linetype = "dashed", alpha = 0.5) +
    240         geom_text(aes(label = sprintf("%.3f", Coefficient), y = Coefficient + 0.05 * sign(Coefficient)),
    241                   position = position_dodge(width = 0.9), size = 3)
    242
    243       #print(plot4)
    244
    245       # Arrange all plots in a 2x2 grid
    246       # grid.arrange(plot1, plot2, plot3, plot4, ncol = 2, nrow = 2)
    247
    248
    249       # Additional diagnostic plots
    250
    251       # Residual analysis
    252       residuals_ga <- y_test - y_pred_test
    253       residuals_lm <- y_test - lm_pred
    254
    255       residual_df <- data.frame(
    256         Index = rep(test_indices, 2),
    257         Residuals = c(as.vector(residuals_ga), as.vector(residuals_lm)),
    258         Model = factor(rep(c("GA", "OLS"), each = length(y_test)))
    259       )
    260
    261       # Residual plot
    262       plot5 <- ggplot(residual_df, aes(x = Index, y = Residuals, color = Model)) +
    263         geom_point(alpha = 0.6) +
    264         geom_hline(yintercept = 0, linetype = "dashed") +
    265         labs(title = "Residuals Comparison",
    266              x = "Time Index", y = "Residuals") +
    267         theme_minimal() +
    268         scale_color_manual(values = c("GA" = "red", "OLS" = "green"))
    269
    270       #print(plot5)
    271
    272       ggsave("d:/pdf/plot1_timeseries.pdf", plot1, width = 8, height = 6)
    273       ggsave("d:/pdf/plot2_convergence.pdf", plot2, width = 8, height = 6)
    274       ggsave("d:/pdf/plot3_predictions.pdf", plot3, width = 8, height = 6)
    275       ggsave("d:/pdf/plot4_parameters.pdf", plot4, width = 8, height = 6)
    276       ggsave("d:/pdf/plot5_residual.pdf", plot5, width = 8, height = 6)
    277
    278       pdf("d:/pdf/plot6_qq.pdf");
    279       # Q-Q plot for normality check
    280       par(mfrow = c(1, 2))
    281       qqnorm(residuals_ga, main = "Q-Q Plot: GA Residuals")
    282       qqline(residuals_ga, col = "red")
    283       qqnorm(residuals_lm, main = "Q-Q Plot: OLS Residuals")
    284       qqline(residuals_lm, col = "green")
    285       par(mfrow = c(1, 1))
    286       dev.off()
    287
    288       # Print summary statistics
    289       cat("\n", rep("=", 50), "\n")
    290       cat("Summary Statistics\n")
    291       cat(rep("=", 50), "\n")
    292       cat(sprintf("Training data points: %d\n", nrow(train_data)))
    293       cat(sprintf("Test data points: %d\n", nrow(test_data)))
    294       cat(sprintf("Number of lags used: %d\n", n_lags))
    295       cat(sprintf("GA final best fitness: %.4f\n", max(ga_result@fitness)))
    296       cat(sprintf("GA final best MSE: %.4f\n", -max(ga_result@fitness)))
    297
    298       # Check stationarity condition
    299       cat("\nStationarity Check:\n")
    300       cat("Sum of absolute GA parameters:", sum(abs(best_params)), "\n")
    301       if (sum(abs(best_params)) < 1) {
    302         cat("? GA model satisfies stationarity condition\n")
    303       } else {
    304         cat("? GA model may not be stationary\n")
    305       }
    306
    307       # Calculate additional metrics
    308       ss_res <- sum((y_test - y_pred_test)^2)
    309       ss_tot <- sum((y_test - mean(y_test))^2)
    310       r_squared <- 1 - ss_res/ss_tot
    311       cat(sprintf("\nR-squared: %.4f\n", r_squared))
    312
    313       # AIC and BIC approximations
    314       n <- length(y_test)
    315       k <- n_lags
    316       aic <- n * log(test_mse) + 2 * k
    317       bic <- n * log(test_mse) + k * log(n)
    318       cat(sprintf("AIC: %.4f\n", aic))
    319       cat(sprintf("BIC: %.4f\n", bic))
    320
    321       # Create a comprehensive report
    322       cat("\n", rep("=", 50), "\n")
    323       cat("MODEL INTERPRETATION\n")
    324       cat(rep("=", 50), "\n")
    325
    326       cat("\n1. Parameter Significance:\n")
    327       for (i in 1:n_lags) {
    328         sig_level <- ifelse(abs(best_params[i]) > 0.1, "Strong",
    329                             ifelse(abs(best_params[i]) > 0.05, "Moderate", "Weak"))
    330         cat(sprintf("   Lag %d: %.4f (%s influence)\n", i, best_params[i], sig_level))
    331       }
    332
    333       cat("\n2. Model Characteristics:\n")
    334       if (sum(best_params) > 0) {
    335         cat("   Positive persistence: Shocks persist in the same direction\n")
    336       } else {
    337         cat("   Negative persistence: Shocks tend to reverse\n")
    338       }
    339
    340       if (best_params[1] > 0.5) {
    341         cat("   Strong short-term momentum detected\n")
    342       }
    343
    344       cat("\n3. Forecast Implications:\n")
    345       cat(sprintf("   - {%.1f}%% of today's value carries to tomorrow\n", best_params[1] * 100))
    346       cat(sprintf("   - Half-life of shocks: approximately %.1f periods\n",
    347                   log(0.5)/log(abs(best_params[1]))))
    348       endsubmit;

    NOTE: Submitting statements to R:

    > library(ggplot2)
    > library(dplyr)
    Attaching package: 'dplyr'
    The following objects are masked from 'package:stats':
        filter, lag
    The following objects are masked from 'package:base':
        intersect, setdiff, setequal, union
    > library(tidyr)
    > library(GA)
    Loading required package: foreach
    Loading required package: iterators
    Package 'GA' version 3.2.5
    Type 'citation("GA")' for citing this R package in publications.
    Attaching package: 'GA'
    The following object is masked from 'package:utils':
        de
    > library(gridExtra)
    Attaching package: 'gridExtra'
    The following object is masked from 'package:dplyr':
        combine
    > library(reshape2)
    Attaching package: 'reshape2'
    The following object is masked from 'package:tidyr':
        smiths
    > # Create lagged features
    > create_lagged_features <- function(data, n_lags = 5) {
    +   df_lagged <- data.frame(target = data)
    +
    +   for (i in 1:n_lags) {
    +     df_lagged[[paste0("lag_", i)]] <- dplyr::lag(data, i)
    +   }
    +
    +   # Remove rows with NA
    +   df_lagged <- na.omit(df_lagged)
    +   return(df_lagged)
    + }
    >
    > # Prepare data for modeling
    > n_lags <- 5
    > df_lagged <- create_lagged_features(df$value, n_lags)
    > cat("\nLagged features dimensions:", dim(df_lagged), "\n")
    >
    > # Split into training and test sets
    > train_size <- floor(0.8 * nrow(df_lagged))
    > train_data <- df_lagged[1:train_size, ]
    > test_data <- df_lagged[(train_size + 1):nrow(df_lagged), ]
    >
    > # Prepare matrices for modeling
    > X_train <- as.matrix(train_data[, -1])
    > y_train <- train_data$target
    > X_test <- as.matrix(test_data[, -1])
    > y_test <- test_data$target
    >
    > # Define fitness function for GA
    > fitness_function <- function(params, X, y) {
    +   # Make predictions
    +   y_pred <- X %*% params
    +
    +   # Calculate MSE
    +   mse <- mean((y - y_pred)^2)
    +
    +   # Return negative MSE (for maximization)
    +   return(-mse)
    + }
    >
    > # Run Genetic Algorithm
    > cat("\n", rep("=", 50), "\n")
    > cat("Running Genetic Algorithm for AR Parameter Optimization\n")
    > cat(rep("=", 50), "\n")
    >
    > # Create empty vectors to store history
    > best_fitness_history <- numeric(100)
    > avg_fitness_history <- numeric(100)
    >
    > # Define monitoring function that stores history
    > monitor <- function(obj) {
    +   if (obj@iter <= 100) {
    +     best_fitness_history[obj@iter] <<- -max(obj@fitness)
    +     avg_fitness_history[obj@iter] <<- -mean(obj@fitness)
    +   }
    +   if (obj@iter %% 10 == 0) {
    +     best_mse <- -max(obj@fitness)
    +     avg_mse <- -mean(obj@fitness)
    +     cat(sprintf("Generation %d: Best MSE = %.4f, Avg MSE = %.4f\n",
    +                 obj@iter, best_mse, avg_mse))
    +   }
    + }
    >
    > # Run GA

    2                                                                                                                         Altair SLC

    > ga_result <- ga(
    +   type = "real-valued",
    +   fitness = function(x) fitness_function(x, X_train, y_train),
    +   lower = rep(-1, n_lags),
    +   upper = rep(1, n_lags),
    +   popSize = 100,
    +   maxiter = 100,
    +   run = 50,
    +   pmutation = 0.15,
    +   pcrossover = 0.8,
    +   elitism = 10,
    +   monitor = monitor,
    +   seed = 123
    + )
    >
    > # Get best solution
    > best_params <- ga_result@solution[1, ]
    > cat("\nBest AR parameters found:\n")
    > print(best_params)
    >
    > # Evaluate on test set
    > y_pred_test <- X_test %*% best_params
    > test_mse <- mean((y_test - y_pred_test)^2)
    > cat(sprintf("\nTest MSE: %.4f\n", test_mse))
    > cat(sprintf("Test RMSE: %.4f\n", sqrt(test_mse)))
    >
    > # Compare with standard linear regression
    > lm_model <- lm(target ~ . - 1, data = train_data)  # -1 to remove intercept
    > lm_coef <- coef(lm_model)
    > lm_pred <- predict(lm_model, newdata = test_data)
    > lm_mse <- mean((y_test - lm_pred)^2)
    > cat(sprintf("\nLinear Regression Test MSE: %.4f\n", lm_mse))
    > cat("Linear Regression coefficients:\n")
    > print(lm_coef)
    >
    > # Create all four plots separately
    >
    > # Plot 1: Original time series
    > plot1 <- ggplot(df[1:200, ], aes(x = date, y = value)) +
    +   geom_line(color = "blue", size = 0.7) +
    +   labs(title = "Original AR Time Series",
    +        x = "Date", y = "Value") +
    +   theme_minimal() +
    +   theme(plot.title = element_text(hjust = 0.5)) +
    +   geom_smooth(method = "loess", se = FALSE, color = "red", linetype = "dashed")
    Warning message:
    Using `size` aesthetic for lines was deprecated in ggplot2 3.4.0.
    i Please use `linewidth` instead.
    >
    > #print(plot1)
    >
    > # Plot 2: GA Convergence
    > convergence_df <- data.frame(
    +   Generation = 1:100,
    +   Best_MSE = best_fitness_history[1:100],
    +   Average_MSE = avg_fitness_history[1:100]
    + )
    >
    > plot2 <- ggplot(convergence_df) +
    +   geom_line(aes(x = Generation, y = Best_MSE, color = "Best MSE"), size = 1) +
    +   geom_line(aes(x = Generation, y = Average_MSE, color = "Average MSE"), size = 1, linetype = "dashed") +
    +   scale_color_manual(values = c("Best MSE" = "red", "Average MSE" = "blue")) +
    +   labs(title = "GA Convergence",
    +        x = "Generation", y = "MSE",
    +        color = "Legend") +
    +   theme_minimal() +
    +   theme(plot.title = element_text(hjust = 0.5)) +
    +   scale_y_log10()
    >
    > #print(plot2)
    >
    > # Plot 3: Test Set Predictions
    > test_indices <- 1:length(y_test)
    > prediction_df <- data.frame(
    +   Index = rep(test_indices, 3),
    +   Value = c(y_test, as.vector(y_pred_test), as.vector(lm_pred)),
    +   Type = factor(rep(c("Actual", "GA Predicted", "OLS Predicted"),
    +                     each = length(y_test)))
    + )
    >
    > plot3 <- ggplot(prediction_df, aes(x = Index, y = Value, color = Type)) +
    +   geom_line(size = 0.8) +
    +   labs(title = "Test Set Predictions",
    +        x = "Time Index", y = "Value") +
    +   theme_minimal() +
    +   theme(plot.title = element_text(hjust = 0.5)) +
    +   scale_color_manual(values = c("Actual" = "blue",
    +                                  "GA Predicted" = "red",
    +                                  "OLS Predicted" = "green"))
    >
    > #print(plot3)
    >
    > # Plot 4: Parameter Comparison
    > params_df <- data.frame(
    +   Lag = factor(rep(1:n_lags, 2), levels = 1:n_lags),
    +   Method = rep(c("GA", "OLS"), each = n_lags),
    +   Coefficient = c(best_params, lm_coef)
    + )
    >
    > plot4 <- ggplot(params_df, aes(x = Lag, y = Coefficient, fill = Method)) +
    +   geom_bar(stat = "identity", position = position_dodge(width = 0.9), width = 0.7) +
    +   labs(title = "AR Parameter Comparison",
    +        x = "Lag", y = "Coefficient Value") +
    +   theme_minimal() +
    +   theme(plot.title = element_text(hjust = 0.5)) +
    +   scale_fill_manual(values = c("GA" = "red", "OLS" = "green")) +
    +   geom_hline(yintercept = 0, linetype = "dashed", alpha = 0.5) +
    +   geom_text(aes(label = sprintf("%.3f", Coefficient), y = Coefficient + 0.05 * sign(Coefficient)),
    +             position = position_dodge(width = 0.9), size = 3)
    >
    > #print(plot4)
    >
    > # Arrange all plots in a 2x2 grid
    > # grid.arrange(plot1, plot2, plot3, plot4, ncol = 2, nrow = 2)
    >
    >
    > # Additional diagnostic plots
    >
    > # Residual analysis
    > residuals_ga <- y_test - y_pred_test
    > residuals_lm <- y_test - lm_pred
    >
    > residual_df <- data.frame(
    +   Index = rep(test_indices, 2),
    +   Residuals = c(as.vector(residuals_ga), as.vector(residuals_lm)),
    +   Model = factor(rep(c("GA", "OLS"), each = length(y_test)))
    + )
    >
    > # Residual plot
    > plot5 <- ggplot(residual_df, aes(x = Index, y = Residuals, color = Model)) +
    +   geom_point(alpha = 0.6) +
    +   geom_hline(yintercept = 0, linetype = "dashed") +
    +   labs(title = "Residuals Comparison",
    +        x = "Time Index", y = "Residuals") +
    +   theme_minimal() +
    +   scale_color_manual(values = c("GA" = "red", "OLS" = "green"))
    >
    > #print(plot5)
    >
    > ggsave("d:/pdf/plot1_timeseries.pdf", plot1, width = 8, height = 6)
    `geom_smooth()` using formula = 'y ~ x'
    > ggsave("d:/pdf/plot2_convergence.pdf", plot2, width = 8, height = 6)
    > ggsave("d:/pdf/plot3_predictions.pdf", plot3, width = 8, height = 6)
    > ggsave("d:/pdf/plot4_parameters.pdf", plot4, width = 8, height = 6)
    > ggsave("d:/pdf/plot5_residual.pdf", plot5, width = 8, height = 6)
    >
    > pdf("d:/pdf/plot6_qq.pdf");
    > # Q-Q plot for normality check
    > par(mfrow = c(1, 2))
    > qqnorm(residuals_ga, main = "Q-Q Plot: GA Residuals")
    > qqline(residuals_ga, col = "red")
    > qqnorm(residuals_lm, main = "Q-Q Plot: OLS Residuals")
    > qqline(residuals_lm, col = "green")
    > par(mfrow = c(1, 1))
    > dev.off()
    >
    > # Print summary statistics
    > cat("\n", rep("=", 50), "\n")
    > cat("Summary Statistics\n")
    > cat(rep("=", 50), "\n")
    > cat(sprintf("Training data points: %d\n", nrow(train_data)))
    > cat(sprintf("Test data points: %d\n", nrow(test_data)))
    > cat(sprintf("Number of lags used: %d\n", n_lags))
    > cat(sprintf("GA final best fitness: %.4f\n", max(ga_result@fitness)))
    > cat(sprintf("GA final best MSE: %.4f\n", -max(ga_result@fitness)))
    >
    > # Check stationarity condition
    > cat("\nStationarity Check:\n")
    > cat("Sum of absolute GA parameters:", sum(abs(best_params)), "\n")
    > if (sum(abs(best_params)) < 1) {
    +   cat("? GA model satisfies stationarity condition\n")
    + } else {
    +   cat("? GA model may not be stationary\n")
    + }
    >
    > # Calculate additional metrics
    > ss_res <- sum((y_test - y_pred_test)^2)
    > ss_tot <- sum((y_test - mean(y_test))^2)
    > r_squared <- 1 - ss_res/ss_tot
    > cat(sprintf("\nR-squared: %.4f\n", r_squared))
    >
    > # AIC and BIC approximations
    > n <- length(y_test)
    > k <- n_lags
    > aic <- n * log(test_mse) + 2 * k
    > bic <- n * log(test_mse) + k * log(n)
    > cat(sprintf("AIC: %.4f\n", aic))
    > cat(sprintf("BIC: %.4f\n", bic))
    >
    > # Create a comprehensive report
    > cat("\n", rep("=", 50), "\n")
    > cat("MODEL INTERPRETATION\n")
    > cat(rep("=", 50), "\n")
    >
    > cat("\n1. Parameter Significance:\n")
    > for (i in 1:n_lags) {
    +   sig_level <- ifelse(abs(best_params[i]) > 0.1, "Strong",
    +                       ifelse(abs(best_params[i]) > 0.05, "Moderate", "Weak"))
    +   cat(sprintf("   Lag %d: %.4f (%s influence)\n", i, best_params[i], sig_level))
    + }
    >
    > cat("\n2. Model Characteristics:\n")
    > if (sum(best_params) > 0) {
    +   cat("   Positive persistence: Shocks persist in the same direction\n")
    + } else {
    +   cat("   Negative persistence: Shocks tend to reverse\n")
    + }
    >
    > if (best_params[1] > 0.5) {
    +   cat("   Strong short-term momentum detected\n")
    + }
    >
    > cat("\n3. Forecast Implications:\n")
    > cat(sprintf("   - {%.1f}%% of today's value carries to tomorrow\n", best_params[1] * 100))
    > cat(sprintf("   - Half-life of shocks: approximately %.1f periods\n",
    +             log(0.5)/log(abs(best_params[1]))))

    NOTE: Processing of R statements complete

    349       import data=workx.prediction_df  r=prediction_df  ;
    NOTE: Creating data set 'WORKX.prediction_df' from R data frame 'prediction_df'
    NOTE: Data set "WORKX.prediction_df" has 117 observation(s) and 3 variable(s)

    350       import data=workx.convergence_df r=convergence_df ;
    NOTE: Creating data set 'WORKX.convergence_df' from R data frame 'convergence_df'
    NOTE: Data set "WORKX.convergence_df" has 100 observation(s) and 3 variable(s)

    351       import data=workx.residual_df    r=residual_df    ;
    NOTE: Creating data set 'WORKX.residual_df' from R data frame 'residual_df'
    NOTE: Data set "WORKX.residual_df" has 78 observation(s) and 3 variable(s)

    352       run;
    NOTE: Procedure r step took :
          real time : 6.109
          cpu time  : 0.031


    ERROR: Error printed on page 1

    NOTE: Submitted statements took :
          real time : 6.379
          cpu time  : 0.125

    /*___                _   _
    |___ \   _ __  _   _| |_| |__   ___  _ __
      __) | | `_ \| | | | __| `_ \ / _ \| `_ \
     / __/  | |_) | |_| | |_| | | | (_) | | | |
    |_____| | .__/ \__, |\__|_| |_|\___/|_| |_|
            |_|    |___/
     _ __  _ __ ___   ___ ___  ___ ___
    | `_ \| `__/ _ \ / __/ _ \/ __/ __|
    | |_) | | | (_) | (_|  __/\__ \__ \
    | .__/|_|  \___/ \___\___||___/___/
    |_|
    */

    options set=PYTHONHOME "D:\py314";
    proc python;
    submit;
    import pyreadstat as ps
    import numpy as np
    import pandas as pd
    import matplotlib.pyplot as plt
    from typing import List, Tuple
    import random
    from sklearn.linear_model import LinearRegression
    from scipy import stats
    import os

    # Read SAS dataset
    df, meta = ps.read_sas7bdat('d:/wpswrkx/ar2model.sas7bdat')
    df.set_index('date', inplace=True)

    # Create lagged features for AR model
    def create_lagged_features(data, n_lags=5):
        df_lagged = pd.DataFrame(index=data.index)
        df_lagged['target'] = data.values

        for i in range(1, n_lags + 1):
            df_lagged[f'lag_{i}'] = data.shift(i)

        # Drop NaN values
        df_lagged.dropna(inplace=True)
        return df_lagged

    # Prepare data for modeling
    n_lags = 5
    df_lagged = create_lagged_features(df['value'], n_lags)
    print(f"\nLagged features shape: {df_lagged.shape}")

    # Genetic Algorithm Implementation
    class GeneticAlgorithmAR:
        def __init__(self, population_size=50, n_generations=100,
                     mutation_rate=0.1, crossover_rate=0.8,
                     n_lags=5, elitism_ratio=0.1):
            self.population_size = population_size
            self.n_generations = n_generations
            self.mutation_rate = mutation_rate
            self.crossover_rate = crossover_rate
            self.n_lags = n_lags
            self.elitism_ratio = elitism_ratio
            self.population = None
            self.fitness_history = []

        def initialize_population(self):
            """Initialize population with random AR parameters"""
            population = []
            for _ in range(self.population_size):
                # Random AR coefficients between -1 and 1
                chromosome = np.random.uniform(-1, 1, self.n_lags)
                population.append(chromosome)
            self.population = np.array(population)

        def fitness_function(self, chromosome, X_train, y_train):
            """Calculate fitness (negative MSE for minimization)"""
            # Make predictions
            y_pred = np.dot(X_train, chromosome)

            # Calculate MSE
            mse = np.mean((y_train - y_pred) ** 2)

            # Return negative MSE (for maximization)
            return -mse

        def selection(self, fitness_scores):
            """Tournament selection"""
            selected = []
            tournament_size = 3

            for _ in range(self.population_size):
                # Randomly select tournament_size individuals
                tournament_idx = np.random.choice(
                    len(self.population), tournament_size, replace=False)
                tournament_fitness = fitness_scores[tournament_idx]

                # Select the best in tournament
                winner_idx = tournament_idx[np.argmax(tournament_fitness)]
                selected.append(self.population[winner_idx])

            return np.array(selected)

        def crossover(self, parent1, parent2):
            """Single-point crossover"""
            if np.random.random() < self.crossover_rate:
                crossover_point = np.random.randint(1, self.n_lags - 1)
                child1 = np.concatenate([parent1[:crossover_point],
                                         parent2[crossover_point:]])
                child2 = np.concatenate([parent2[:crossover_point],
                                         parent1[crossover_point:]])
                return child1, child2
            return parent1.copy(), parent2.copy()

        def mutation(self, chromosome):
            """Gaussian mutation"""
            mutated = chromosome.copy()
            for i in range(len(mutated)):
                if np.random.random() < self.mutation_rate:
                    # Add small random perturbation
                    mutated[i] += np.random.normal(0, 0.1)
                    # Keep within bounds
                    mutated[i] = np.clip(mutated[i], -1, 1)
            return mutated

        def fit(self, X_train, y_train, verbose=True):
            """Run genetic algorithm"""
            self.initialize_population()
            best_fitness_history = []
            avg_fitness_history = []

            for generation in range(self.n_generations):
                # Calculate fitness for all individuals
                fitness_scores = np.array([
                    self.fitness_function(ind, X_train, y_train)
                    for ind in self.population
                ])

                # Track progress
                best_fitness = np.max(fitness_scores)
                avg_fitness = np.mean(fitness_scores)
                best_fitness_history.append(-best_fitness)  # Store MSE
                avg_fitness_history.append(-avg_fitness)

                if verbose and generation % 10 == 0:
                    print(f"Generation {generation}: Best MSE = {-best_fitness:.4f}, "
                          f"Avg MSE = {-avg_fitness:.4f}")

                # Selection
                selected_population = self.selection(fitness_scores)

                # Create new population
                new_population = []

                # Elitism: keep best individuals
                n_elite = int(self.population_size * self.elitism_ratio)
                elite_indices = np.argsort(fitness_scores)[-n_elite:]
                elite_population = self.population[elite_indices]

                # Generate rest through crossover and mutation
                while len(new_population) < self.population_size - n_elite:
                    # Select parents
                    parent1, parent2 = random.sample(list(selected_population), 2)

                    # Crossover
                    child1, child2 = self.crossover(parent1, parent2)

                    # Mutation
                    child1 = self.mutation(child1)
                    child2 = self.mutation(child2)

                    new_population.extend([child1, child2])

                # Add elite individuals
                new_population.extend(elite_population)
                self.population = np.array(new_population[:self.population_size])

            # Find best solution
            final_fitness = np.array([
                self.fitness_function(ind, X_train, y_train)
                for ind in self.population
            ])
            best_idx = np.argmax(final_fitness)
            best_solution = self.population[best_idx]

            self.best_fitness_history = best_fitness_history
            self.avg_fitness_history = avg_fitness_history

            return best_solution

    # Prepare train/test split
    train_size = int(len(df_lagged) * 0.8)
    train_data = df_lagged.iloc[:train_size]
    test_data = df_lagged.iloc[train_size:]

    X_train = train_data.drop('target', axis=1).values
    y_train = train_data['target'].values
    X_test = test_data.drop('target', axis=1).values
    y_test = test_data['target'].values

    # Run Genetic Algorithm
    print("\n" + "="*50)
    print("Running Genetic Algorithm for AR Parameter Optimization")
    print("="*50)

    ga = GeneticAlgorithmAR(population_size=100, n_generations=100,
                            mutation_rate=0.15, n_lags=n_lags)
    best_params = ga.fit(X_train, y_train)

    print(f"\nBest AR parameters found: {best_params}")

    # Evaluate on test set
    y_pred_test = np.dot(X_test, best_params)
    test_mse = np.mean((y_test - y_pred_test) ** 2)
    print(f"Test MSE: {test_mse:.4f}")
    print(f"Test RMSE: {np.sqrt(test_mse):.4f}")

    # Compare with standard OLS solution
    lr = LinearRegression()
    lr.fit(X_train, y_train)
    lr_pred = lr.predict(X_test)
    lr_mse = np.mean((y_test - lr_pred) ** 2)
    print(f"\nLinear Regression Test MSE: {lr_mse:.4f}")
    print(f"Linear Regression coefficients: {lr.coef_}")

    # Print summary statistics
    print("\n" + "="*50)
    print("Summary Statistics")
    print("="*50)
    print(f"Training data points: {len(train_data)}")
    print(f"Test data points: {len(test_data)}")
    print(f"Number of lags used: {n_lags}")
    print(f"GA final best MSE: {ga.best_fitness_history[-1]:.4f}")
    print(f"GA final avg MSE: {ga.avg_fitness_history[-1]:.4f}")

    # Additional metrics
    print("\n" + "="*50)
    print("Model Performance Metrics")
    print("="*50)
    print(f"GA R-squared: {1 - test_mse/np.var(y_test):.4f}")
    print(f"OLS R-squared: {1 - lr_mse/np.var(y_test):.4f}")
    print(f"GA AIC: {len(y_test) * np.log(test_mse) + 2 * n_lags:.4f}")
    print(f"OLS AIC: {len(y_test) * np.log(lr_mse) + 2 * n_lags:.4f}")

    endsubmit;
    run;

    /*           _               _
      ___  _   _| |_ _ __  _   _| |_
     / _ \| | | | __| `_ \| | | | __|
    | (_) | |_| | |_| |_) | |_| | |_
     \___/ \__,_|\__| .__/ \__,_|\__|
                    |_|
    */

    /**************************************************************************************************************************/
    /*   ==================================================                                                                   */
    /*  Running Genetic Algorithm for AR Parameter Optimization                                                               */
    /*  ==================================================                                                                    */
    /*                                                                                                                        */
    /*  Generation 0: Best MSE = 0.3147, Avg MSE = 2.5929                                                                     */
    /*  Generation 10: Best MSE = 0.2526, Avg MSE = 0.2763                                                                    */
    /*  Generation 20: Best MSE = 0.2480, Avg MSE = 0.2538                                                                    */
    /*  Generation 30: Best MSE = 0.2479, Avg MSE = 0.2528                                                                    */
    /*  Generation 40: Best MSE = 0.2478, Avg MSE = 0.2536                                                                    */
    /*  Generation 50: Best MSE = 0.2478, Avg MSE = 0.2558                                                                    */
    /*  Generation 60: Best MSE = 0.2478, Avg MSE = 0.2539                                                                    */
    /*  Generation 70: Best MSE = 0.2478, Avg MSE = 0.2550                                                                    */
    /*  Generation 80: Best MSE = 0.2478, Avg MSE = 0.2528                                                                    */
    /*  Generation 90: Best MSE = 0.2478, Avg MSE = 0.2542                                                                    */
    /*                                                                                                                        */
    /*  Best AR parameters found: [ 0.61609494  0.31641004 -0.03044781 -0.02051997  0.00525886]                               */
    /*                                                                                                                        */
    /*  Test MSE: 0.2157                                                                                                      */
    /*  Test RMSE: 0.4644                                                                                                     */
    /*                                                                                                                        */
    /*  Linear Regression Test MSE: 0.2173                                                                                    */
    /*  Linear Regression coefficients: [ 0.61168543  0.31630493 -0.02629849 -0.02968443  0.00704908]                         */
    /*                                                                                                                        */
    /*  ==================================================                                                                    */
    /*  Summary Statistics                                                                                                    */
    /*  ==================================================                                                                    */
    /*                                                                                                                        */
    /*  Training data points: 156                                                                                             */
    /*  Test data points: 39                                                                                                  */
    /*  Number of lags used: 5                                                                                                */
    /*  GA final best MSE: 0.2478                                                                                             */
    /*  GA final avg MSE: 0.2538                                                                                              */
    /*                                                                                                                        */
    /*  ==================================================                                                                    */
    /*  Model Performance Metrics                                                                                             */
    /*  ==================================================                                                                    */
    /*                                                                                                                        */
    /*  GA R-squared: 0.2021                                                                                                  */
    /*  OLS R-squared: 0.1963                                                                                                 */
    /*  GA AIC: -49.8185                                                                                                      */
    /*  OLS AIC: -49.5371                                                                                                     */
    /**************************************************************************************************************************/

     /*
    | | ___   __ _
    | |/ _ \ / _` |
    | | (_) | (_| |
    |_|\___/ \__, |
             |___/
    */

    1                                          Altair SLC       14:55 Wednesday, March 11, 2026

    NOTE: Copyright 2002-2025 World Programming, an Altair Company
    NOTE: Altair SLC 2026 (05.26.01.00.000758)
          Licensed to Roger DeAngelis
    NOTE: This session is executing on the X64_WIN11PRO platform and is running in 64 bit mode

    NOTE: AUTOEXEC processing beginning; file is C:\wpsoto\autoexec.sas
    NOTE: AUTOEXEC source line
    1       +  ï»¿ods _all_ close;
               ^
    ERROR: Expected a statement keyword : found "?"
    NOTE: Library workx assigned as follows:
          Engine:        SAS7BDAT
          Physical Name: d:\wpswrkx

    NOTE: Library slchelp assigned as follows:
          Engine:        WPD
          Physical Name: C:\Progra~1\Altair\SLC\2026\sashelp

    NOTE: Library worksas assigned as follows:
          Engine:        SAS7BDAT
          Physical Name: d:\worksas

    NOTE: Library workwpd assigned as follows:
          Engine:        WPD
          Physical Name: d:\workwpd


    LOG:  14:55:07
    NOTE: 1 record was written to file PRINT

    NOTE: The data step took :
          real time : 0.031
          cpu time  : 0.015


    NOTE: AUTOEXEC processing completed

    1         options set=PYTHONHOME "D:\py314";
    2         proc python;
    3         submit;
    4         import pyreadstat as ps
    5         import numpy as np
    6         import pandas as pd
    7         import matplotlib.pyplot as plt
    8         from typing import List, Tuple
    9         import random
    10        from sklearn.linear_model import LinearRegression
    11        from scipy import stats
    12        import os
    13        from matplotlib.backends.backend_pdf import PdfPages  # Missing import!
    14
    15        # Read SAS dataset
    16        df, meta = ps.read_sas7bdat('d:/wpswrkx/ar2model.sas7bdat')
    17        df.set_index('date', inplace=True)
    18
    19        # Create lagged features for AR model
    20        def create_lagged_features(data, n_lags=5):
    21            df_lagged = pd.DataFrame(index=data.index)
    22            df_lagged['target'] = data.values
    23
    24            for i in range(1, n_lags + 1):
    25                df_lagged[f'lag_{i}'] = data.shift(i)
    26
    27            # Drop NaN values
    28            df_lagged.dropna(inplace=True)
    29            return df_lagged
    30
    31        # Prepare data for modeling
    32        n_lags = 5
    33        df_lagged = create_lagged_features(df['value'], n_lags)
    34        print(f"\nLagged features shape: {df_lagged.shape}")
    35
    36        # Genetic Algorithm Implementation
    37        class GeneticAlgorithmAR:
    38            def __init__(self, population_size=50, n_generations=100,
    39                         mutation_rate=0.1, crossover_rate=0.8,
    40                         n_lags=5, elitism_ratio=0.1):
    41                self.population_size = population_size
    42                self.n_generations = n_generations
    43                self.mutation_rate = mutation_rate
    44                self.crossover_rate = crossover_rate
    45                self.n_lags = n_lags
    46                self.elitism_ratio = elitism_ratio
    47                self.population = None
    48                self.fitness_history = []
    49
    50            def initialize_population(self):
    51                """Initialize population with random AR parameters"""
    52                population = []
    53                for _ in range(self.population_size):
    54                    # Random AR coefficients between -1 and 1
    55                    chromosome = np.random.uniform(-1, 1, self.n_lags)
    56                    population.append(chromosome)
    57                self.population = np.array(population)
    58
    59            def fitness_function(self, chromosome, X_train, y_train):
    60                """Calculate fitness (negative MSE for minimization)"""
    61                # Make predictions
    62                y_pred = np.dot(X_train, chromosome)
    63
    64                # Calculate MSE
    65                mse = np.mean((y_train - y_pred) ** 2)
    66
    67                # Return negative MSE (for maximization)
    68                return -mse
    69
    70            def selection(self, fitness_scores):
    71                """Tournament selection"""
    72                selected = []
    73                tournament_size = 3
    74
    75                for _ in range(self.population_size):
    76                    # Randomly select tournament_size individuals
    77                    tournament_idx = np.random.choice(
    78                        len(self.population), tournament_size, replace=False)
    79                    tournament_fitness = fitness_scores[tournament_idx]
    80
    81                    # Select the best in tournament
    82                    winner_idx = tournament_idx[np.argmax(tournament_fitness)]
    83                    selected.append(self.population[winner_idx])
    84
    85                return np.array(selected)
    86
    87            def crossover(self, parent1, parent2):
    88                """Single-point crossover"""
    89                if np.random.random() < self.crossover_rate:
    90                    crossover_point = np.random.randint(1, self.n_lags - 1)
    91                    child1 = np.concatenate([parent1[:crossover_point],
    92                                             parent2[crossover_point:]])
    93                    child2 = np.concatenate([parent2[:crossover_point],
    94                                             parent1[crossover_point:]])
    95                    return child1, child2
    96                return parent1.copy(), parent2.copy()
    97
    98            def mutation(self, chromosome):
    99                """Gaussian mutation"""
    100               mutated = chromosome.copy()
    101               for i in range(len(mutated)):
    102                   if np.random.random() < self.mutation_rate:
    103                       # Add small random perturbation
    104                       mutated[i] += np.random.normal(0, 0.1)
    105                       # Keep within bounds
    106                       mutated[i] = np.clip(mutated[i], -1, 1)
    107               return mutated
    108
    109           def fit(self, X_train, y_train, verbose=True):
    110               """Run genetic algorithm"""
    111               self.initialize_population()
    112               best_fitness_history = []
    113               avg_fitness_history = []
    114
    115               for generation in range(self.n_generations):
    116                   # Calculate fitness for all individuals
    117                   fitness_scores = np.array([
    118                       self.fitness_function(ind, X_train, y_train)
    119                       for ind in self.population
    120                   ])
    121
    122                   # Track progress
    123                   best_fitness = np.max(fitness_scores)
    124                   avg_fitness = np.mean(fitness_scores)
    125                   best_fitness_history.append(-best_fitness)  # Store MSE
    126                   avg_fitness_history.append(-avg_fitness)
    127
    128                   if verbose and generation % 10 == 0:
    129                       print(f"Generation {generation}: Best MSE = {-best_fitness:.4f}, "
    130                             f"Avg MSE = {-avg_fitness:.4f}")
    131
    132                   # Selection
    133                   selected_population = self.selection(fitness_scores)
    134
    135                   # Create new population
    136                   new_population = []
    137
    138                   # Elitism: keep best individuals
    139                   n_elite = int(self.population_size * self.elitism_ratio)
    140                   elite_indices = np.argsort(fitness_scores)[-n_elite:]
    141                   elite_population = self.population[elite_indices]
    142
    143                   # Generate rest through crossover and mutation
    144                   while len(new_population) < self.population_size - n_elite:
    145                       # Select parents
    146                       parent1, parent2 = random.sample(list(selected_population), 2)
    147
    148                       # Crossover
    149                       child1, child2 = self.crossover(parent1, parent2)
    150
    151                       # Mutation
    152                       child1 = self.mutation(child1)
    153                       child2 = self.mutation(child2)
    154
    155                       new_population.extend([child1, child2])
    156
    157                   # Add elite individuals
    158                   new_population.extend(elite_population)
    159                   self.population = np.array(new_population[:self.population_size])
    160
    161               # Find best solution
    162               final_fitness = np.array([
    163                   self.fitness_function(ind, X_train, y_train)
    164                   for ind in self.population
    165               ])
    166               best_idx = np.argmax(final_fitness)
    167               best_solution = self.population[best_idx]
    168
    169               self.best_fitness_history = best_fitness_history
    170               self.avg_fitness_history = avg_fitness_history
    171
    172               return best_solution
    173
    174       # Prepare train/test split
    175       train_size = int(len(df_lagged) * 0.8)
    176       train_data = df_lagged.iloc[:train_size]
    177       test_data = df_lagged.iloc[train_size:]
    178
    179       X_train = train_data.drop('target', axis=1).values
    180       y_train = train_data['target'].values
    181       X_test = test_data.drop('target', axis=1).values
    182       y_test = test_data['target'].values
    183
    184       # Run Genetic Algorithm
    185       print("\n" + "="*50)
    186       print("Running Genetic Algorithm for AR Parameter Optimization")
    187       print("="*50)
    188
    189       ga = GeneticAlgorithmAR(population_size=100, n_generations=100,
    190                               mutation_rate=0.15, n_lags=n_lags)
    191       best_params = ga.fit(X_train, y_train)
    192
    193       print(f"\nBest AR parameters found: {best_params}")
    194
    195       # Evaluate on test set
    196       y_pred_test = np.dot(X_test, best_params)
    197       test_mse = np.mean((y_test - y_pred_test) ** 2)
    198       print(f"Test MSE: {test_mse:.4f}")
    199       print(f"Test RMSE: {np.sqrt(test_mse):.4f}")
    200
    201       # Compare with standard OLS solution
    202       lr = LinearRegression()
    203       lr.fit(X_train, y_train)
    204       lr_pred = lr.predict(X_test)
    205       lr_mse = np.mean((y_test - lr_pred) ** 2)
    206       print(f"\nLinear Regression Test MSE: {lr_mse:.4f}")
    207       print(f"Linear Regression coefficients: {lr.coef_}")
    208
    209       # Print summary statistics
    210       print("\n" + "="*50)
    211       print("Summary Statistics")
    212       print("="*50)
    213       print(f"Training data points: {len(train_data)}")
    214       print(f"Test data points: {len(test_data)}")
    215       print(f"Number of lags used: {n_lags}")
    216       print(f"GA final best MSE: {ga.best_fitness_history[-1]:.4f}")
    217       print(f"GA final avg MSE: {ga.avg_fitness_history[-1]:.4f}")
    218
    219       # Additional metrics
    220       print("\n" + "="*50)
    221       print("Model Performance Metrics")
    222       print("="*50)
    223       print(f"GA R-squared: {1 - test_mse/np.var(y_test):.4f}")
    224       print(f"OLS R-squared: {1 - lr_mse/np.var(y_test):.4f}")
    225       print(f"GA AIC: {len(y_test) * np.log(test_mse) + 2 * n_lags:.4f}")
    226       print(f"OLS AIC: {len(y_test) * np.log(lr_mse) + 2 * n_lags:.4f}")
    227
    228       endsubmit;

    NOTE: Submitting statements to Python:


    229       run;
    NOTE: Procedure python step took :
          real time : 4.418
          cpu time  : 0.062


    230
    ERROR: Error printed on page 1

    NOTE: Submitted statements took :
          real time : 4.497
          cpu time  : 0.140

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
