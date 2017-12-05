# utl_hex_dump_of_fixed_record_formated_file_or_importing_a_text_file_with_no_headers
I need to check the file against the layout. I am assumming no delimiter and no end of record. File has back to back 292 byte records. Note macro utlrulr can handle any record format.
    Hex dump of fixed record formated file or importing a text file with no headers

    for macro utlrulr see
    https://github.com/rogerjdeangelis/utl_hex_dump_of_complex_text_file

    I need to check the file against the layout.
    I am assumming no delimiter and no end of record. File has back to back
    292 byte records. Note macro utlrulr can handle any record format.

    INPUT
    =====

      Fixed length file. Columns in fixed positions, no delimiters, and no end of record characters.

      12345678901234567
                                                             57                            87
      VARCHAR2(17Byte) VARCHAR2(39Byte)                       VARCHAR2(30Byte)              VARCHAR2(33Byte) ...

    PROCESS
    =======

       %utlrulr
          (
           uinflt =d:/txt/fixed.txt,
           uprnlen =100,    /* Linesize for Dump */
           ulrecl  =100,    /* maximum record length */
           urecfm  =f,
           uobs = 3,        /* number of obs to dump */
           uchrtyp =ascii,  /* ascii or ebcdic */
           uotflt =d:\txt\fixed_hex.txt
          );

    OUTPUT
    ======

       ASCII Flatfile Ruler & Hex
       utlrulr
       d:/txt/fixed.txt
       d:\txt\fixed_hex.txt


        --- Record Number ---  1   ---  Record Length ---- 100

                       18                                     57                            87
       VARCHAR2(17Byte) VARCHAR2(39Byte)                       VARCHAR2(30Byte)              VARCHAR2(33Byt
       1...5....10...15...20...25...30...35...40...45...50...55...60...65...70...75...80...85...90...95...1
       5454445323347762254544453233477622222222222222222222222254544453233477622222222222222254544453233477
       6123812281729459061238122839294590000000000000000000000061238122830294590000000000000061238122833294


        --- Record Number ---  2   ---  Record Length ---- 100

       e)                 A3 B4  VARCHAR2(12BVARCHAR2(19Byte)   VARCHAR2(19Byte)   VARCHAR2(21Byte)     VAR
       1...5....10...15...20...25...30...35...40...45...50...55...60...65...70...75...80...85...90...95...1
       6222222222222222222432432254544453233454544453233477622225454445323347762222545444532334776222222545
       5900000000000000000130240061238122812261238122819294590006123812281929459000612381228212945900000612


        --- Record Number ---  3   ---  Record Length ---- 92

       CHAR2(50Byte)                                  C8      D3 E3 F8      G3 H3 I3 J3 K3 L8     292
       1...5....10...15...20...25...30...35...40...45...50...55...60...65...70...75...80...85...90.
       44453233477622222222222222222222222222222222222432222224324324322222243243243243243243222222
       381228502945900000000000000000000000000000000003800000043053068000000730830930A30B30C8000000


    https://goo.gl/uqPw1J
    https://communities.sas.com/t5/SAS-Procedures/importing-a-text-file-with-no-headers/m-p/418323

    *                _               _       _
     _ __ ___   __ _| | _____     __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \   / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/  | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|   \__,_|\__,_|\__\__,_|

    ;

    proc sql;
      create
        table have as
      select
       'VARCHAR2(17Byte)'  as  CONTACT_ID          length=17
      ,'VARCHAR2(39Byte)'  as  COMPANY_NAME        length=39
      ,'VARCHAR2(30Byte)'  as  FIRST_NAME          length=30
      ,'VARCHAR2(33Byte)'  as  LAST_NAME           length=33
      ,'A3'                as  GENDER              length=3
      ,'B4'                as  PREFERED_LANGUAGE   length=4
      ,'VARCHAR2(12Byte)'  as  MARITAL_STATUS      length=12
      ,'VARCHAR2(19Byte)'  as  HOME_PHONE          length=19
      ,'VARCHAR2(19Byte)'  as  CELL_PHONE          length=19
      ,'VARCHAR2(21Byte)'  as  BUISINESS_PHONE     length=21
      ,'VARCHAR2(50Byte)'  as  EMAIL               length=50
      ,'C8'                as  DT_BIRTH            length=8
      ,'D3'                as  VIP_INDICATOR       length=3
      ,'E3'                as  STAFF_INDICATOR     length=3
      ,'F8'                as  CLIENT_SINCE_DATE   length=8
      ,'G3'                as  DNSH                length=3
      ,'H3'                as  HOME_DNCL           length=3
      ,'I3'                as  CELL_DNCL           length=3
      ,'J3'                as  BUSINESS_DNCL       length=3
      ,'K3'                as  DNSO                length=3
      ,'L8'                as  DT_TO_DATE          length=8

     from
      sashelp.class(obs=1);
    run;quit;


    data _null_;
       file "d:/txt/fixed.txt" lrecl=292 recfm=f;
       set have;
       put
         CONTACT_ID          $char17.
         COMPANY_NAME        $char39.
         FIRST_NAME          $char30.
         LAST_NAME           $char33.
         GENDER              $char3.
         PREFERED_LANGUAGE   $char4.
         MARITAL_STATUS      $char12.
         HOME_PHONE          $char19.
         CELL_PHONE          $char19.
         BUISINESS_PHONE     $char21.
         EMAIL               $char50.
         DT_BIRTH            $char8.
         VIP_INDICATOR       $char3.
         STAFF_INDICATOR     $char3.
         CLIENT_SINCE_DATE   $char8.
         DNSH                $char3.
         HOME_DNCL           $char3.
         CELL_DNCL           $char3.
         BUSINESS_DNCL       $char3.
         DNSO                $char3.
         DT_TO_DATE          $char8.
       ;
    run;quit;

    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __
    / __|/ _ \| | | | | __| |/ _ \| '_ \
    \__ \ (_) | | |_| | |_| | (_) | | | |
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|

    ;

     %utlrulr
          (
           uinflt =d:/txt/fixed.txt,
           uprnlen =100,    /* Linesize for Dump */
           ulrecl  =100,    /* maximum record length */
           urecfm  =f,
           uobs = 3,        /* number of obs to dump */
           uchrtyp =ascii,  /* ascii or ebcdic */
           uotflt =d:\txt\fixed_hex.txt
          );


