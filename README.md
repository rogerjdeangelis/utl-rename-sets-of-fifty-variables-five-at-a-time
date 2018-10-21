# utl-rename-sets-of-fifty-variables-five-at-a-time
Rename sets of 50 variables 5 at a time.

    Rename sets of 50 variables 5 at a time

    github
    https://tinyurl.com/yb24shwt
    https://github.com/rogerjdeangelis/utl-rename-sets-of-fifty-variables-five-at-a-time

    StackOverflow
    https://stackoverflow.com/questions/52902403/sas-macro-rename-variables


    INPUT ( 50 Variables x1-x50
    ===========================

    array xs[50] x1-x50;
    array alphas[50] $1 A1-A5 B1-B5 C1-C5 D1-D5 E1-E5 F1-F5 G1-G5 H1-H5 I1-I5 J1-J5;

    Up to 40 obs from WORK.HAVE total obs=1

     X1 X2 X3 X4 X5   X6 X7 X8 X9 X10  ... X46 X47 X48 X49 X50
      1  1  1  1  1    1  1  1  1   1        1   1   1   1   1

    WORK.HAVE - Total Obs 1

                         |  RULES
     -- NUMERIC --       |  RENAME X1-X50 using sets of 5

    X1       N8       1  |  A1
    X2       N8       1  |  A2
    X3       N8       1  |  A3
    X4       N8       1  |  A4
    X5       N8       1  |  A5

    X6       N8       1  |  B1
    X7       N8       1  |  B2
    X8       N8       1  |  B3
    X9       N8       1  |  B4
    X10      N8       1  |  B5
    ...                    ...

    X46      N8       1  |  J1
    X47      N8       1  |  J2
    X48      N8       1  |  J3
    X49      N8       1  |  J4
    X50      N8       1  |  J5


    EXAMPLE OUTPUT
    ==============

     A1 A2 A3 A4 A5   B6 B7 B8 B9 B10  ... J46 J47 J48 J49 J50
      1  1  1  1  1    1  1  1  1   1        1   1   1   1   1

     -- NUMERIC --
    A1       N8       1
    A2       N8       1
    A3       N8       1
    A4       N8       1
    A5       N8       1

    B1       N8       1
    B2       N8       1
    B3       N8       1
    B4       N8       1
    B5       N8       1
    ...

    B46      N8       1
    B47      N8       1
    B48      N8       1
    B49      N8       1
    B50      N8       1


    PROCESS
    =======

    data _null_;
      length rens $32756;
      array xs[50] x1-x50;
      array alphas[50] $1 A1-A5 B1-B5 C1-C5 D1-D5 E1-E5 F1-F5 G1-G5 H1-H5 I1-I5 J1-J5;
      set have(obs=1);
      do idx=1 to 50;
         rens=catx(' ',rens,vname(xs[idx]),'=',vname(alphas[idx]));
      end;
      drop idx;
      call symputx('rens',rens);
    run;quit;

    %put &=rens;

    /*
    RENS=  X1=A1   X2=A2  X3=A3  X4=A4  X5=A5
           ....
           X46=J1 X47=J2 X48=J3 X49=J4 X50=J5
    */

    proc datasets lib=work;
    modify have;
    rename &rens;
    run;quit

    OUTPUT
    ======

    see above

    *                _               _       _
     _ __ ___   __ _| | _____     __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \   / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/  | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|   \__,_|\__,_|\__\__,_|

    ;

    data have;
      array xs[50] x1-x50 (50*1);
    run;quit;

