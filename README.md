# utl-sum-the-product-of-coeficients-times-variables-using-sql-sas-r-python-multi-language
Sum the product of coeficients times variables using sql sas r python multi language 
    %let pgm=utl-sum-the-product-of-coeficients-times-variables-using-sql-sas-r-python-multi-language;

    %stop_submission;

    Sum the product of coeficients times variables using sql sas r python multi language

      SOLUTIONS

          1 sas sql
          2 r sql (same code as sas)
          3 python sql (same code as sas)
          4 r dot product
          5 stackoverflow solution (not run)


    github
    https://tinyurl.com/yzb6za7y
    https://github.com/rogerjdeangelis/utl-sum-the-product-of-coeficients-times-variables-using-sql-sas-r-python-multi-language

    related to
    https://tinyurl.com/5y8vjz8u
    https://stackoverflow.com/questions/79295472/summing-up-values-in-one-column-a-based-on-if-they-are-conditionally-marked-y

    /*               _     _
     _ __  _ __ ___ | |__ | | ___ _ __ ___
    | `_ \| `__/ _ \| `_ \| |/ _ \ `_ ` _ \
    | |_) | | | (_) | |_) | |  __/ | | | | |
    | .__/|_|  \___/|_.__/|_|\___|_| |_| |_|
    |_|
    */

    /**************************************************************************************************************************/
    /*                          |                                         |                                                   */
    /*      INPUT               |      PROCESS                            |        OUTPUT                                     */
    /*      =====               |      =======                            |        ======                                     */
    /*                          |                                         |                                                   */
    /*                          |                                         |                                                   */
    /*   RO C1 C2 C3 C4 C5      | USE RO=00 as COEFCIENTS                 |       RO    COUNT                                 */
    /*                          | TO APPLY COLUMNS c1-c5                  |                                                   */
    /*   00  1  1  1  1  1      |                                         |       01      1                                   */
    /*   01  0  0  0  1  0      | 00.01 =1                                |       02      3                                   */
    /*   02  1  1  0  1  0      | 00.02 =3                                |       03      0                                   */
    /*   03  0  0  0  0  0      | ..                                      |       04      2                                   */
    /*   04  1  0  0  0  1      | 00.12 =2                                |       05      1                                   */
    /*   05  1  0  0  0  0      |                                         |       06      0                                   */
    /*   06  0  0  0  0  0      | 01 = 1*0+1*0+1*0+1*1+1*0 = 1            |       07      3                                   */
    /*   07  0  1  1  0  1      | 02 = 1*1+1*1+1*0+1*1+1*0 = 3            |       08      3                                   */
    /*   08  0  1  1  1  0      | ...                                     |       09      2                                   */
    /*   09  0  1  0  0  1      | 12 = 1*1+1*1+1*0+1*0+1*0 = 2            |       10      1                                   */
    /*   10  0  0  0  1  0      |                                         |       11      0                                   */
    /*   11  0  0  0  0  0      |                                         |       12      2                                   */
    /*   12  1  1  0  0  0      | SAME CODE IN SAS, R and PYTHON SQL      |                                                   */
    /*                          | ==================================      |                                                   */
    /*                          |                                         |                                                   */
    /*                          |   select                                |                                                   */
    /*                          |        l.ro                             |                                                   */
    /*                          |       ,l.c1*r.c1                        |                                                   */
    /*                          |       +l.c2*r.c2                        |                                                   */
    /*                          |       +l.c3*r.c3                        |                                                   */
    /*                          |       +l.c4*r.c4                        |                                                   */
    /*                          |       +l.c5*r.c5 as count               |                                                   */
    /*                          |   from                                  |                                                   */
    /*                          |    sd1.have as l left join sd1.have as r|                                                   */
    /*                          |   on                                    |                                                   */
    /*                          |     r.ro  = '00'                        |                                                   */
    /*                          |   where                                 |                                                   */
    /*                          |     l.ro ne '00'                        |                                                   */
    /*                          | ;quit;                                  |                                                   */
    /*                          |                                         |                                                   */
    /*                          |---------------------------------------------------------------------------------------------*/
    /*                          |                                         |           |                                       */
    /*                          | R DOT PRODUCT                           | R         |  SAS                                  */
    /*                          | =============                           |           |                                       */
    /*                          |                                         |      V1   |   ROWNAMES    V1                      */
    /*                          | FIRST ROW MINUS FIRST COLUMN            |           |                                       */
    /*                          | vector<-as.matrix(read_sas(             |   1   1   |       1        1                      */
    /*                          |        "d:/sd1/have.sas7bdat")[1,-1]);  |   2   3   |       2        3                      */
    /*                          |                                         |   3   0   |       3        0                      */
    /*                          | REMAINING ROWS MINUS FIRST COLUMN       |   4   2   |       4        2                      */
    /*                          | matrix<-as.matrix(read_sas(             |   5   1   |       5        1                      */
    /*                          |        "d:/sd1/have.sas7bdat")[-1,-1]); |   6   0   |       6        0                      */
    /*                          |                                         |   7   3   |       7        3                      */
    /*                          | want <- as.data.frame(                  |   8   3   |       8        3                      */
    /*                          |   t(vector %*% t(matrix)));             |   9   2   |       9        2                      */
    /*                          |                                         |   10  1   |      10        1                      */
    /*                          |                                         |   11  0   |      11        0                      */
    /*                          |                                         |   12  2   |      12        2                      */
    /*                          |                                         |                                                   */
    /*                          |-----------------------------------------|---------------------------------------------------*/
    /*                          |                                         |                                                   */
    /*                          | R TIDYVERSE LANGUAGE (NOT RUN)          |                                                   */
    /*                          | ===============================         |                                                   */
    /*                          |                                         |                                                   */
    /*                          |  mmysum <- (                            |                                                   */
    /*                          |                                         |                                                   */
    /*                          |    relevant_months                      |                                                   */
    /*                          |    %>% summarise(                       |                                                   */
    /*                          |     across(everything(),                |                                                   */
    /*                          |        ~ (function (var,val) {          |                                                   */
    /*                          |                                         |                                                   */
    /*                          |            if (var!='mycount'){         |                                                   */
    /*                          |                                         |                                                   */
    /*                          |              sum(ifelse(val=='y'        |                                                   */
    /*                          |                  ,mycount,0)            |                                                   */
    /*                          |                  , na.rm = TRUE)        |                                                   */
    /*                          |            } else NA                    |                                                   */
    /*                          |           })(cur_column(), .)           |                                                   */
    /*                          |     )                                   |                                                   */
    /*                          |    )                                    |                                                   */
    /*                          |  )                                      |                                                   */
    /*                          |  rbind(                                 |                                                   */
    /*                          |    relevant_months,                     |                                                   */
    /*                          |    mysum                                |                                                   */
    /*                          |    )                                    |                                                   */
    /*                          |                                         |                                                   */
    /********************************************************************************|*****************************************/

    /*                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */

    options validvarname=upcase;
    libname sd1 "d:/sd1";
    data sd1.have;
       input ro $3. (c1-c5)( 5*2.);
    cards4;
    00 1 1 1 1 1
    01 0 0 0 1 0
    02 1 1 0 1 0
    03 0 0 0 0 0
    04 1 0 0 0 1
    05 1 0 0 0 0
    06 0 0 0 0 0
    07 0 1 1 0 1
    08 0 1 1 1 0
    09 0 1 0 0 1
    10 0 0 0 1 0
    11 0 0 0 0 0
    12 1 1 0 0 0
    ;;;;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*                                                                                                                        */
    /*     RO    C1    C2    C3    C4    C5                                                                                   */
    /*                                                                                                                        */
    /*     00     1     1     1     1     1                                                                                   */
    /*     01     0     0     0     1     0                                                                                   */
    /*     02     1     1     0     1     0                                                                                   */
    /*     03     0     0     0     0     0                                                                                   */
    /*     04     1     0     0     0     1                                                                                   */
    /*     05     1     0     0     0     0                                                                                   */
    /*     06     0     0     0     0     0                                                                                   */
    /*     07     0     1     1     0     1                                                                                   */
    /*     08     0     1     1     1     0                                                                                   */
    /*     09     0     1     0     0     1                                                                                   */
    /*     10     0     0     0     1     0                                                                                   */
    /*     11     0     0     0     0     0                                                                                   */
    /*     12     1     1     0     0     0                                                                                   */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*                             _
    / |  ___  __ _ ___   ___  __ _| |
    | | / __|/ _` / __| / __|/ _` | |
    | | \__ \ (_| \__ \ \__ \ (_| | |
    |_| |___/\__,_|___/ |___/\__, |_|
                                |_|
    */
    proc sql;
      create
         table want as
      select
           l.ro
          ,l.c1*r.c1
          +l.c2*r.c2
          +l.c3*r.c3
          +l.c4*r.c4
          +l.c5*r.c5 as count
      from
        sd1.have as l left join sd1.have as r
      on
        r.ro  = '00'
      where
        l.ro ne '00'
    ;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*     RO    COUNT                                                                                                        */
    /*                                                                                                                        */
    /*     01      1                                                                                                          */
    /*     02      3                                                                                                          */
    /*     03      0                                                                                                          */
    /*     04      2                                                                                                          */
    /*     05      1                                                                                                          */
    /*     06      0                                                                                                          */
    /*     07      3                                                                                                          */
    /*     08      3                                                                                                          */
    /*     09      2                                                                                                          */
    /*     10      1                                                                                                          */
    /*     11      0                                                                                                          */
    /*     12      2                                                                                                          */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*___                     _
    |___ \   _ __   ___  __ _| |
      __) | | `__| / __|/ _` | |
     / __/  | |    \__ \ (_| | |
    |_____| |_|    |___/\__, |_|
                           |_|
    */

    %utl_rbeginx;
    parmcards4;
    library(haven)
    library(sqldf)
    source("c:/oto/fn_tosas9x.R")
    have     <-read_sas("d:/sd1/have.sas7bdat")
    want<-sqldf('
      select
           l.ro
          ,l.c1*r.c1
          +l.c2*r.c2
          +l.c3*r.c3
          +l.c4*r.c4
          +l.c5*r.c5 as count
      from
        have as l left join have as r
      on
        r.ro  = "00"
      where
        l.ro <> "00"
    ')
    want
    fn_tosas9x(
          inp    = want
         ,outlib ="d:/sd1/"
         ,outdsn ="rwant"
         )
    ;;;;
    %utl_rendx;

    proc print data=sd1.rwant;
    run;quit;

    /**************************************************************************************************************************/
    /*              |                                                                                                         */
    /*  R           | SAS                                                                                                     */
    /*              |                                                                                                         */
    /*   RO count   | ROWNAMES    RO    COUNT                                                                                 */
    /*              |                                                                                                         */
    /*   01     1   |     1       01      1                                                                                   */
    /*   02     3   |     2       02      3                                                                                   */
    /*   03     0   |     3       03      0                                                                                   */
    /*   04     2   |     4       04      2                                                                                   */
    /*   05     1   |     5       05      1                                                                                   */
    /*   06     0   |     6       06      0                                                                                   */
    /*   07     3   |     7       07      3                                                                                   */
    /*   08     3   |     8       08      3                                                                                   */
    /*   09     2   |     9       09      2                                                                                   */
    /*   10     1   |    10       10      1                                                                                   */
    /*   11     0   |    11       11      0                                                                                   */
    /*   12     2   |    12       12      2                                                                                   */
    /*              |                                                                                                         */
    /**************************************************************************************************************************/

    /*____               _   _                             _
    |___ /   _ __  _   _| |_| |__   ___  _ __    ___  __ _| |
      |_ \  | `_ \| | | | __| `_ \ / _ \| `_ \  / __|/ _` | |
     ___) | | |_) | |_| | |_| | | | (_) | | | | \__ \ (_| | |
    |____/  | .__/ \__, |\__|_| |_|\___/|_| |_| |___/\__, |_|
            |_|    |___/                                |_|
    */

    proc datasets lib=sd1 nolist nodetails;
     delete pywant;
    run;quit;

    %utl_pybeginx;
    parmcards4;
    exec(open('c:/oto/fn_python.py').read());
    have,meta = ps.read_sas7bdat('d:/sd1/have.sas7bdat');
    want=pdsql('''
         select
              l.ro
             ,l.c1*r.c1
             +l.c2*r.c2
             +l.c3*r.c3
             +l.c4*r.c4
             +l.c5*r.c5 as count
         from
           have as l left join have as r
         on
           r.ro  = "00"
         where
           l.ro <> "00"
       ''');
    print(want);
    fn_tosas9x(want,outlib='d:/sd1/',outdsn='pywant',timeest=3);
    ;;;;
    %utl_pyendx;

    proc print data=sd1.pywant;
    run;quit;

    /**************************************************************************************************************************/
    /*                 |                                                                                                      */
    /* PYTHON          | SAS                                                                                                  */
    /*                 |                                                                                                      */
    /*      RO  count  | Obs    RO    COUNT                                                                                   */
    /*                 |                                                                                                      */
    /*  0   01    1.0  |   1    01      1                                                                                     */
    /*  1   02    3.0  |   2    02      3                                                                                     */
    /*  2   03    0.0  |   3    03      0                                                                                     */
    /*  3   04    2.0  |   4    04      2                                                                                     */
    /*  4   05    1.0  |   5    05      1                                                                                     */
    /*  5   06    0.0  |   6    06      0                                                                                     */
    /*  6   07    3.0  |   7    07      3                                                                                     */
    /*  7   08    3.0  |   8    08      3                                                                                     */
    /*  8   09    2.0  |   9    09      2                                                                                     */
    /*  9   10    1.0  |  10    10      1                                                                                     */
    /*  10  11    0.0  |  11    11      0                                                                                     */
    /*  11  12    2.0  |  12    12      2                                                                                     */
    /*                 |                                                                                                      */
    /**************************************************************************************************************************/

    /*  _                _       _                         _            _
    | || |    _ __    __| | ___ | |_   _ __  _ __ ___   __| |_   _  ___| |_
    | || |_  | `__|  / _` |/ _ \| __| | `_ \| `__/ _ \ / _` | | | |/ __| __|
    |__   _| | |    | (_| | (_) | |_  | |_) | | | (_) | (_| | |_| | (__| |_
       |_|   |_|     \__,_|\___/ \__| | .__/|_|  \___/ \__,_|\__,_|\___|\__|
                                      |_|
    */

    proc datasets lib=sd1 nodetails nolist;
     delete rrwant;
    run;quit;

    %utl_submit_r64x('
    library(haven);
    source("c:/oto/fn_tosas9x.R");
    vector<-as.matrix(read_sas(
      "d:/sd1/have.sas7bdat")[1,-1]);
    matrix<-as.matrix(read_sas(
      "d:/sd1/have.sas7bdat")[-1,-1]);
    want <- as.data.frame(
      t(vector %*% t(matrix)));
    want;
    fn_tosas9x(
          inp    = want
         ,outlib ="d:/sd1/"
         ,outdsn ="rrwant"
         );
    ');


    proc print data=sd1.rrwant;
    run;quit;

    /**************************************************************************************************************************/
    /*            |                                                                                                           */
    /*  R         |  SAS                                                                                                      */
    /*       V1   |   ROWNAMES    V1                                                                                          */
    /*            |                                                                                                           */
    /*    1   1   |       1        1                                                                                          */
    /*    2   3   |       2        3                                                                                          */
    /*    3   0   |       3        0                                                                                          */
    /*    4   2   |       4        2                                                                                          */
    /*    5   1   |       5        1                                                                                          */
    /*    6   0   |       6        0                                                                                          */
    /*    7   3   |       7        3                                                                                          */
    /*    8   3   |       8        3                                                                                          */
    /*    9   2   |       9        2                                                                                          */
    /*    10  1   |      10        1                                                                                          */
    /*    11  0   |      11        0                                                                                          */
    /*    12  2   |      12        2                                                                                          */
    /*            |                                                                                                           */
    /**************************************************************************************************************************/

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */

