# utl_standard_deviation_of_90_day_rolling_standard_deviations
Standard deviation of 90 day on rolling standard deviations. Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS community.
    Standard deviation of 90 day rolling standard deviations

     WPS/Proc R

     10 million readings is a tiny amount of data.
     Are you sure you did not mean 100 billion(big data)?

     Benchmark for rolling 90 day standard deviations
     Seconds

       user  system elapsed
       2.02    0.45    2.46


    gethub
    https://tinyurl.com/y8v6kqvt
    https://github.com/rogerjdeangelis/utl_standard_deviation_of_90_day_rolling_standard_deviations

    SAS Forum
    https://tinyurl.com/yb4lyx8a
    https://communities.sas.com/t5/Base-SAS-Programming/Standard-deviation-on-rolling-basis/m-p/446482

    latest version of RollingWindow
    https://github.com/andrewuhl/RollingWindow

    INPUT
    =====

     SD1.HAVE total obs=10,000,000

         Obs    PRICES

           1      24
           2       8
           3      38
           4       9
           5      25
           6       8
           7       3
           8      10
           9      44
          10      14
       ....      ...
     9999999      22
    10000000      56


    PROCESS (working code)
    ======================

        have[,want := RollingStd(PRICES,window = 90)]

    OUTPUT
    ======

       Up to 40 obs from wantwps total obs=10,000,000

            Obs    PRICES    WANT

              1      24        .
              2       8        .
              3      38        .
              4       9        .
              5      25        .
           ...
             87       6        .
             88       4        .
             89      78        .
             90      92      28.3270
             91      85      28.5328
             92      41      28.2457
             93       0      28.6577
           ...

        9999998      74      28.9483
        9999999      10      29.2666
       10000000      42      29.2712

    *                _              _       _
     _ __ ___   __ _| | _____    __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \  / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/ | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|  \__,_|\__,_|\__\__,_|

    ;

    options validvarname=upcase;
    libname sd1 "d:/sd1";
    data sd1.have;
     do dat=1 to 10e6;
       prices=int(100*uniform(1234));
       drop dat;
       output;
     end;
    run;quit;

    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __
    / __|/ _ \| | | | | __| |/ _ \| '_ \
    \__ \ (_) | | |_| | |_| | (_) | | | |
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|

    ;

    %utl_submit_wps64('
    libname sd1 sas7bdat "d:/sd1";
    options set=R_HOME "C:/Program Files/R/R-3.3.1";
    libname wrk sas7bdat "%sysfunc(pathname(work))";
    proc r;
    submit;
    library(haven);
    library(dplyr);
    library(data.table);
    library(RollingWindow);
    have<-as.data.table(read_sas("d:/sd1/have.sas7bdat"));
    system.time(have[,want := RollingStd(PRICES,window = 90)]);
    endsubmit;
    import r=have  data=wrk.wantwps;
    run;quit;
    ');

