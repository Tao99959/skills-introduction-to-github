*&---------------------------------------------------------------------*
*& Report ZTEST_D00190_TEST7
*&---------------------------------------------------------------------*
*&
*&---------------------------------------------------------------------*
REPORT ZTEST_D00190_TEST7.

TYPES:
  BEGIN OF typ_tab,
    col0 type char1,
    col1 type char1,
    col2 type i,
  END OF typ_tab,
  typ_t_tab TYPE STANDARD TABLE OF typ_tab WITH EMPTY KEY.

*  DATA w_sum type i.
*  DATA w_max type i.

  DATA(lt_tab) = VALUE typ_t_tab( ( col0 = 'X' col1 = 'A' col2 = 1 )
                                  ( col0 = 'X' col1 = 'B' col2 = 2 )
                                  ( col0 = 'X' col1 = 'C' col2 = 3 )
                                  ( col0 = 'X' col1 = 'A' col2 = 4 )
                                  ( col0 = 'X' col1 = 'B' col2 = 5 )
                                  ( col0 = 'Z' col1 = 'B' col2 = 6 )
                                  ( col0 = 'Z' col1 = 'C' col2 = 7 )
                                  ( col0 = 'Z' col1 = 'C' col2 = 8 )
                                  ( col0 = 'Z' col1 = 'A' col2 = 9 ) ).

  LOOP AT lt_tab INTO DATA(wa_tab) GROUP BY ( key1 = wa_tab-col1 )
                 ASSIGNING FIELD-SYMBOL(<fs_tab>).

    WRITE: / <fs_tab>-key1, ' list:'.

*    CLEAR w_sum.
*    LOOP AT GROUP <fs_tab> INTO DATA(wa_group).
*      w_sum = w_sum + wa_group-col2.
*      WRITE / wa_group-col2.
*    ENDLOOP.

    DATA(w_max) = REDUCE #( INIT i type i
                            FOR <lfs_max> in GROUP <fs_tab>
                            NEXT i = nmax( val1 = i val2 = <lfs_max>-col2 ) ).
    DATA(w_sum) = REDUCE #( INIT i TYPE i
                            FOR <lfs_sum> in GROUP <fs_tab>
                            NEXT i = i + <lfs_sum>-col2 ).

    WRITE: / sy-uline,
           / 'Max: ', w_max,
           / 'Sum: ', w_sum.
  ENDLOOP.
