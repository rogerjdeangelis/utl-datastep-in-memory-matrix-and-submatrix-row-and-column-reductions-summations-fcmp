# utl-datastep-in-memory-matrix-and-submatrix-row-and-column-reductions-summations-fcmp
    %let pgm=utl-datastep-in-memory-matrix-and-submatrix-row-and-column-reductions-summations-fcmp;

    Datastep in memory matrix and submatrix row and column reductions summations fcmp

    %stop_submission;

    github
    http://tinyurl.com/2xhv2dcz
    https://github.com/rogerjdeangelis/utl-datastep-in-memory-matrix-and-submatrix-row-and-column-reductions-summations-fcmp

    The FCMP function can be simplified, you do not need 'varargs' and you  can rearrange the function arguments.

    Two Forms

    Function summation(rc[*,*],args $)

    More useful form when used with datastep do loops
    function summation(rc[*,*],rbeg,rend,cbeg,cend);

    /*               _     _
     _ __  _ __ ___ | |__ | | ___ _ __ ___
    | `_ \| `__/ _ \| `_ \| |/ _ \ `_ ` _ \
    | |_) | | | (_) | |_) | |  __/ | | | | |
    | .__/|_|  \___/|_.__/|_|\___|_| |_| |_|
    |_|
    */

    /**************************************************************************************************************************/
    /*                                    |                                                |                                  */
    /*  IN MEMORY ARRAY                   |    PROCESS                                     |        OUTPUT                    */
    /*                                    |                                                |                                  */
    /*  array observed[3,3] _temporary_   |    function summation(arg $,rc[*,*]) varargs;  |      +--------------+            */
    /*          (                         |       nrows=dim(rc,1);                         |      |   +-----+    |            */
    /*           1 2 3                    |       ncols=dim(rc,2);                         |      | 1 | 2 3 |  5 |            */
    /*           5 7 9                    |       rbeg=input(scan(arg,1,":"),best.);       |      | 5 | 7 9 | 16 |            */
    /*           0 4 5                    |       rend=input(scan(arg,2,":"),best.);       |      |   +-----+ 21 | sum        */
    /*           );                       |       cbeg=input(scan(arg,3,":"),best.);       |      | 0   4 5      | 2+3+7+9    */
    /*                                    |       cend=input(scan(arg,4,":"),best.);       |      +--------------+            */
    /*                                    |       total=0;                                 |                                  */
    /*                                    |        do i= rbeg to rend;                     |         subtotal=21              */
    /*                                    |          do j=cbeg to cend;                    |                                  */
    /*                                    |             total=total + rc[i,j];             |   summation('1:2:2:3',observed)  */
    /*                                    |          end;                                  |                                  */
    /*                                    |        end;                                    |    rows 1 and 2                  */
    /*                                    |    return(total);                              |    columns 2 and 3               */
    /*                                    |                                                |                                  */
    /*                                    |    subtotal = summation('1:2:2:3',observed);   |                                  */
    /*                                    |                                                |                                  */
    /*                                    |                                                |                                  */
    /**************************************************************************************************************************/

    /*                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */

    /*----                                                                   ----*/
    /*----  load in memoey array                                             ----*/
    /*----  create and save fcmp function                                    ----*/
    /*----                                                                   ----*/

    array observed[3,3] _temporary_
            (
             1 2 3
             5 7 9
             0 4 5
            );

    options cmplib=work.functions;

    proc fcmp outlib=work.functions.samples trace ;
    function summation(arg $,rc[*,*]) varargs;
       nrows=dim(rc,1);
       ncols=dim(rc,2);
       rbeg=input(scan(arg,1,":"),best.);
       rend=input(scan(arg,2,":"),best.);
       cbeg=input(scan(arg,3,":"),best.);
       cend=input(scan(arg,4,":"),best.);
       total=0;
        do i= rbeg to rend;
          do j=cbeg to cend;
             total=total + rc[i,j];
          end;
        end;
    return(total);
    endsub;
    run;quit;

    /*
     _ __  _ __ ___   ___ ___  ___ ___
    | `_ \| `__/ _ \ / __/ _ \/ __/ __|
    | |_) | | | (_) | (_|  __/\__ \__ \
    | .__/|_|  \___/ \___\___||___/___/
    |_|
    */

    /*----                                                                   ----*/
    /*---- FCMP is compiled and there is no default looping in the datastep  ----*/
    /*----                                                                   ----*/

    data want;
    array observed[3,3] _temporary_
            (
             1 2 3
             5 7 9
             0 4 5
            );

    grandtot = sum(of observed[*]);

    subtotal = summation('1:2:2:3',observed);
    put subtotal=;

    run;quit;

    /*           _               _
      ___  _   _| |_ _ __  _   _| |_
     / _ \| | | | __| `_ \| | | | __|
    | (_) | |_| | |_| |_) | |_| | |_
     \___/ \__,_|\__| .__/ \__,_|\__|
                    |_|
    */

    put subtotal=;

    SUBTOTAL=21

    /*
     _ __ ___ _ __   ___  ___
    | `__/ _ \ `_ \ / _ \/ __|
    | | |  __/ |_) | (_) \__ \
    |_|  \___| .__/ \___/|___/
             |_|
    */
    /*----                                                                   ----*/
    /*---- github is a development site                                      ----*/
    /*---- if you find issues with any repo please let me know               ----*/
    /*----                                                                   ----*/
    /*----                                                                   ----*/


    REPO
    -------------------------------------------------------------------------------------------------------------------------------
    https://github.com/rogerjdeangelis/utl-Computing-the-matrix-product-of-a-dataframe-with-its-transpose
    https://github.com/rogerjdeangelis/utl-creating-a-new-variable-that-contains-the-diagonal-elements-of-a-matrix
    https://github.com/rogerjdeangelis/utl-datastep-matrix-column-and-row-reductions
    https://github.com/rogerjdeangelis/utl-flatten-a-diagonal-bock-matrix-into-rows
    https://github.com/rogerjdeangelis/utl-fun-calculating-the-double-sum-matrix-operation
    https://github.com/rogerjdeangelis/utl-matrix-join-the-columns-of-table-a-with-the-rows-of-table-b
    https://github.com/rogerjdeangelis/utl-normalizing-the-proc-corr-output-correlation-matrix
    https://github.com/rogerjdeangelis/utl-rank-rows-of-a-matrix-without-looping
    https://github.com/rogerjdeangelis/utl-rowwise-element-by-elemet-mutiplication-of-a-vector-times-a-matrix
    https://github.com/rogerjdeangelis/utl-summarizing-a-matrix-over-rows-and-columns-simultaneously
    https://github.com/rogerjdeangelis/utl-suppress-high-correlations-in-the-a-correlation-matrix
    https://github.com/rogerjdeangelis/utl-transpose-and-create-a-state-matrix
    https://github.com/rogerjdeangelis/utl-vector-times-a-matrix-drop-down-from-sas-to-r
    https://github.com/rogerjdeangelis/utl_dot_product_of_a_small_matrix_and_a_small_vector_12_days_or_12_seconds
    https://github.com/rogerjdeangelis/utl_dot_product_of_a_vector_and_a_matrix_in_one_datastep
    https://github.com/rogerjdeangelis/utl_transposing_and_summarizing_a_matrix_that_lacks_row_identifiers
    https://github.com/rogerjdeangelis/utl_use_proc_r_for_matrix_dot_product_not_a_wps_or_sas_datastep
    https://github.com/rogerjdeangelis/utl_what_does_a_10_by_10_matrix_of_random_coin_flips_look_like
    https://github.com/rogerjdeangelis/utl-using-linear-regression-with-base-sas-and-r-to-interpolate-missimg-values


    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
Datastep in memory matrix and submatrix row and column reductions summations fcmp  
