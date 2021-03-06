# utl_compare_corresp_vs_report_output_datasets
Sort, Summarize and Transpose using one proc. Comparison between the datasets created by 'proc corresp' and 'proc report'

    ```  SAS-L: A comparison of output crosstabulation tables from 'proc corresp' and 'proc report'.                                                                  ```
    ```                                                                                                                                                               ```
    ```    Goal                                                                                                                                                       ```
    ```      Sort, summarize and traspose in one proc, prior to 'proc print' or 'proc report'.                                                                        ```
    ```                                                                                                                                                               ```
    ```                                                                                                                                                               ```
    ```    CONCLUSION                                                                                                                                                 ```
    ```                                                                                                                                                               ```
    ```     Use corresp if you have stat.                                                                                                                             ```
    ```     I am not a expert in 'proc report' but have painted myself                                                                                                ```
    ```     in a corner many times.                                                                                                                                   ```
    ```                                                                                                                                                               ```
    ```                                                                                                                                                               ```
    ```    CORRESP                                                                                                                                                    ```
    ```                                                                                                                                                               ```
    ```     Positives                                                                                                                                                 ```
    ```                                                                                                                                                               ```
    ```      1. Proc corresp provides                                                                                                                                 ```
    ```           Count cross tables                                                                                                                                  ```
    ```           Row percent crosstabbs                                                                                                                              ```
    ```           Col percent crosstabs                                                                                                                               ```
    ```      2. Automatic 'nice' names for across columns                                                                                                             ```
    ```                                                                                                                                                               ```
    ```      3. You may need to sort the input because corresp does not support a class statement,                                                                    ```
    ```         so if you have 3 categorical variables you may need a 'by' statement.                                                                                 ```
    ```         The table statement supports more than two levels, like 'proc report'.                                                                                ```
    ```           tables sex, sex age                                                                                                                                 ```
    ```                                                                                                                                                               ```
    ```     Negatives                                                                                                                                                 ```
    ```                                                                                                                                                               ```
    ```      1. Is not in base SAS                                                                                                                                    ```
    ```      2. Need extra ODS exclude all then select just what you want or                                                                                          ```
    ```         cpu cycles may spike.                                                                                                                                 ```
    ```      3. Separate datasets for the thre types of percents, ie row, column and rowxcolumn.                                                                      ```
    ```         However it is not easy in 'report' either.                                                                                                            ```
    ```                                                                                                                                                               ```
    ```     REPORT                                                                                                                                                    ```
    ```     ======                                                                                                                                                    ```
    ```                                                                                                                                                               ```
    ```      Positives                                                                                                                                                ```
    ```                                                                                                                                                               ```
    ```       1. Handles three or more categorical variables                                                                                                          ```
    ```          without a sort.                                                                                                                                      ```
    ```       2. Do not need stat                                                                                                                                     ```
    ```       3. Can easily add row statstics ie sums, means, max ....                                                                                                ```
    ```       4. Mutiple differenct across variables                                                                                                                  ```
    ```                                                                                                                                                               ```
    ```      Negatives                                                                                                                                                ```
    ```                                                                                                                                                               ```
    ```       1. You need to rename columns                                                                                                                           ```
    ```       2. You need compute blocks to produce row and column percents? (gets messy? - hardcoded _cs)                                                            ```
    ```       3. Does not support a simplified arry statement, you need to know the column count.                                                                     ```
    ```                                                                                                                                                               ```
    ```           array ary _c:;             * not supported                                                                                                          ```
    ```           rename=(_c:= _2014-_2017)  * this is never supported                                                                                                ```
    ```                                                                                                                                                               ```
    ```           array ary[4]  _c2_ _c3_ _c4_ _c5_  * is supported                                                                                                   ```
    ```                                                                                                                                                               ```
    ```                                                                                                                                                               ```
    ```    A comparison of crosstabulation datasets from 'proc corresp' and 'proc report'.                                                                            ```
    ```    Note 'proc tabulate' often switches to a long and skinny output structure,                                                                                 ```
    ```    like proc summary.                                                                                                                                         ```
    ```                                                                                                                                                               ```
    ```    The main reason for pivoting long and skinny to fat is to prepare data                                                                                     ```
    ```    JUST BEFORE a report or excel output. Data should not be saved in the fat structure.                                                                       ```
    ```    I like to prepare data for a a final dumb 'proc print' or 'proc report'.                                                                                   ```
    ```    I like to minimize compute blocks and across variables.                                                                                                    ```
    ```                                                                                                                                                               ```
    ```    Sort, summarize and transpose                                                                                                                              ```
    ```                                                                                                                                                               ```
    ```    WORK.CLASS total obs=331                                                                                                                                   ```
    ```                                                                                                                                                               ```
    ```    Obs    YEAR    SEX    CNT                                                                                                                                  ```
    ```                                                                                                                                                               ```
    ```      1    2014     F      75                                                                                                                                  ```
    ```      2    2014     M     100                                                                                                                                  ```
    ```      3    2014     M      57                                                                                                                                  ```
    ```      4    2014     F      14                                                                                                                                  ```
    ```    ...                                                                                                                                                        ```
    ```                                                                                                                                                               ```
    ```    into the fat structures below                                                                                                                              ```
    ```                                                                                                                                                               ```
    ```     WORKING CODE                                                                                                                                              ```
    ```                                                                                                                                                               ```
    ```      CORRESP                                                                                                                                                  ```
    ```                                                                                                                                                               ```
    ```       proc corresp data=class all dimens=1 print=both outf=outf;                                                                                              ```
    ```         ods output observed   =xpocnt(rename=(label=Sex sum=rowcnt));                                                                                         ```
    ```         tables   sex, year;                                                                                                                                   ```
    ```         weight cnt;                                                                                                                                           ```
    ```                                                                                                                                                               ```
    ```       WORK.XPOCNTtotal obs=3                                                                                                                                  ```
    ```                                                                                                                                                               ```
    ```       SEX    _2014    _2015    _2016    _2017       ROWCNT                                                                                                    ```
    ```                                                                                                                                                               ```
    ```       F       1588     2264     2136     1950         7938                                                                                                    ```
    ```       M       2360     1474     2331     1737         7902                                                                                                    ```
    ```                                                                                                                                                               ```
    ```       Sum     3948     3738     4467     3687        15840                                                                                                    ```
    ```                                                                                                                                                               ```
    ```      REPORT                                                                                                                                                   ```
    ```                                                                                                                                                               ```
    ```       proc report data=class out=clsXpo(rename=(                                                                                                              ```
    ```          _C2_ =_2014 _C3_ =_2015 _C4_ =_2016 _C5_ =_2017));                                                                                                   ```
    ```        cols sex year, (cnt) cnt=tot;                                                                                                                          ```
    ```         define sex / group;                                                                                                                                   ```
    ```         define year / across;                                                                                                                                 ```
    ```         define tot / computed sum "rowcnt";                                                                                                                   ```
    ```         define cnt / analysis sum '';                                                                                                                         ```
    ```         rbreak after / summarize;                                                                                                                             ```
    ```                                                                                                                                                               ```
    ```       WORK.CLSXPO total obs=3                                                                                                                                 ```
    ```                                                                                                                                                               ```
    ```       SEX    _2014    _2015    _2016    _2017     TOT     _BREAK_                                                                                             ```
    ```                                                                                                                                                               ```
    ```        F      1588     2264     2136     1950     7938                                                                                                        ```
    ```        M      2360     1474     2331     1737     7902                                                                                                        ```
    ```                                                                                                                                                               ```
    ```               3948     3738     4467     3687    15840    _RBREAK_                                                                                            ```
    ```                                                                                                                                                               ```
    ```  HAVE                                                                                                                                                         ```
    ```  ====                                                                                                                                                         ```
    ```                                                                                                                                                               ```
    ```      Up to 40 obs WORK.CLASS total obs=331                                                                                                                    ```
    ```                                                                                                                                                               ```
    ```      Obs    YEAR    SEX    CNT                                                                                                                                ```
    ```                                                                                                                                                               ```
    ```        1    2014     F      75                                                                                                                                ```
    ```        2    2014     M     100                                                                                                                                ```
    ```        3    2014     M      57                                                                                                                                ```
    ```        4    2014     F      14                                                                                                                                ```
    ```        5    2014     F      32                                                                                                                                ```
    ```        6    2014     F       6                                                                                                                                ```
    ```        7    2014     M     100                                                                                                                                ```
    ```        8    2014     M      27                                                                                                                                ```
    ```        9    2014     F      53                                                                                                                                ```
    ```       ...                                                                                                                                                     ```
    ```                                                                                                                                                               ```
    ```  WANT                                                                                                                                                         ```
    ```  ====                                                                                                                                                         ```
    ```                                                                                                                                                               ```
    ```    REPORT                                                                                                                                                     ```
    ```    ======                                                                                                                                                     ```
    ```                                                                                                                                                               ```
    ```    WORK.CLSXPO total obs=3                                                                                                                                    ```
    ```                                                                                                                                                               ```
    ```    SEX    _2014    _2015    _2016    _2017     TOT     _BREAK_                                                                                                ```
    ```                                                                                                                                                               ```
    ```     F      1588     2264     2136     1950     7938                                                                                                           ```
    ```     M      2360     1474     2331     1737     7902                                                                                                           ```
    ```                                                                                                                                                               ```
    ```            3948     3738     4467     3687    15840    _RBREAK_                                                                                               ```
    ```                                                                                                                                                               ```
    ```     CORRESP                                                                                                                                                   ```
    ```     =======                                                                                                                                                   ```
    ```                                                                                                                                                               ```
    ```     COUNTS  -- XPOCNT total obs=3                                                                                                                             ```
    ```                                                                                                                                                               ```
    ```      SEX    _2014    _2015    _2016    _2017     SUM                                                                                                          ```
    ```                                                                                                                                                               ```
    ```      F       1588     2264     2136     1950     7938                                                                                                         ```
    ```      M       2360     1474     2331     1737     7902                                                                                                         ```
    ```                                                                                                                                                               ```
    ```      Sum     3948     3738     4467     3687    15840                                                                                                         ```
    ```                                                                                                                                                               ```
    ```     PERCENTS                                                                                                                                                  ```
    ```                                                                                                                                                               ```
    ```      SEX     _2014      _2015      _2016      _2017       SUM                                                                                                 ```
    ```                                                                                                                                                               ```
    ```      F      10.0253    14.2929    13.4848    12.3106     50.114                                                                                               ```
    ```      M      14.8990     9.3056    14.7159    10.9659     49.886                                                                                               ```
    ```                                                                                                                                                               ```
    ```      Sum    24.9242    23.5985    28.2008    23.2765    100.000                                                                                               ```
    ```                                                                                                                                                               ```
    ```                                                                                                                                                               ```
    ```     COLUMN PERCENTS                                                                                                                                           ```
    ```                                                                                                                                                               ```
    ```      SEX     _2014      _2015      _2016      _2017                                                                                                           ```
    ```                                                                                                                                                               ```
    ```       F     40.2229    60.5671    47.8173    52.8885                                                                                                          ```
    ```       M     59.7771    39.4329    52.1827    47.1115                                                                                                          ```
    ```                                                                                                                                                               ```
    ```                                                                                                                                                               ```
    ```      ROW PERCENTS                                                                                                                                             ```
    ```                                                                                                                                                               ```
    ```      SEX     _2014      _2015      _2016      _2017                                                                                                           ```
    ```                                                                                                                                                               ```
    ```       F     20.0050    28.5210    26.9085    24.5654                                                                                                          ```
    ```       M     29.8659    18.6535    29.4989    21.9818                                                                                                          ```
    ```                                                                                                                                                               ```
    ```  *                _               _       _                                                                                                                   ```
    ```   _ __ ___   __ _| | _____     __| | __ _| |_ __ _                                                                                                            ```
    ```  | '_ ` _ \ / _` | |/ / _ \   / _` |/ _` | __/ _` |                                                                                                           ```
    ```  | | | | | | (_| |   <  __/  | (_| | (_| | || (_| |                                                                                                           ```
    ```  |_| |_| |_|\__,_|_|\_\___|   \__,_|\__,_|\__\__,_|                                                                                                           ```
    ```                                                                                                                                                               ```
    ```  ;                                                                                                                                                            ```
    ```                                                                                                                                                               ```
    ```  data class(drop=rec);                                                                                                                                        ```
    ```    call streaminit(1234);                                                                                                                                     ```
    ```    do year=2014 to 2017;                                                                                                                                      ```
    ```      do rec=1 to 100;                                                                                                                                         ```
    ```      if rand("uniform")<.5 then sex='M';                                                                                                                      ```
    ```      else sex='F';                                                                                                                                            ```
    ```      cnt=int(100*rand("uniform"))+1;                                                                                                                          ```
    ```      if rand("uniform")<.8 then output;                                                                                                                       ```
    ```      end;                                                                                                                                                     ```
    ```    end;                                                                                                                                                       ```
    ```    stop;                                                                                                                                                      ```
    ```  run;quit;                                                                                                                                                    ```
    ```                                                                                                                                                               ```
    ```  *                                                                                                                                                            ```
    ```    ___ ___  _ __ _ __ ___  ___ _ __                                                                                                                           ```
    ```   / __/ _ \| '__| '__/ _ \/ __| '_ \                                                                                                                          ```
    ```  | (_| (_) | |  | | |  __/\__ \ |_) |                                                                                                                         ```
    ```   \___\___/|_|  |_|  \___||___/ .__/                                                                                                                          ```
    ```                               |_|                                                                                                                             ```
    ```  ;                                                                                                                                                            ```
    ```                                                                                                                                                               ```
    ```  ods exclude all;                                                                                                                                             ```
    ```  ods output ObservedPct=xpopct(rename=(label=Sex ));                                                                                                          ```
    ```  ods output observed   =xpocnt(rename=label=Sex);                                                                                                             ```
    ```  ods output colprofiles=xpocolfrac(rename=(label=Sex));                                                                                                       ```
    ```  ods output rowprofiles=xporowfrac(rename=(label=Sex));                                                                                                       ```
    ```  ods output colprofilespct=xpocolpct(rename=(label=Sex));                                                                                                     ```
    ```  ods output rowprofilespct=xporowpct(rename=(label=Sex));                                                                                                     ```
    ```  proc corresp data=class all dimens=1 print=both;                                                                                                             ```
    ```   tables   sex, year;                                                                                                                                         ```
    ```   weight cnt;                                                                                                                                                 ```
    ```  run;quit;                                                                                                                                                    ```
    ```  ods select all;                                                                                                                                              ```
    ```                                                                                                                                                               ```
    ```  *                          _                                                                                                                                 ```
    ```   _ __ ___ _ __   ___  _ __| |_                                                                                                                               ```
    ```  | '__/ _ \ '_ \ / _ \| '__| __|                                                                                                                              ```
    ```  | | |  __/ |_) | (_) | |  | |_                                                                                                                               ```
    ```  |_|  \___| .__/ \___/|_|   \__|                                                                                                                              ```
    ```           |_|                                                                                                                                                 ```
    ```  ;                                                                                                                                                            ```
    ```                                                                                                                                                               ```
    ```  proc report data=class out=clsXpo(rename=(                                                                                                                   ```
    ```  _C2_ =_2014 _C3_ =_2015 _C4_ =_2016 _C5_ =_2017));                                                                                                           ```
    ```  cols sex year, (cnt) cnt=tot cnt=mean;                                                                                                                       ```
    ```  define sex / group;                                                                                                                                          ```
    ```  define year / across;                                                                                                                                        ```
    ```  define tot / computed sum "rowcnt";                                                                                                                          ```
    ```  define cnt / analysis sum '';                                                                                                                                ```
    ```  define mean / analysis mean 'mean';                                                                                                                          ```
    ```   rbreak after / summarize;                                                                                                                                   ```
    ```  run;quit;                                                                                                                                                    ```
    ```                                                                                                                                                               ```

