_D_e_t_e_c_t_i_n_g _B_r_e_a_k_p_o_i_n_t_s _a_n_d _E_s_t_i_m_a_t_i_n_g _S_e_g_m_e_n_t_s _i_n _T_r_e_n_d (_D_B_E_S_T)

_D_e_s_c_r_i_p_t_i_o_n:

     A program for analyzing vegetation time series, with two
     algorithms: 1) change detection algorithm that detects trend
     changes, determines their type (abrupt or non-abrupt), and
     estimates their timing, magnitude, number, and direction; 2)
     generalization algorithm that simplifies the temporal trend into
     main features. The user can set the number of major breakpoints or
     magnitude of greatest changes of interest for detection, and can
     control the generalization process by setting an additional
     parameter of generalization-percentage.

_U_s_a_g_e:

     DBEST(data, data.type, seasonality = -1, algorithm, breakpoints.no = -1, 
     generalization.percent = -1, change.magnitude = -1, first.level.shift, 
     second.level.shift, duration, distance.threshold, alpha, plot = -1)
     
_A_r_g_u_m_e_n_t_s:

    data: univariate time-series to be analysed. This should be an
          object of class 'ts'/'zoo' with a frequency greater than one
          without NA's or a vector without NA's. If the input data is a
          vector the algorithm will automatically assign a start year.

data.type: the data type. There are two options: "cyclical" for time
          series with a seasonal cycle, or "non-cyclical" for time
          series without seasonal cycle (e.g. deseasonalized data)

seasonality: the seasonality period as a number. If the input data type
          is non-cyclical this variable should be omitted or set to
          'none'/'null'. This parameter will overwrite the frequency
          value of an input object of class 'ts'/'zoo'. However, if the
          input data is an object of class 'ts'/'zoo' and the
          seasonality period is omitted, then the algorithm will use as
          a seasonality period the frequency value from the input data.

algorithm: the algorithm mode. There are two options: "change
          detection" and "generalization".

breakpoints.no: the number of greatest changes to be detected (change
          detection algorithm); the number of major breakpoints to be
          included in the generalized trend (generalization algorithm).
          This parameter should be omitted if 'generalization.percent'
          or 'change.magnitude' is in use.

change.magnitude: the lowest magnitude for the changes to be detected
          (change detection algorithm); the largest variation allowed
          within a generalized segment (generalization algorithm). This
          parameter should be omitted if 'breakpoints.no' or
          'generalization.percent' is in use.

generalization.percent: the highest percent (between 0 to 100) the
          trend should be generalized; (0 = the least-simplified trend;
          100 = the most-simplified trend). This parameter should be
          omitted if 'breakpoints.no' or 'change.magnitude' is in use.

first.level.shift: the first level-shift-threshold value. This
          parameter corresponds to the lowest absolute difference in
          the time-series data between the level-shift point (abrupt
          change) and the next data point.

second.level.shift: the second level-shift-threshold value. This
          parameter corresponds to the lowest absolute difference in
          the means of the data calculated over the duration period
          before and after the level-shift point.

duration: the duration threshold value. This parameter corresponds to
          the lowest time period (time steps) within which the shift in
          the mean of the data level, before and after the level-shift
          point, persists; and, the lowest spacing (time steps) between
          successive level-shift points.

distance.threshold: the distance threshold value. This correspond to
          the the lowest perpendicular distance from farthest data
          point to the straight line passing through every pair of
          successive peak and valley points. The algorithm will
          estimate a distance threshold if this parameter is set to
          'default'.

   alpha: the statistical significance level value used for testing the
          significance of detected changes.

    plot: display figures. This parameter could be omitted or set to:
          "on", "fig1", "fig2" or "off". The "fig1" option will display
          the input data and the estimated trend, plus the trend local
          change. The "fig2" option will display a graph with the
          decomposition of the time-series, including the actual data,
          the trend, the seasonal component and the remainder. The "on"
          option displays both 'figure 1' and 'figure 2'. The "off"
          option displays no figure.

_D_e_t_a_i_l_s:

     An object of the class "DBEST" is a list with elements depending
     on whether the generalization algorithm or change detection
     algorithm is used.

_V_a_l_u_e:

BreakpointNo: the number of breakpoints or changes detected.

SegmentNo: the number of segments estimated by the algorithm.

   Start: a list with numbers representing the starting points of the
          changes as time-steps.

Duration: a list with numbers representing the durations of the changes
          as time-steps.

     End: a list with numbers representing the ending points of the
          changes as time-steps.

  Change: a list with the values of the changes.

ChangeType: a list with the types of the changes as numbers which could
          be 0 or 1. The numbers correspond to a non-abrupt change (0)
          or abrupt change (1).

Significance: a list with the statistical significances of the changes
          as numbers which could be 0 or 1. The numbers correspond to a
          statistically in-significant change (0) or significant change
          (1).

    RMSE: the calculated Root Mean Squares Error of the fit.

     MAD: the calculated Maximum Absolute Difference of the fit.

_N_o_t_e:

     1) DBEST detects changes requested by the user, and determines the
     type of the detected changes based on the definition of abrupt
     change (level-shift) made by user. The user can define what
     properties a data point must have, based on the studied
     application, to be considered as abrupt change or a level-shift.
     This is done using the three arguments of: first.level.shift,
     second.level.shift, and duration.  For example, for the vegetation
     change application using monthly NDVI time-series studied in
     Jamali et al. 2015, an abrupt changes is a one time-step change >=
     0.1 (NDVI units) that results in a shift >= 0.2 (NDVI units) in
     the mean level of NDVI, and the shift is valid for at-least two
     years (24 months).

     2) Dashed vertical lines mark the STARTING point of detected
     changes.

     3) Abrupt changes are in RED and non-abrupt changes are in ORANGE.

     4) Here, DBEST uses a seasonal-trend decomposition method (STL)
     with slightly different setting parameters compared to that used
     in Jamali et al. 2015. This may lead to little change to the plots
     of the test data below compared to figures 4 and 5 published in
     Jamali et al. 2015.

_A_u_t_h_o_r(_s):

     Sadegh Jamali, Hristo Tomov

_R_e_f_e_r_e_n_c_e_s:

     Jamali S, Jönsson P, Eklundh L, Ardö J, Seaquist J (2015).
     Detecting changes in vegetation trends using time series
     segmentation. Remote Sensing of Environment, 156, 182-195.
     http://dx.doi.org/10.1016/j.rse.2014.09.010

     Tomov H (2016). Automated temporal NDVI analysis over the Middle
     East for the period 1982 – 2010.
     http://lup.lub.lu.se/student-papers/record/8871893

_S_e_e _A_l_s_o:

     ‘plot.DBEST’ for plotting of DBEST() results.

_E_x_a_m_p_l_e_s:

     data(NDVI.Site1)
     NDVI.Example1 <- ts(NDVI.Site1, start=1982, frequency=12)
     
     data(NDVI.Site2)
     NDVI.Example2 <- ts(NDVI.Site2, start=1982, frequency=12)
     
     # Examples of a trend generalisation (Site 1 & Site 2, Jamali et al. 2015)
     #(a, f) no-breakpoint
     DBEST.a <- DBEST(data=NDVI.Example1, data.type="cyclical", 
                      seasonality=12, algorithm="generalisation", 
                      breakpoints.no=0, first.level.shift=0.1, 
                      second.level.shift=0.2, duration=24, 
                      distance.threshold="default", alpha=0.05, plot="fig1")
     
     DBEST.f <- DBEST(data=NDVI.Example2, data.type="cyclical", 
                      seasonality=12, algorithm="generalisation", 
                      breakpoints.no=0, first.level.shift=0.1, 
                      second.level.shift=0.2, duration=24, 
                      distance.threshold="default", alpha=0.05, plot="fig1")
     
     #(b, g) major breakpoint
     DBEST.b <- DBEST(data=NDVI.Example1, data.type="cyclical", 
                      seasonality=12, algorithm="generalisation", 
                      breakpoints.no=1, first.level.shift=0.1, 
                      second.level.shift=0.2, duration=24, 
                      distance.threshold="default", alpha=0.05, plot="fig1")
     
     DBEST.g <- DBEST(data=NDVI.Example2, data.type="cyclical", 
                      seasonality=12, algorithm="generalisation", 
                      breakpoints.no=1, first.level.shift=0.1, 
                      second.level.shift=0.2, duration=24, 
                      distance.threshold="default", alpha=0.05, plot="fig1")
     
     #(c, h) change magnitude 0.1
     DBEST.c <- DBEST(data=NDVI.Example1, data.type="cyclical", 
                      seasonality=12, algorithm="generalisation", 
                      change.magnitude=0.1, first.level.shift=0.1, 
                      second.level.shift=0.2, duration=24, 
                      distance.threshold="default", alpha=0.05, plot="fig1")
     
     DBEST.h <- DBEST(data=NDVI.Example2, data.type="cyclical", 
                      seasonality=12, algorithm="generalisation", 
                      change.magnitude=0.1, first.level.shift=0.1, 
                      second.level.shift=0.2, duration=24, 
                      distance.threshold="default", alpha=0.05, plot="fig1")
     
     #(d, i) all-inclusive breakpoints
     DBEST.d <- DBEST(data=NDVI.Example1, data.type="cyclical", 
                      seasonality=12, algorithm="generalisation", 
                      generalization.percent=0, first.level.shift=0.1, 
                      second.level.shift=0.2, duration=24, 
                      distance.threshold="default", alpha=0.05, plot="fig1")
     
     DBEST.i <- DBEST(data=NDVI.Example2, data.type="cyclical", 
                      seasonality=12, algorithm="generalisation", 
                      generalization.percent=0, first.level.shift=0.1, 
                      second.level.shift=0.2, duration=24, 
                      distance.threshold="default", alpha=0.05, plot="fig1")
     
     # Examples of a change detection (Site 1 & Site 2, Jamali et al. 2015)
     
     #(a, b) detection the three major changes
     DBEST.a <- DBEST(data=NDVI.Example1, data.type="cyclical", 
                      seasonality=12, algorithm="change detection", 
                      breakpoints.no=3, first.level.shift=0.1, 
                      second.level.shift=0.2, duration=24, 
                      distance.threshold="default", alpha=0.05, plot="on")
     
     DBEST.b <- DBEST(data=NDVI.Example2, data.type="cyclical", 
                      seasonality=12, algorithm="change detection", 
                      breakpoints.no=3, first.level.shift=0.1, 
                      second.level.shift=0.2, duration=24, 
                      distance.threshold="default", alpha=0.05, plot="on")
     
