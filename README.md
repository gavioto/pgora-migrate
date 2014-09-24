NAME
    pgORA-Migrate - Oracle to PostgreSQL database schema converter

DESCRIPTION
    pgORA-Migrate is a free tool used to migrate an Oracle database to a PostgreSQL
    compatible schema. It connects your Oracle database, scan it
    automatically and extracts its structure or data, it then generates SQL
    scripts that you can load into your PostgreSQL database.

    pgORA-Migrate can be used from reverse engineering Oracle database to huge
    enterprise database migration or simply to replicate some Oracle data
    into a PostgreSQL database. It is really easy to used and doesn't need
    any Oracle database knowledge than providing the parameters needed to
    connect to the Oracle database.

FEATURES
    pgORA-Migrate consist of a Perl script (pgora-migrate) and a Perl module (pgORA-Migrate.pm),
    the only thing you have to modify is the configuration file pgora-migrate.conf
    by setting the DSN to the Oracle database and optionaly the name of a
    schema. Once that's done you just have to set the type of export you
    want: TABLE with constraints, VIEW, MVIEW, TABLESPACE, SEQUENCE,
    INDEXES, TRIGGER, GRANT, FUNCTION, PROCEDURE, PACKAGE, PARTITION, TYPE,
    INSERT or COPY, FDW, QUERY, KETTLE .

    By default pgORA-Migrate exports to a file that you can load into PostgreSQL
    with the psql client, but you can also import directly into a PostgreSQL
    database by setting its DSN into the configuration file. With all
    configuration options of pgora-migrate.conf you have full control of what
    should be exported and how.

    Features included:

            - Export full database schema (tables, views, sequences, indexes), with
              unique, primary, foreign key and check constraints.
            - Export grants/privileges for users and groups.
            - Export range and list partition.
            - Export a table selection (by specifying the table names).
            - Export Oracle schema to a PostgreSQL 8.4+ schema.
            - Export predefined functions, triggers, procedures, packages and
              package bodies.
            - Export full data or following a WHERE clause.
            - Full support of Oracle BLOB object as PG BYTEA.
            - Export Oracle views as PG tables.
            - Export Oracle user defined types.
            - Provide some basic automatic conversion of PLSQL code to PLPGSQL.
            - Works on any plateform.
            - Export Oracle tables as foreign data wrapper tables.
            - Export materialized view.
            - Show a detailled report of an Oracle database content.
            - Migration cost assessment of an Oracle database.
            - Migration cost assessment of PL/SQL code from a file.
            - Migration cost assessment of Oracle SQL queries stored in a file.
            - Generate XML ktr files to be used with Penthalo Data Integrator (Kettle)

    pgORA-Migrate do its best to automatically convert your Oracle database to
    PostgreSQL but there's still manual works to do. The Oracle specific
    PL/SQL code generated for functions, procedures, packages and triggers
    has to be reviewed to match the PostgreSQL syntax. You will find some
    useful recommandations on porting Oracle PL/SQL code to PostgreSQL
    PL/PGSQL at "Converting from other Databases to PostgreSQL", section:
    Oracle (http://wiki.postgresql.org/wiki/Main_Page).

    See http://pgora-migrate.darold.net/report.html for a HTML sample of an Oracle
    database migration report.

INSTALLATION
    All Perl modules can always be found at CPAN (http://search.cpan.org/).
    Just type the full name of the module (ex: DBD::Oracle) into the search
    input box, it will brings you the page for download.

    Releases of pgORA-Migrate stay at SF.net
    (https://sourceforge.net/projects/pgora-migrate/).

    Under Windows you should install Strawberry Perl
    (http://strawberryperl.com/) and the OSes corresponding Oracle clients.
    It seems that compiling DBD::Oracle from CPAN on Windows can be a
    struggle and there be little documentation on that (mostly outdated and
    not working). Installing the free version of ActiveState Perl
    (http://www.activestate.com/activeperl) could help as they seems to have
    an already packaged DBD::Oracle easy to install.

  Requirement
    You need a modern Perl distribution (perl 5.6 and more), the DBI and
    DBD::Oracle Perl modules to be installed. These are used to connect to
    the Oracle database. To install DBD::Oracle and have it working you need
    to have the Oracle client libraries installed and the ORACLE_HOME
    environment variable must be defined.

    On old Perl version you may need to install the Time::HiRes Perl module.

  Optional
    By default pgORA-Migrate dumps export to flat files, to load them into your
    PostgreSQL database you need the PostgreSQL client (psql). If you don't
    have it on the host running pgORA-Migrate you can always transfer these files
    to a host with the psql client installed. If you prefer to load export
    'on the fly', the perl module DBD::Pg is required.

    pgORA-Migrate allow to dump all output int a compressed gzip file, to do that
    you need the Compress::Zlib Perl module or if you prefer using bzip2
    compression, the program bzip2 must be available in your PATH.

  Installing pgORA-Migrate
    Like any other Perl Module pgORA-Migrate can be installed with the following
    commands:

            tar xzf pgora-migrate-10.x.tar.gz
            cd pgora-migrate-10.x/
            perl Makefile.PL
            make && make install

    This will install pgORA-Migrate.pm into your site Perl repository, pgora-migrate into
    /usr/local/bin/ and pgora-migrate.conf into /etc/pgora-migrate/.

    On Windows(tm) OSes you may use instead:

            perl Makefile.PL
            dmake && dmake install

    This will install scripts and libraries into your Perl site installation
    directory and the pgora-migrate.conf file as well as all documentation files
    into C:\pgora-migrate\

  Packaging
    If you want to build binary package for your preferred Linux
    distribution take a look at the packaging/ directory of the source
    tarball. There's everything to build RPM, Slackware and Debian packages.
    See README file in that directory.

  Installing DBD::Oracle
    pgORA-Migrate need perl module DBD::Oracle for connectivity to an Oracle
    database from perl DBI. To get DBD::Oracle get it from CPAN a perl
    module repository.

    After setting ORACLE_HOME and LD_LIBRARY_PATH environment variables as
    root user, install DBD::Oracle. Proceed as follow:

            export LD_LIBRARY_PATH=/usr/lib/oracle/11.2/client64/lib
            export ORACLE_HOME=/usr/lib/oracle/11.2/client64/
            perl -MCPAN -e 'install DBD::Oracle'

    If you are running for the first time it will ask so many questions
    which you can keep defaults by pressing ENTER key, but you need to give
    one appropriate i.e., mirror site for CPAN to download the modules. Or
    install through CPAN manually if the above don't works:

            #perl -MCPAN -e shell
            cpan> get DBD::Oracle
            cpan> quit
            cd ~/.cpan/build/DBD-Oracle*
            export LD_LIBRARY_PATH=/usr/lib/oracle/11.2/client64/lib
            export ORACLE_HOME=/usr/lib/oracle/11.2/client64/
            perl Makefile.PL
            make
            make install

    Installing DBD::Oracle require that the three Oracle packages:
    instant-client, SDK and SQLplus are installed.

CONFIGURATION
    pgORA-Migrate configuration can be as simple as choose the Oracle database to
    export and choose the export type. This can be done in the minute.

    By reading this documentation you will also be able to:

            - Select only certain tables and/or column for export.
            - Rename some tables and/or column during export.
            - Select data to export following a WHERE clause per table.
            - Delay database constraints during data loading.
            - Compress exported data to save disk space.
            - and much more.

    The full control of the Oracle database migration is taken though a
    single configuration file named pgora-migrate.conf. The format of this file
    consist in a directive name in upper case followed by tab character and
    a value. Comments are lines beginning with a #.

  pgORA-Migrate usage
    By default pgORA-Migrate will look for /etc/pgora-migrate/pgora-migrate.conf configuration
    file, if the file exist you can simply execute:

            /usr/local/bin/pgora-migrate

    or under Windows(tm) run pgora-migrate.bat file, located in your perl bin
    directory. Windows(tm) users may also find a template configuration file
    in C:\pgora-migrate

    If you want to call another configuration file, just give the path as
    command line argument:

            /usr/local/bin/pgora-migrate -c /etc/pgora-migrate/new_pgora-migrate.conf

    Here are all command line parameters available when using pgora-migrate:

      Usage: pgora-migrate [-dhpqv --estimate_cost --dump_as_html] [--option value]

        -a | --allow str  : comma-separated list of objects to allow from export.
                            Can be used with SHOW_COLUMN too.
        -b | --basedir dir: Used to set the default output directory, where files
                            resulting from exports will be stored.
        -c | --conf file  : Used to set an alternate configuration file than the
                            default /etc/pgora-migrate/pgora-migrate.conf.
        -d | --debug      : Enable verbose output.
        -e | --exclude str: comma-separated list of objects to exclude from export.
                            Can be used with SHOW_COLUMN too.
        -h | --help       : Print this short help.
        -i | --input file : File containing Oracle PL/SQL code to convert with
                            no Oracle database connection initiated.
        -j | --jobs num   : number of parallel process to send data to PostgreSQL.
        -J | --copies num : number of parallel connection to extract data from Oracle.
        -l | --log file   : Used to set a log file. Default is stdout.
        -L | --limit num  : number of tuples extracted from Oracle and stored in
                            memory before writing, default: 10000.
        -n | --namespace schema : Used to set the Oracle schema to extract from.
        -o | --out file   : Used to set the path to the output file where SQL will
                            be written. Default: output.sql in running directory.
        -p | --plsql      : Enable PLSQL to PLPSQL code conversion.
        -P | --parallel num: Number of parallel tables to extract at the same time.
        -q | --quiet      : disable progress bar.
        -s | --source DSN : Allow to set the Oracle DBI datasource.
        -t | --type export: Used to set the export type. It will override the one
                            given in the configuration file (TYPE).
        -u | --user name  : Used to set the Oracle database connection user.
        -v | --version    : Show pgORA-Migrate Version and exit.
        -w | --password pwd : Used to set the password of the Oracle database user.
        --forceowner: if set to 1 force pgora-migrate to set tables and sequences owner
                      like in Oracle database. If the value is set to a username this
                      one will be used as the objects owner. By default it's the user
                      used to connect to the Pg database that will be the owner.
        --nls_lang code: use this to set the Oracle NLS_LANG client encoding.
        --client_encoding code: Use this to set the PostgreSQL client encoding.
        --view_as_table str: comma-separated list of view to export as table.
        --estimate_cost   : activate the migration cost evalution with SHOW_REPORT
        --cost_unit_value minutes: number of minutes for a cost evalution unit.
                      default: 5 minutes, correspond to a migration conducted by a
                      PostgreSQL expert. Set it to 10 if this is your first migration.
       --dump_as_html     : force pgora-migrate to dump report in HTML, used only with
                            SHOW_REPORT. Default is to dump report as simple text.
       --init_project NAME: initialise a typical pgora-migrate project tree. Top directory
                            will be created under project base dir.
       --project_base DIR : define the base dir for pgora-migrate project trees. Default
                            is current directory.

    It is possible to add your own custom option(s) in the Perl script
    pgora-migrate as any configuration directive from pgora-migrate.conf can be passed in
    lower case to the new pgORA-Migrate object instance. See pgora-migrate code on how to
    add your own option.

  Generate a migration template
    The two options --project_base and --init_project when used indicate to
    pgora-migrate that he has to create a project template with a work tree, a
    configuration file and a script to export all objects from the Oracle
    database. Here a sample of the command usage:

        pgora-migrate --project_base /home/git/tmp --init_project test_project

            Creating project test_project.
            /home/git/tmp/test_project/
                    schema/
                            fdws/
                            functions/
                            grants/
                            kettles/
                            mviews/
                            packages/
                            partitions/
                            procedures/
                            sequences/
                            tables/
                            tablespaces/
                            triggers/
                            types/
                            views/
                    sources/
                            functions/
                            mviews/
                            packages/
                            partitions/
                            procedures/
                            triggers/
                            types/
                            views/
                    data/
                    config/
                    reports/

            Generating generic configuration file
            Creating script export_schema.sh to automate all exports.

    It create a generic config file where you just have to define the Oracle
    database connection and a shell script called export_schema.sh.

  Oracle database connection
    There's 5 configuration directives to control the access to the Oracle
    database.

    ORACLE_HOME
        Used to set ORACLE_HOME environment variable to the Oracle libraries
        required by the DBD::Oracle Perl module.

    ORACLE_DSN
        This directive is used to set the data source name in the form
        standard DBI DSN. For example:

                dbi:Oracle:host=oradb_host.myhost.com;sid=DB_SID

        or

                dbi:Oracle:DB_SID

        for the second notation the SID should be declared in the well known
        file $ORACLE_HOME/network/admin/tnsnames.ora.

    ORACLE_USER et ORACLE_PWD
        These two directives are used to define the user and password for
        the Oracle database connection. Note that if you can it is better to
        login as Oracle super admin to avoid grants problem during the
        database scan and be sure that nothing is missing.

    USER_GRANTS
        Set this directive to 1 if you connect the Oracle database as simple
        user and do not have enough grants to extract things from the
        DBA_... tables. It will use tables ALL_... instead.

        Warning: if you use export type GRANT, you must set this
        configuration option to 0 or it will not works.

    TRANSACTION
        This directive may be used if you want to change the default
        isolation level of the data export transaction. Default is now to
        set the level to a serializable transaction to ensure data
        consistency. The allowed values for this directive are:

                readonly: 'SET TRANSACTION READ ONLY',
                readwrite: 'SET TRANSACTION READ WRITE',
                serializable: 'SET TRANSACTION ISOLATION LEVEL SERIALIZABLE'
                committed: 'SET TRANSACTION ISOLATION LEVEL READ COMMITTED',

        Releases before 6.2 used to set the isolation level to READ ONLY
        transaction but in some case this was breaking data consistency so
        now default is set to SERIALIZABLE.

    INPUT_FILE
        This directive did not control the Oracle database connection or
        unless it purely disable the use of any Oracle database by accepting
        a file as argument. Set this directive to a file containing PL/SQL
        Oracle Code like function, procedure or full package body to prevent
        pgORA-Migrate from connecting to an Oracle database and just apply his
        convertion tool to the content of the file. This can be used with
        the most of export types: TABLE, TRIGGER, PROCEDURE, VIEW, FUNCTION
        or PACKAGEi, etc.

  Data encryption with Oracle server
    If your Oracle Client config file already includes the encryption
    method, then DBD:Oracle uses those settings to encrypt the connection
    while you extract the data. For example if you have configured the
    Oracle Client config file (sqlnet.or or .sqlnet) with the following
    information:

            # Configure encryption of connections to Oracle
            SQLNET.ENCRYPTION_CLIENT = required
            SQLNET.ENCRYPTION_TYPES_CLIENT = (AES256, RC4_256)
            SQLNET.CRYPTO_SEED = 'should be 10-70 random characters'

    Any tool that uses the Oracle client to talk to the database will be
    encrypted if you setup a session encryption like above.

    For example, Perl's DBI uses DBD-Oracle, which uses the Oracle client
    for actually handling database communication. If the installation of
    Oracle client used by Perl is setup to request encrypted connections,
    then your Perl connection to an Oracle database will also be encrypted.

    Full details at
    https://kb.berkeley.edu/jivekb/entry.jspa?externalID=1005

  Testing
    Once you have set the Oracle database DSN you can execute pgora-migrate to see
    if it works. By default the configuration file will export the database
    schema to a file called 'output.sql'. Take a look in it to see if the
    schema has been exported.

    Take some time here to test your installation as most of the problem
    take place here, the other configuration step are more technical.

  Trouble shooting
    If the output.sql file has not exported anything else than the Pg
    transaction header and footer there's two possible reasons. The perl
    script pgora-migrate dump an ORA-XXX error, that mean that you DSN or login
    information are wrong, check the error and your settings and try again.
    The perl script says nothing and the output file is empty: the user has
    not enough right to extract something from the database. Try to connect
    Oracle as super user or take a look at directive USER_GRANTS above and
    at next section, especiallly the SCHEMA directive.

    LOGFILE
        By default all message are sent to the standard output. If you give
        a file path to that directive, all output will be appended to this
        file.

  Oracle schema to export
    The Oracle database export can be limited to a specific Schema or
    Namespace, this can be mandatory following the database connection user.

    SCHEMA
        This directive is used to set the schema name to use during export.
        For example:

                SCHEMA  APPS

        will extract objects associated to the APPS schema.

    EXPORT_SCHEMA
        By default the Oracle schema is not exported into the PostgreSQL
        database and all objects are created under the default Pg namespace.
        If you want to also export this schema and create all objects under
        this namespace, set the EXPORT_SCHEMA directive to 1. This will set
        the schema search_path at top of export SQL file to the schema name
        set in the SCHEMA directive with the default pg_catalog schema. If
        you want to change this path, use the directive PG_SCHEMA.

    CREATE_SCHEMA
        Enable/disable the CREATE SCHEMA SQL order at starting of the output
        file. It is enable by default and concern on TABLE export type.

    COMPILE_SCHEMA
        By default pgORA-Migrate will only export valid PL/SQL code. You can force
        Oracle to compile again the invalidated code to get a chance to have
        it obtain the valid status and then be able to export it.

        Enable this directive to force Oracle to compile schema before
        exporting code. This will ask to Oracle to validate the PL/SQL that
        could have been invalidate after a export/import for example. If you
        set the value to 1 it will exec: DBMS_UTILITY.compile_schema(schema
        => sys_context('USERENV', 'SESSION_USER')); but if you provide the
        name of a particular schema it will use the following command:
        DBMS_UTILITY.compile_schema(schema => 'schemaname'); The 'VALID' or
        'INVALID' status applies to functions, procedures, packages and user
        defined types.

    EXPORT_INVALID
        If the above configuration directive is not enough to validate your
        PL/SQL code enable this configuration directive to allow export of
        all PL/SQL code even if it is marked as invalid. The 'VALID' or
        'INVALID' status applies to functions, procedures, packages and user
        defined types.

    PG_SCHEMA
        Allow you to defined/force the PostgreSQL schema to use. The value
        can be a comma delimited list of schema name. By default if you set
        EXPORT_SCHEMA to 1, the PostgreSQL schema search_path will be set to
        the schema name set as value of the SCHEMA directive plus the
        default pg_catalog schema as follow:

                SET search_path = $SCHEMA, pg_catalog;

        If you set PG_SCHEMA to something like "user_schema, public" for
        example the search path will be set like this:

                SET search_path = $PG_SCHEMA;
                -- SET search_path = user_schema, public;

        This will force to not use the Oracle schema set in the SCHEMA
        directive.

    SYSUSERS
        Without explicit schema, pgORA-Migrate will export all objects that not
        belongs to system schema or role:

                CTXSYS,DBSNMP,EXFSYS,LBACSYS,MDSYS,MGMT_VIEW,OLAPSYS,ORDDATA,OWBSYS,
                ORDPLUGINS,ORDSYS,OUTLN,SI_INFORMTN_SCHEMA,SYS,SYSMAN,SYSTEM,WK_TEST,
                WKSYS,WKPROXY,WMSYS,XDB,APEX_PUBLIC_USER,DIP,FLOWS_020100,FLOWS_030000,
                FLOWS_040100,FLOWS_FILES,MDDATA,ORACLE_OCM,SPATIAL_CSW_ADMIN_USR,
                SPATIAL_WFS_ADMIN_USR,XS$NULL,PERFSTAT,SQLTXPLAIN,DMSYS,TSMSYS,WKSYS,
                APEX_040200,DVSYS,OJVMSYS,GSMADMIN_INTERNAL,APPQOSSYS

        Following your Oracle installation you may have several other system
        role defined. To append these users to the schema exclusion list,
        just set the SYSUSERS configuration directive to a comma-separated
        list of system user to exclude. For example:

                SYSUSERS        INTERNAL,SYSDBA

        will add users INTERNAL and SYSDBA to the schema exclusion list.

    FORCE_OWNER
        By default the owner of the database objects is the one you're using
        to connect to PostgreSQL using the psql command. If you use an other
        user (postgres for exemple) you can force pgORA-Migrate to set the object
        owner to be the one used in the Oracle database by setting the
        directive to 1, or to a completely different username by setting the
        directive value to that username.

    USE_TABLESPACE
        When enabled this directive force pgora-migrate to export all tables,
        indexes constraint and indexes using the tablespace name defined in
        Oracle database. This works only with tablespace that are not TEMP,
        USERS and SYSTEM.

  Export type
    The export action is perform following a single configuration directive
    'TYPE', some other add more control on what should be really exported.

    TYPE
        Here are the different values of the TYPE directive, default is
        TABLE:

                - TABLE: Extract all tables with indexes, primary keys, unique keys,
                  foreign keys and check constraints.
                - VIEW: Extract only views.
                - GRANT: Extract roles converted to Pg groups, users and grants on all
                  objects.
                - SEQUENCE: Extract all sequence and their last position.
                - TABLESPACE: Extract storage spaces for tables and indexes (Pg >= v8).
                - TRIGGER: Extract triggers defined following actions.
                - FUNCTION: Extract functions.
                - PROCEDURE: Extract procedures.
                - PACKAGE: Extract packages and package bodies.
                - INSERT: Extract data as INSERT statement.
                - COPY: Extract data as COPY statement.
                - PARTITION: Extract range and list Oracle partitioning.
                - TYPE: Extract user defined Oracle type.
                - FDW: Export Oracle tables as foreign table for oracle_fdw.
                - MVIEW: Export materialized view.
                - QUERY: Try to automatically convert Oracle SQL queries.
                - KETTLE: generate XML ktr template files to be used by Kettle.

        Only one type of export can be perform at the same time so the TYPE
        directive must be unique. If you have more than one only the last
        found in the file will be registered.

        Some export type can not or should not be load directly into the
        PostgreSQL database and still require little manual editing. This is
        the case for GRANT, TABLESPACE, TRIGGER, FUNCTION, PROCEDURE, TYPE,
        QUERY and PACKAGE export types especially if you have PLSQL code or
        Oracle specific SQL in it.

        For TABLESPACE you must ensure that file path exist on the system.

        Note that you can chained multiple export by giving to the TYPE
        directive a comma-separated list of export type.

        The PARTITION export is a work in progress as table partition
        support is not yet implemented into PostgreSQL. pgORA-Migrate will convert
        Oracle partition using table inheritence, trigger and function
        workaround. See document at Pg site:
        http://www.postgresql.org/docs/current/interactive/ddl-partitioning.
        html This new feature in pgORA-Migrate has not been widly tested so feel
        free to report any bug and patch.

        The TYPE export allow export of user defined Oracle type. If you
        don't use the --plsql command line parameter it simply dump Oracle
        user type asis else pgORA-Migrate will try to convert it to PostgreSQL
        syntax.

        The KETTLE export type requires that the Oracle and PostgreSQL DNS
        are defined.

        Since pgORA-Migrate v8.1 there's three new export types:

                SHOW_VERSION : display Oracle version
                SHOW_SCHEMA  : display the list of schema available in the database.
                SHOW_TABLE   : display the list of tables available.
                SHOW_COLUMN  : display the list of tables columns available and the
                        Ora2PG conversion type from Oracle to PostgreSQL that will be
                        applied. It will also warn you if there's PostgreSQL reserved
                        words in Oracle object names.

        Here is an example of the SHOW_COLUMN output:

                [2] TABLE CURRENT_SCHEMA (1 rows) (Warning: 'CURRENT_SCHEMA' is a reserved word in PostgreSQL)
                        CONSTRAINT : NUMBER(22) => bigint (Warning: 'CONSTRAINT' is a reserved word in PostgreSQL)
                        FREEZE : VARCHAR2(25) => varchar(25) (Warning: 'FREEZE' is a reserved word in PostgreSQL)
                ...
                [6] TABLE LOCATIONS (23 rows)
                        LOCATION_ID : NUMBER(4) => smallint
                        STREET_ADDRESS : VARCHAR2(40) => varchar(40)
                        POSTAL_CODE : VARCHAR2(12) => varchar(12)
                        CITY : VARCHAR2(30) => varchar(30)
                        STATE_PROVINCE : VARCHAR2(25) => varchar(25)
                        COUNTRY_ID : CHAR(2) => char(2)

        Those extraction keyword are use to only display the requested
        information and exit. This allow you to quickly know on what you are
        going to work.

        The SHOW_COLUMN allow an other pgora-migrate command line option: '--allow
        relname' or '-a relname' to limit the displayed information to the
        given table.

        Since pgORA-Migrate v8.2 there's a new export type:

                SHOW_ENCODING : display the Oracle session encoding, useful to set NSL_LANG.

        Since release v8.12, pgORA-Migrate allow you to export your Oracle Table
        definition to be use with the oracle_fdw foreign data wrapper. By
        using type FDW your Oracle tables will be exported as follow:

                CREATE FOREIGN TABLE oratab (
                        id        integer           NOT NULL,
                        text      character varying(30),
                        floating  double precision  NOT NULL
                ) SERVER oradb OPTIONS (table 'ORATAB');

        Now you can use the table like a regular PostgreSQL table.

        See http://pgxn.org/dist/oracle_fdw/ for more information on this
        foreign data wrapper.

        Release 10 adds a new export type destinated to evaluate the content
        of the database to migrate, in terms of objects and cost to end the
        migration:

                SHOW_REPORT  : show a detailled report of the Oracle database content.

        Here is a sample of report:

                --------------------------------------
                pgORA-Migrate: Oracle Database Content Report
                --------------------------------------
                Version Oracle Database 10g Express Edition Release 10.2.0.1.0
                Schema  HR
                Size  880.00 MB
         
                --------------------------------------
                Object  Number  Invalid Comments
                --------------------------------------
                CLUSTER   2 0 Clusters are not supported and will not be exported.
                FUNCTION  40  0 Total size of function code: 81992.
                INDEX     435 0 232 index(es) are concerned by the export, others are automatically generated and will
                                        do so on PostgreSQL. 1 bitmap index(es). 230 b-tree index(es). 1 reversed b-tree index(es)
                                        Note that bitmap index(es) will be exported as b-tree index(es) if any. Cluster, domain,
                                        bitmap join and IOT indexes will not be exported at all. Reverse indexes are not exported
                                        too, you may use a trigram-based index (see pg_trgm) or a reverse() function based index
                                        and search. You may also use 'varchar_pattern_ops', 'text_pattern_ops' or 'bpchar_pattern_ops'
                                        operators in your indexes to improve search with the LIKE operator respectively into
                                        varchar, text or char columns.
                MATERIALIZED VIEW 1 0 All materialized view will be exported as snapshot materialized views, they
                                        are only updated when fully refreshed.
                PACKAGE BODY  2 1 Total size of package code: 20700.
                PROCEDURE 7 0 Total size of procedure code: 19198.
                SEQUENCE  160 0 Sequences are fully supported, but all call to sequence_name.NEXTVAL or sequence_name.CURRVAL
                                        will be transformed into NEXTVAL('sequence_name') or CURRVAL('sequence_name').
                TABLE     265 0 1 external table(s) will be exported as standard table. See EXTERNAL_TO_FDW configuration
                                        directive to export as file_fdw foreign tables or use COPY in your code if you just
                                        want to load data from external files. 2 binary columns. 4 unknow types.
                TABLE PARTITION 8 0 Partitions are exported using table inheritance and check constraint. 1 HASH partitions.
                                        2 LIST partitions. 6 RANGE partitions. Note that Hash partitions are not supported.
                TRIGGER   30  0 Total size of trigger code: 21677.
                TYPE      7 1 5 type(s) are concerned by the export, others are not supported. 2 Nested Tables.
                                        2 Object type. 1 Subtype. 1 Type Boby. 1 Type inherited. 1 Varrays. Note that Type
                                        inherited and Subtype are converted as table, type inheritance is not supported.
                TYPE BODY 0 3 Export of type with member method are not supported, they will not be exported.
                VIEW      7 0 Views are fully supported, but if you have updatable views you will need to use
                                        INSTEAD OF triggers.
                DATABASE LINK 1 0 Database links will not be exported. You may try the dblink perl contrib module or use
                                        the SQL/MED PostgreSQL features with the different Foreign Data Wrapper (FDW) extentions.

                Note: Invalid code will not be exported unless the EXPORT_INVALID configuration directive is activated.

        There also a more advanced report with migration cost. See the
        dedicated chapter about Migration Cost Evaluation.

    ESTIMATE_COST
        Activate the migration cost evaluation. Must only be used with
        SHOW_REPORT, FUNCTION, PROCEDURE, PACKAGE and QUERY export type.
        Default is disabled. You may wat to use the --estimate_cost command
        line option instead to activate this functionnality. Note that
        enabling this directive will force PLSQL_PGSQL activation.

    COST_UNIT_VALUE
        Set the value in minutes of the migration cost evaluation unit.
        Default is five minutes per unit. See --cost_unit_value to change
        the unit value at command line.

    DUMP_AS_HTML
        By default when using SHOW_REPORT the migration report is generated
        as simple text, enabling this directive will force pgora-migrate to create
        a report in HTML format.

        See http://pgora-migrate.darold.net/report.html for a sample report.

    JOBS
        This configuration directive adds multiprocess support to data
        export type, the value is the number of process to use. Default is
        multiprocess disable.

        This directive is used to set the number of cores to used to
        parallelize data import into PostgreSQL. It replace the old code
        based on Perl Threads activated with the obsolete THREAD_COUNT
        configuration directive that was not very useful and is now replaced
        with fork() calls.

        There's no more limitation in parallel processing than the number of
        cores and the PostgreSQL I/O performance capabilities.

        Doesn't works under Windows Operating System, it is simply disabled.

    ORACLE_COPIES
        This configuration directive adds multiprocess support to extract
        data from Oracle. The value is the number of process to use to
        parallelize the select query. Default is parallel query disable.

        The parallelism is built on splitting the query following of the
        number of cores given as value to ORACLE_COPIES as follow:

                SELECT * FROM MYTABLE WHERE ABS(MOD(COLUMN, ORACLE_COPIES)) = CUR_PROC

        where COLUMN is a technical key like a primary or unique key where
        split will be based and the current core used by the query
        (CUR_PROC).

        Doesn't works under Windows Operating System, it is simply disabled.

    DEFINED_PK
        This directive is used to defined the technical key to used to split
        the query between number of cores set with the ORACLE_COPIES
        variable. For example:

                DEFINED_PK      EMPLOYEES:employee_id

        The parallel query that will be used supposing that -J or
        ORACLE_COPIES is set to 8:

                SELECT * FROM EMPLOYEES WHERE ABS(MOD(employee_id, 8)) = N

        where N is the current process forked starting from 0.

    PARALLEL_TABLES
        This directive is used to defined the number of tables that will be
        processed in parallel for data extraction. The limit is the number
        of cores on your machine. pgORA-Migrate will open one database connection
        for each parallel table extraction. This directive, when upper than
        1, will invalidate ORACLE_COPIES but not JOBS, so the real number of
        process that will be used if PARALLEL_TABLES * JOBS.

        Note that this directive when set upper that 1 will also
        automatically enable the FILE_PER_TABLE directive if your are
        exporting to files.

    FDW_SERVER
        This directive is used to set the name of the foreign data server
        that is used in the "CREATE SERVER name FOREIGN DATA WRAPPER
        oracle_fdw ..." command. This name will then be used in the "CREATE
        FOREIGN TABLE ..." SQL command. Default is arbitrary set to orcl.
        This only concern export type FDW.

    EXTERNAL_TO_FDW
        This directive, enabled by default, allow to export Oracle's
        External Tables as file_fdw foreign tables. To not export these
        tables at all, set the directive to 0.

  Limiting object to export
    You may want to export only a part of an Oracle database, here are a set
    of configuration directives that will allow you to control what parts of
    the database should be exported.

    ALLOW
        This directive allow you to set a list of objects on witch the
        export must be limited, excluding all other objects in the same type
        of export. The value is a space or comma-separated list of objects
        name to export. You can include valid regex into the list. For
        example:

                ALLOW           EMPLOYEES SALE_.* COUNTRIES .*_GEOM_SEQ

        will export objects with name EMPLOYEES, COUNTRIES, all objects
        begining with 'SALE_' and all objects with a name ending by
        '_GEOM_SEQ'. The object depends of the export type. Note that regex
        will not works with 8i database.

        This directive replace the obsolete 'TABLES' configuration
        directive, this is just a renaming to be less confusing.

    EXCLUDE
        This directive is the opposite of the previous, it allow you to
        define a space or comma-separated list of object name to exclude
        from the export. You can include valid regex into the list. For
        example:

                EXCLUDE         EMPLOYEES TMP_.* COUNTRIES

        will exclude object with name EMPLOYEES, COUNTRIES and all tables
        begining with 'tmp_'.

        For example, you can ban from export some unwanted function with
        this directive:

                EXCLUDE         write_to_.* send_mail_.*

        this example will exclude all functions, procedures or functions in
        a package with the name begining with those regex. Note that regex
        will not works with 8i database.

    VIEW_AS_TABLE
        Set which view to export as table. By default none. Value must be a
        list of view name or regexp separated by space or comma. If the
        object name is a view and the export type is TABLE, the view will be
        exported as a create table statement. If export type is COPY or
        INSERT, the corresponding data will be exported.

        This was initialy done with the ALLOW (or old TABLES directive) but
        that was not allow to use both object exclusion and view as table.

        See chapter "Exporting Oracle views as PostgreSQL tables" for more
        details.

    WHERE
        This directive allow you to specify a WHERE clause filter when
        dumping the contents of tables. Value is construct as follow:
        TABLE_NAME[WHERE_CLAUSE], or if you have only one where clause for
        each table just put the where clause as value. Both are possible
        too. Here are some examples:

                # Global where clause applying to all tables included in the export
                WHERE  1=1

                # Apply the where clause only on table TABLE_NAME
                WHERE  TABLE_NAME[ID1='001']

                # Applies two different clause on tables TABLE_NAME and OTHER_TABLE
                # and a generic where clause on DATE_CREATE to all other tables
                WHERE  TABLE_NAME[ID1='001' OR ID1='002] DATE_CREATE > '2001-01-01' OTHER_TABLE[NAME='test']

        Any where clause not included into a table name bracket clause will
        be applied to all exported table including the tables defined in the
        where clause. These WHERE clauses are very useful if you want to
        archive some data or at the opposite only export some recent data.

    TOP_MAX
        This directive is used to limit the number of item shown in the top
        N lists like the top list of tables per number of rows and the top
        list of largest tables in megabytes. By default it is set to 10
        items.

    LOG_ON_ERROR
        Enable this directive if you want to continue direct data import on
        error. When pgORA-Migrate received an error in the COPY or INSERT statement
        from PostgreSQL it will log the statement to a file called
        TABLENAME_error.log in the output directory and continue to next
        bulk of data. Like this you can try to fix the statement and
        manually reload the error log file. Default is disabled: abort
        import on error.

  Modifying object structure
    One of the great usage of pgORA-Migrate is its flexibility to replicate Oracle
    database into PostgreSQL database with a different structure or schema.
    There's three configuration directives that allow you to map those
    differences.

    REORDERING_COLUMNS
        Enable this directive to reordering columns and minimized the
        footprint on disc, so that more rows fit on a data page, which is
        the most important factor for speed. Default is disabled, that mean
        the same order than in Oracle tables definition, that's should be
        enough for most usage. This directive is only used with TABLE
        export.

    MODIFY_STRUCT
        This directive allow you to limit the columns to extract for a given
        table. The value consist in a space-separated list of table name
        with a set of column between parenthesis as follow:

                MODIFY_STRUCT   NOM_TABLE(nomcol1,nomcol2,...) ...

        for example:

                MODIFY_STRUCT   T_TEST1(id,dossier) T_TEST2(id,fichier)

        This will only extract columns 'id' and 'dossier' from table T_TEST1
        and columns 'id' and 'fichier' from the T_TEST2 table. This
        directive is only used with COPY or INSERT export.

    MODIFY_TYPE
        Some time you need to force the destination type, for example a
        column exported as timestamp by pgORA-Migrate can be forced into type date.
        Value is a comma-separated list of TABLE:COLUMN:TYPE structure. If
        you need to use comma or space inside type definintion you will have
        to backslach them.

                TABLE1:COL3:varchar,TABLE1:COL4:decimal(9\,6)

    REPLACE_TABLES
        This directive allow you to remap a list of Oracle table name to a
        PostgreSQL table name during export. The value is a list of
        space-separated values with the following structure:

                REPLACE_TABLES  ORIG_TBNAME1:DEST_TBNAME1 ORIG_TBNAME2:DEST_TBNAME2

        Oracle tables ORIG_TBNAME1 and ORIG_TBNAME2 will be respectively
        renamed into DEST_TBNAME1 and DEST_TBNAME2

    REPLACE_COLS
        Like table name, the name of the column can be remapped to a
        different name using the following syntaxe:

                REPLACE_COLS    ORIG_TBNAME(ORIG_COLNAME1:NEW_COLNAME1,ORIG_COLNAME2:NEW_COLNAME2)

        For example:

                REPLACE_COLS    T_TEST(dico:dictionary,dossier:folder)

        will rename Oracle columns 'dico' and 'dossier' from table T_TEST
        into new name 'dictionary' and 'folder'.

    REPLACE_AS_BOOLEAN
        If you want to change the type of some Oracle columns into
        PostgreSQL boolean during the export you can define here a list of
        tables and column separated by space as follow.

                REPLACE_AS_BOOLEAN     TB_NAME1:COL_NAME1 TB_NAME1:COL_NAME2 TB_NAME2:COL_NAME2

        The values set in the boolean columns list will be replaced with the
        't' and 'f' following the default replacement values and those
        additionally set in directive BOOLEAN_VALUES.

        You can also give a type and a precision to automatically convert
        all fields of that type as a boolean. For example:

                REPLACE_AS_BOOLEAN      NUMBER:1 CHAR:1 TB_NAME1:COL_NAME1 TB_NAME1:COL_NAME2

        will also replace any field of type number(1) or char(1) as a
        boolean in all exported tables.

    BOOLEAN_VALUES
        Use this to add additional definition of the possible boolean values
        used in Oracle fields. You must set a space-separated list of
        TRUE:FALSE values. By default here are the values recognized by
        pgORA-Migrate:

                BOOLEAN_VALUES          yes:no y:n 1:0 true:false enabled:disabled

        Any values defined here will be added to the default list.

    INDEXES_SUFFIX
        Add the given value as suffix to indexes names. Useful if you have
        indexes with same name as tables. For example:

                INDEXES_SUFFIX          _idx

        will add _idx at ed of all index name. Not so common but can help.

    PREFIX_PARTITION
        Enable this directive if you want that your partition table name
        will be exported using the parent table name. Disabled by default.
        If you have multiple partitioned table, when exported to PostgreSQL
        some partitions could have the same name but dfferent parent tables.
        This is not allowed, table name must be unique.

  Oracle Spatial to PostGis
    pgORA-Migrate fully export Spatial object from Oracle database. There's some
    configuration directives that could be used to control the export.

    AUTODETECT_SPATIAL_TYPE
        Enable this directive if you want pgORA-Migrate to detect the real spatial
        type and dimension used in a spatial column. Otherwise column will
        be created with non-constrained "geometry" type. Enabling this
        feature will force pgORA-Migrate to scan a sample of 50000 column to look
        at the GTYPE used. You can increase the sample by setting the value
        of AUTODETECT_SPATIAL_TYPE to the desired number of line. The
        directive is enabled by default.

        For example, in the case of a column named shape and defined with
        Oracle type SDO_GEOMETRY, with AUTODETECT_SPATIAL_TYPE disabled it
        will be converted as:

            shape geometry(GEOMETRY) or shape geometry(GEOMETRYZ, 4326)

        and if the directive is enabled and the column just contains a
        single geometry type that use a single dimension:

            shape geometry(POLIGON, 4326) or shape geometry(POLIGONZ, 4326)

        with a two or three dimensional polygon.

    CONVERT_SRID
        This directive allow you to control the automatically convertion of
        Oracle SRID to standard EPSG. If enabled, pgORA-Migrate will use the Oracle
        function sdo_cs.map_oracle_srid_to_epsg() to convert all SRID.
        Enabled by default.

        If the SDO_SRID returned by Oracle is NULL, it will be replaced by
        the default value 8307 converted to its EPSG value: 4326 (see
        DEFAULT_SRID).

        If the value is upper than 1, all SRID will be forced to this value,
        in this case DEFAULT_SRID will not be used when Oracle returns a
        null value and the value will be forced to CONVERT_SRID.

    DEFAULT_SRID
        Use this directive to override the default EPSG SRID to used: 4326.
        Can be overwritten by CONVERT_SRID, see above.

    USE_SC40_PACKAGE
        If you want to replace call to SDO_UTIL.TO_WKTGEOMETRY() by a call
        to the more efficient methode SC4O.ST_AsText(), especially for 3D
        geomrtry, activate this directive. You must install the SC40 package
        before, this is the SpatialDB Advisor extension developped by Simon
        Greener and available at
        http://www.spatialdbadvisor.com/source_code_form

        If SC40 is not installed in the same schema, you can set the value
        to the schema name. Example:

                     USE_SC40_PACKAGE   CODESYS

        The function will be called as CODESYS.SC4O.ST_AsText().

    POSTGIS_SCHEMA
        Use this directive to add a specific schema to the search path to
        look for PostGis functions.

  PostgreSQL Import
    By default conversion to PostgreSQL format is written to file
    'output.sql'. The command:

            psql mydb < output.sql

    will import content of file output.sql into PostgreSQL mydb database.

    DATA_LIMIT
        When you are performing INSERT/COPY export pgORA-Migrate proceed by chunks
        of DATA_LIMIT tuples for speed improvement. Tuples are stored in
        memory before being written to disk, so if you want speed and have
        enough system resources you can grow this limit to an upper value
        for example: 100000 or 1000000. Before release 7.0 a value of 0 mean
        no limit so that all tuples are stored in memory before being
        flushed to disk. In 7.x branch this has been remove and chunk will
        be set to the default: 10000

    OUTPUT
        The pgORA-Migrate output filename can be changed with this directive.
        Default value is output.sql. if you set the file name with extension
        .gz or .bz2 the output will be automatically compressed. This
        require that the Compress::Zlib Perl module is installed if the
        filename extension is .gz and that the bzip2 system command is
        installed for the .bz2 extension.

    OUTPUT_DIR
        Since release 7.0, you can define a base directory where wfile will
        be written. The directory must exists.

    BZIP2
        This directive allow you to specify the full path to the bzip2
        program if it can not be found in the PATH environment variable.

    FILE_PER_CONSTRAINT
        Allow object constraints to be saved in a separate file during
        schema export. The file will be named CONSTRAINTS_OUTPUT, where
        OUTPUT is the value of the corresponding configuration directive.
        You can use .gz xor .bz2 extension to enable compression. Default is
        to save all data in the OUTPUT file. This directive is usable only
        with TABLE export type.

    FILE_PER_INDEX
        Allow indexes to be saved in a separate file during schema export.
        The file will be named INDEXES_OUTPUT, where OUTPUT is the value of
        the corresponding configuration directive. You can use .gz xor .bz2
        file extension to enable compression. Default is to save all data in
        the OUTPUT file. This directive is usable only with TABLE AND
        TABLESPACE export type. With the TABLESPACE export, it is used to
        write "ALTER INDEX ... TABLESPACE ..." into a separate file named
        TBSP_INDEXES_OUPUT that can be loaded at end of the migration after
        the indexes creation to move the indexes.

    FILE_PER_TABLE
        Allow data export to be saved in one file per table/view. The files
        will be named as tablename_OUTPUT, where OUTPUT is the value of the
        corresponding configuration directive. You can still use .gz xor
        .bz2 extension in the OUTPUT directive to enable compression.
        Default 0 will save all data in one file, set it to 1 to enable this
        feature. This is usable only during INSERT or COPY export type.

    FILE_PER_FUNCTION
        Allow functions, procedures and triggers to be saved in one file per
        object. The files will be named as objectname_OUTPUT. Where OUTPUT
        is the value of the corresponding configuration directive. You can
        still use .gz xor .bz2 extension in the OUTPUT directive to enable
        compression. Default 0 will save all in one single file, set it to 1
        to enable this feature. This is usable only during the corresponding
        export type, the package body export has a special behavior.

        When export type is PACKAGE and you've enabled this directive,
        pgORA-Migrate will create a directory per package, named with the lower
        case name of the package, and will create one file per
        function/procedure into that directory. If the configuration
        directive is not enabled, it will create one file per package as
        packagename_OUTPUT, where OUTPUT is the value of the corresponding
        directive.

    TRUNCATE_TABLE
        If this directive is set to 1, a TRUNCATE TABLE instruction will be
        add before loading data. This is usable only during INSERT or COPY
        export type.

    STOP_ON_ERROR
        Set this parameter to 0 to not include the call to \set
        ON_ERROR_STOP ON in all SQL scripts generated by pgORA-Migrate. By default
        this order is always present so that the script will immediatly
        abort when an error is encountered.

    If you want to import data on the fly to the PostgreSQL database you
    have three configuration directives to set the PostgreSQL database
    connection. This is only possible with COPY or INSERT export type as for
    database schema there's no real interest to do that.

    PG_DSN
        Use this directive to set the PostgreSQL data source namespace using
        DBD::Pg Perl module as follow:

                dbi:Pg:dbname=pgdb;host=localhost;port=5432

        will connect to database 'pgdb' on localhost at tcp port 5432.

    PG_USER and PG_PWD
        These two directives are used to set the login user and password.

    SYNCHRONOUS_COMMIT
        Specifies whether transaction commit will wait for WAL records to be
        written to disk before the command returns a "success" indication to
        the client. This is the equivalent to set synchronous_commit
        directive of postgresql.conf file. This is only used when you load
        data directly to PostgreSQL, the default is off to disable
        synchronous commit to gain speed at writing data. Some modified
        version of PostgreSQL, like greenplum, do not have this setting, so
        in this set this directive to 1, pgora-migrate will not try to change the
        setting.

  Taking export under control
    The following other configuration directives interact directly with the
    export process and give you fine granuality in database export control.

    SKIP
        For TABLE export you may not want to export all schema constraints,
        the SKIP configuration directive allow you to specify a
        space-separated list of constraints that should not be exported.
        Possible values are:

                - fkeys: turn off foreign key constraints
                - pkeys: turn off primary keys
                - ukeys: turn off unique column constraints
                - indexes: turn off all other index types
                - checks: turn off check constraints

        For example:

                SKIP    indexes,checks

        will removed indexes ans check constraints from export.

    PKEY_IN_CREATE
        Enable this directive if you want to add primary key definition
        inside the create table statement. If disabled (the default) primary
        key definition will be add with an alter table statement. Enable it
        if you are exporting to GreenPlum PostgreSQL database.

    KEEP_PKEY_NAMES
        By default names of the primary and unique key in the source Oracle
        database are ignored and key names are created in the target
        PostgreSQL database with the PostgreSQL internal default naming
        rules. If you want to preserve Oracle primary key names set this
        option to 1.

    FKEY_DEFERRABLE
        When exporting tables, pgORA-Migrate normally exports constraints as they
        are, if they are non-deferrable they are exported as non-deferrable.
        However, non-deferrable constraints will probably cause problems
        when attempting to import data to Pg. The FKEY_DEFERRABLE option set
        to 1 will cause all foreign key constraints to be exported as
        deferrable.

    DEFER_FKEY
        In addition, when exporting data the DEFER_FKEY option set to 1 will
        add a command to defer all foreign key constraints during data
        export. Constraints will then be checked at the end of each
        transaction. Note that this will works only if foreign keys are
        deferrable and that all data can stay in a single transaction. This
        will work only if foreign keys have been exported as deferrables.
        Constraints will then be checked at the end of the transaction.

        This directive can also be enabled if you want to force all foreign
        keys to be created as deferrable and initially deferred during
        schema export (TABLE export type). Since release 11.5 only.

    DROP_FKEY
        When this directive is enabled pgORA-Migrate forces the deletion of all
        foreign keys before data import and to recreate them at end of the
        data import.

    DROP_INDEXES
        This direction is also introduce since version 7.0 and allow you to
        gain lot of speed improvement during data import by removing all
        indexes that are not an automatic index (ex: indexes of primary
        keys) and recreate them at the end of data import.

    DISABLE_TRIGGERS
        This directive is used to disable triggers on all tables in COPY or
        INSERT export modes. Available values are iO, USER (disable
        userdefined triggers only) and ALL (includes RI system triggers).
        Default is 0: do not add SQL statements to disable trigger before
        data import.

        If you want to disable triggers during data migration, set the value
        to USER if your are connected as non superuser and ALL if you are
        connected as PostgreSQL superuser. A value of 1 is equal to USER.

    DISABLE_SEQUENCE
        If set to 1 disables alter of sequences on all tables during COPY or
        INSERT export mode. This is used to prevent the update of sequence
        during data migration. Default is 0, alter sequences.

    NOESCAPE
        By default all data that are not of type date or time are escaped.
        If you experience any problem with that you can set it to 1 to
        disable character escaping during data export. This directive is
        only used during a COPY export. See STANDARD_CONFORMING_STRINGS for
        enabling/disabling escape with INSERT statements.

    STANDARD_CONFORMING_STRINGS
        This controls whether ordinary string literals ('...') treat
        backslashes literally, as specified in SQL standard. This was the
        default before pgORA-Migrate v8.5 so that all strings was escaped first,
        now this is currently on, causing pgORA-Migrate to use the escape string
        syntax (E'...') if this parameter is not set to 0. This is the exact
        behavior of the same option in PostgreSQL. This directive is only
        used during data export to build INSERT statements. See NOESCAPE for
        enabling/disabling escape in COPY statements.

    PG_NUMERIC_TYPE
        If set to 1 replace portable numeric type into PostgreSQL internal
        type. Oracle data type NUMBER(p,s) is approximatively converted to
        real and float PostgreSQL data type. If you have monetary fields or
        don't want rounding issues with the extra decimals you should
        preserve the same numeric(p,s) PostgreSQL data type. Do that only if
        you need very good precision because using numeric(p,s) is slower
        than using real or double.

    PG_INTEGER_TYPE
        If set to 1 replace portable numeric type into PostgreSQL internal
        type. Oracle data type NUMBER(p) or NUMBER are converted to
        smallint, integer or bigint PostgreSQL data type following the
        length of the precision. If NUMBER without precision are set to
        DEFAULT_NUMERIC (see bellow).

    DEFAULT_NUMERIC
        NUMBER without precision are converted by default to bigint only if
        PG_INTEGER_TYPE is true. You can overwrite this value to any PG
        type, like integer or float.

    DATA_TYPE
        If you're experiencing any problem in data type schema conversion
        with this directive you can take full control of the correspondence
        between Oracle and PostgreSQL types to redefine data type
        translation used in Ora2pg. The syntax is a comma-separated list of
        "Oracle datatype:Postgresql datatype". Here are the default list
        used:

                DATA_TYPE       DATE:timestamp,LONG:text,LONG RAW:bytea,CLOB:text,NCLOB:text,BLOB:bytea,BFILE:bytea,RAW:bytea,ROWID:oid,FLOAT:double precision,DEC:decimal,DECIMAL:decimal,DOUBLE PRECISION:double precision,INT:integer,INTEGER:integer,REAL:real,SMALLINT:smallint,BINARY_FLOAT:double precision,BINARY_DOUBLE:double precision,TIMESTAMP:timestamp,XMLTYPE:xml,BINARY_INTEGER:integer,PLS_INTEGER:integer,TIMESTAMP WITH TIME ZONE:timestamp with time zone,TIMESTAMP WITH LOCAL TIME ZONE:timestamp with time zone

        Note that the directive and the list definition must be a single
        line.

        There's a special case with BFILE when they are converted to text
        field, they will contains the path to the external file. If you set
        the destination type to bytea, the default, pgORA-Migrate will export the
        BFILE as bytea.

        There's no SQL function available to retrieve the path to the BFILE,
        then pgORA-Migrate have to create one using the DBMS_LOB package.

                CREATE OR REPLACE FUNCTION pgora-migrate_get_bfilename( p_bfile IN BFILE )
                RETURN VARCHAR2
                  AS
                    l_dir   VARCHAR2(4000);
                    l_fname VARCHAR2(4000);
                    l_path  VARCHAR2(4000);
                  BEGIN
                    dbms_lob.FILEGETNAME( p_bfile, l_dir, l_fname );
                    SELECT directory_path INTO l_path FROM all_directories
                        WHERE directory_name = l_dir;
                    l_dir := rtrim(l_path,'/');
                    RETURN l_dir || '/' || l_fname;
                  END;

        This function is only created if pgORA-Migrate found a table with a BFILE
        column and the function is dropped at end of the export. This
        concern COPY and INSERT export type.

    PRESERVE_CASE
        If you want to preserve the case of Oracle object name set this
        directive to 1. By default pgORA-Migrate will convert all Oracle object
        names to lower case. I do not recommand to enable this unless you
        will always have to double-quote object names on all your SQL
        scripts.

    ORA_RESERVED_WORDS
        Allow escaping of column name using Oracle reserved words. Value is
        a list of comma-separated reserved word. Default is audit,comment.

    USE_RESERVED_WORDS
        Enable this directive if you have table or column names that are a
        reserved word for PostgreSQL. pgORA-Migrate will double quote the name of
        the object.

    GEN_USER_PWD
        Set this directive to 1 to replace default password by a random
        password for all extracted user during a GRANT export.

    PG_SUPPORTS_MVIEW
        Since PostgreSQL 9.3, materialized view are supported with the SQL
        syntax 'CREATE MATERIALIZED VIEW'. To force pgORA-Migrate to use the native
        PostgreSQL support you must enable this configuration - enable by
        default. If you want to use the old style with table and a set of
        function, you should disable it.

    PG_SUPPORTS_IFEXISTS
        PostgreSQL version below 9.x do not support IF EXISTS in DDL
        statements. Disabling the directive with value 0 will prevent pgORA-Migrate
        to add those keywords in all generated statments. Default value is
        1, enabled.

    PG_SUPPORTS_ROLE (Deprecated)
        This option is deprecated since pgORA-Migrate release v7.3.

        By default Oracle roles are translated into PostgreSQL groups. If
        you have PostgreSQL 8.1 or more consider the use of ROLES and set
        this directive to 1 to export roles.

    PG_SUPPORTS_INOUT (Deprecated)
        This option is deprecated since pgORA-Migrate release v7.3.

        If set to 0, all IN, OUT or INOUT parameters will not be used into
        the generated PostgreSQL function declarations (disable it for
        PostgreSQL database version lower than 8.1), This is now enable by
        default.

    PG_SUPPORTS_DEFAULT
        This directive enable or disable the use of default parameter value
        in function export. Until PostgreSQL 8.4 such a default value was
        not supported, this feature is now enable by default.

    PG_SUPPORTS_WHEN (Deprecated)
        Add support to WHEN clause on triggers as PostgreSQL v9.0 now
        support it. This directive is enabled by default, set it to 0
        disable this feature.

    PG_SUPPORTS_INSTEADOF (Deprecated)
        Add support to INSTEAD OF usage on triggers (used with PG >= 9.1),
        if this directive is disabled the INSTEAD OF triggers will be
        rewritten as Pg rules.

    PG_SUPPORTS_CHECKOPTION
        If enabled, export views with CHECK OPTION. Enable it if you have
        PostgreSQL version 9.4 or higher. Default: 0, disabled.

    PG_SUPPORTS_IFEXISTS
        If disabled, do not export object with IF EXISTS statements. Enabled
        by default.

    LONGREADLEN
        Use this directive to set the database handle's 'LongReadLen'
        attribute to a value that will be the larger than the expected size
        of the LOBs. The default is 1Mb witch may not be enough to extract
        BLOBs or CLOBs. If the size of the LOB exceeds the 'LongReadLen'
        DBD::Oracle will return a 'ORA-24345: A Truncation' error. Default:
        1023*1024 bytes.

        Take a look at this page to learn more:
        http://search.cpan.org/~pythian/DBD-Oracle-1.22/Oracle.pm#Data_Inter
        face_for_Persistent_LOBs

        Important note: If you increase the value of this directive take
        care that DATA_LIMIT will probably needs to be reduced. Even if you
        only have a 1Mb blob, trying to read 10000 of them (the default
        DATA_LIMIT) all at once will require 10Gb of memory. You may extract
        data from those table separatly and set a DATA_LIMIT to 500 or
        lower, otherwise you may experience some Out of memory.

    LONGTRUNKOK
        If you want to bypass the 'ORA-24345: A Truncation' error, set this
        directive to 1, it will truncate the data extracted to the
        LongReadLen value. Disable by default so that you will be warned if
        your LongReadLen value is not high enough.

    XML_PRETTY
        Force the use getStringVal() instead of getClobVal() for XML data
        export. Default is 1, enabled for backward compatibility. Set it to
        0 to use extract method a la CLOB.

    ENABLE_MICROSECOND
        Set it to O if you want to disable export of millisecond from Oracle
        timestamp columns. By default milliseconds are exported with the use
        of following format:

                'YYYY-MM-DD HH24:MI:SS.FF'

        Disabling will force the use of the following Oracle format:

                to_char(..., 'YYYY-MM-DD HH24:MI:SS')

        By default milliseconds are exported.

    DISABLE_COMMENT
        Set this to 1 if you don't want to export comment associated to
        tables and columns definition. Default is enabled.

  Special options to handle character encoding
    NLS_LANG
        By default Oracle character set is automatically detected, but if
        you experience any issues where mutibyte characters are being
        substituted with some replacement characters during the export try
        to set the NLS_LANG configuration directive to the Oracle encoding.
        This may help a lot especially with UTF8 encoding. For example:

                NLS_LANG        AMERICAN_AMERICA.UTF8

        This will set $ENV{NLS_LANG} to the given value.

    BINMODE
        If you experience the Perl warning: "Wide character in print", it
        means that you tried to write a Unicode string to a non-unicode file
        handle. You can force Perl to use binary mode for output by setting
        the BINMODE configuration option to the specified encoding. If you
        set it to 'utf8', it will force printing like this: binmode OUTFH,
        ":utf8"; By default pgORA-Migrate opens the output file in 'raw' binary
        mode.

    CLIENT_ENCODING
        By default PostgreSQL client encoding is automatically detected and
        set to avoid encoding issue. If you want to defined your own or if
        you experience some ERROR: invalid byte sequence for encoding
        "UTF8": 0xe87472 when loading data you may want to set the encoding
        of the PostgreSQL client.

        For example, let's say you have an Oracle database with all data
        encoded in FRENCH_FRANCE.WE8ISO8859P15, your system use fr_FR.UTF-8
        as console encoding and your PostgreSQL database is encoded in UTF8.
        What you have to do is set the NLS_LANG to
        FRENCH_FRANCE.WE8ISO8859P15 and the CLIENT_ENCODING to LATIN9.

        You can take a look at the PostgreSQL supported character sets here:
        http://www.postgresql.org/docs/9.0/static/multibyte.html

  PLSQL to PLPSQL convertion
    Automatic code convertion from Oracle PLSQL to PostgreSQL PLPGSQL is a
    work in progress in pgORA-Migrate and surely you will always have manual work.
    The Perl code used for automatic conversion is all stored in a specific
    Perl Module named pgORA-Migrate/PLSQL.pm feel free to modify/add you own code
    and send me patches. The main work in on function, procedure, package
    and package body headers and parameters rewrite.

    PLSQL_PGSQL
        Enable/disable PLSQL to PLPSQL convertion. Enabled by default.

    ALLOW_CODE_BREAK
        This directive is use to enable/disable the plsql to pgplsql
        conversion part that could break the original code if they include
        complex subqueries. Default is enabled, you must disabled if to
        preserve backward compatibility. This concern the following
        replacement: decode(), substr()

        For example code like this:

                substr(decode("db_status",'active',"dbname",null),1,128)

        can easily be replaced by the PostgreSQL equivalent:

                substring((CASE WHEN "db_status"='active' THEN "dbname" ELSE NULL END) from 1 for 128))

        The problem could comes when you introduce subquery into one of the
        substr() or decode() parameter. For example the replacement of

                substr(decode("db_status",(select status from dbcluster where lbl=substr("dbname",1,3)),"dbname",null),1,128)

        will break the code. You can still compare to the original Oracle
        code and solve the problem, but if you want you can disable this
        unsecure replacement.

    NULL_EQUAL_EMPTY
        By default pgORA-Migrate will replace all conditions with a test on NULL by
        a call to the coalesce() function to mimic the Oracle behavior where
        empty string are considered equal to NULL.

                (field1 IS NULL) is replaced by (coalesce(field1::text, '') = '')
                (field2 IS NOT NULL) is replaced by (field2 IS NOT NULL AND field2::text <> '')

        Default is replacement to be sure that your application will have se
        same behavior.

  Materialized view
    Since PostgreSQL 9.3, materialized view are supported with the CREATE
    MATERIALIZED VIEW syntax, to force pgORA-Migrate to use the native PostgreSQL
    support you must enable the configuration directive PG_SUPPORTS_MVIEW.

    In other case pgORA-Migrate will export all materialized views as "Snapshot
    Materialized Views" as explain in this document:
    http://tech.jonathangardner.net/wiki/PostgreSQL/Materialized_Views.

    When exporting materialized view pgORA-Migrate will first add the SQL code to
    create the "materialized_views" table:

            CREATE TABLE materialized_views (
                    mview_name text NOT NULL PRIMARY KEY,
                    view_name text NOT NULL,
                    iname text,
                    last_refresh TIMESTAMP WITH TIME ZONE
            );

    all materialized views will have an entry in this table. It then adds
    the plpgsql code to create tree functions:

            create_materialized_view(text, text, text) used to create a materialized view
            drop_materialized_view(text) used to delete a materialized view
            refresh_full_materialized_view(text) used to refresh a view

    then it adds the SQL code to create the view and the materialized view:

            CREATE VIEW mviewname_mview AS
            SELECT ... FROM ...;

            SELECT create_materialized_view('mviewname','mviewname_mview', change with the name of the colum to used for the index);

    The first argument is the name of the materializd view, the second the
    name of the view on which the materialized view is based and the third
    is the column name on which the index should be build (aka most od the
    time the primary key). This column is not automatically deduced so you
    need to repace its name.

    As said above pgORA-Migrate only supports snapshot materialized views so the
    table will be entirely refreshed by issuing first a truncate of the
    table and then by load again all data from the view:

             refresh_full_materialized_view('mviewname');

    To drop the materialized view you just have to call the
    drop_materialized_view() function with the name of the materialized view
    as parameter.

  Other configuration directives
    DEBUG
        Set it to 1 will enable verbose output.

    IMPORT
        You can define common pgORA-Migrate configuration directives into a single
        file that can be imported into other configuration files with the
        IMPORT configuration directive as follow:

                IMPORT  commonfile.conf

        will import all configuration directives defined into
        commonfile.conf into the current configuration file.

  Exporting Oracle views as PostgreSQL tables
    You can export any Oracle view as a PostgreSQL table simply by setting
    TYPE configuration option to TABLE to have the corresponding create
    table statement. Or use type COPY or INSERT to export the corresponding
    data. To allow that you have to specify your views in the VIEW_AS_TABLE
    configuration option.

    Then if pgORA-Migrate finds the view it will extract its schema (if TYPE=TABLE)
    into a PG create table form, then it will extract the data (if TYPE=COPY
    or INSERT) following the view schema.

    You can use the ALLOW and EXCLUDE directive in addition to select the
    others objects to export.

  Export as Kettle transformation XML files
    The KETTLE export type is useful if you want to use Penthalo Data
    Integrator (Kettle) to import data to PostgreSQL. With this type of
    export pgORA-Migrate will generate one XML Kettle transformation files (.ktr)
    per table and add a line to manually execute the transformation in the
    output.sql file. For example:

            pgora-migrate -c pgora-migrate.conf -t KETTLE -j 12 -a MYTABLE -o load_mydata.sh

    will generate one file called 'HR.MYTABLE.ktr' and add a line to the
    output file (load_mydata.sh):

            #!/bin/sh

            KETTLE_TEMPLATE_PATH='.'

            JAVAMAXMEM=4096 ./pan.sh -file $KETTLE_TEMPLATE_PATH/HR.MYTABLE.ktr -level Detailed

    The -j 12 option will create a template with 12 processes to insert data
    into PostgreSQL. It is also possible to specify the number of parallel
    queries used to extract data from the Oracle with the -J command line
    option as follow:

            pgora-migrate -c pgora-migrate.conf -t KETTLE -J 4 -j 12 -a EMPLOYEES -o load_mydata.sh

    This is only possible if you have defined the technical key to used to
    split the query between cores in the DEFINED_PKEY configuration
    directive. For example:

            DEFINED_PK      EMPLOYEES:employee_id

    will force the number of Oracle connection copies to 4 and defined the
    SQL query as follow in the Kettle XML transformation file:

            <sql>SELECT * FROM HR.EMPLOYEES WHERE ABS(MOD(employee_id,${Internal.Step.Unique.Count}))=${Internal.Step.Unique.Number}</sql>

    The KETTLE export type requires that the Oracle and PostgreSQL DSN are
    defined. You can also activate the TRUNCATE_TABLE directive to force a
    truncation of the table before data import.

    The KETTLE export type is an original work of Marc Cousin.

  Migration cost assessment
    Estimating the cost of a migration process from Oracle to PostgreSQL is
    not easy. To obtain a good assessment of this migration cost, pgORA-Migrate
    will inspect all database objects, all functions and stored procedures
    to detect if there's still some objects and PL/SQL code that can not be
    automatically converted by pgORA-Migrate.

    pgORA-Migrate has a content analysis mode that inspect the Oracle database to
    generate a text report on what the Oracle database contains and what can
    not be exported.

    To activate the "analysis and report" mode, you have to use the export
    de type SHOW_REPORT like in the following command:

            pgora-migrate -t SHOW_REPORT

    Here is a sample report obtained with this command:

            --------------------------------------
            pgORA-Migrate: Oracle Database Content Report
            --------------------------------------
            Version Oracle Database 10g Enterprise Edition Release 10.2.0.1.0
            Schema  HR
            Size  880.00 MB
         
            --------------------------------------
            Object  Number  Invalid Comments
            --------------------------------------
            CLUSTER   2 0 Clusters are not supported and will not be exported.
            FUNCTION  40  0 Total size of function code: 81992.
            INDEX     435 0 232 index(es) are concerned by the export, others are automatically generated and will
                                            do so on PostgreSQL. 1 bitmap index(es). 230 b-tree index(es). 1 reversed b-tree index(es)
                                            Note that bitmap index(es) will be exported as b-tree index(es) if any. Cluster, domain,
                                            bitmap join and IOT indexes will not be exported at all. Reverse indexes are not exported
                                            too, you may use a trigram-based index (see pg_trgm) or a reverse() function based index
                                            and search. You may also use 'varchar_pattern_ops', 'text_pattern_ops' or 'bpchar_pattern_ops'
                                            operators in your indexes to improve search with the LIKE operator respectively into
                                            varchar, text or char columns.
            MATERIALIZED VIEW 1 0 All materialized view will be exported as snapshot materialized views, they
                                            are only updated when fully refreshed.
            PACKAGE BODY  2 1 Total size of package code: 20700.
            PROCEDURE 7 0 Total size of procedure code: 19198.
            SEQUENCE  160 0 Sequences are fully supported, but all call to sequence_name.NEXTVAL or sequence_name.CURRVAL
                                            will be transformed into NEXTVAL('sequence_name') or CURRVAL('sequence_name').
            TABLE     265 0 1 external table(s) will be exported as standard table. See EXTERNAL_TO_FDW configuration
                                            directive to export as file_fdw foreign tables or use COPY in your code if you just
                                            want to load data from external files. 2 binary columns. 4 unknow types.
            TABLE PARTITION 8 0 Partitions are exported using table inheritance and check constraint. 1 HASH partitions.
                                            2 LIST partitions. 6 RANGE partitions. Note that Hash partitions are not supported.
            TRIGGER   30  0 Total size of trigger code: 21677.
            TYPE      7 1 5 type(s) are concerned by the export, others are not supported. 2 Nested Tables.
                                            2 Object type. 1 Subtype. 1 Type Boby. 1 Type inherited. 1 Varrays. Note that Type
                                            inherited and Subtype are converted as table, type inheritance is not supported.
            TYPE BODY 0 3 Export of type with member method are not supported, they will not be exported.
            VIEW      7 0 Views are fully supported, but if you have updatable views you will need to use
                                            INSTEAD OF triggers.
            DATABASE LINK 1 0 Database links will not be exported. You may try the dblink perl contrib module or use
                                            the SQL/MED PostgreSQL features with the different Foreign Data Wrapper (FDW) extentions.
                                        
            Note: Invalid code will not be exported unless the EXPORT_INVALID configuration directive is activated.

    Once the database can be analysed, pgORA-Migrate, by his ability to convert SQL
    and PL/SQL code from Oracle syntax to PostgreSQL, can go further by
    estimating the code difficulties and estimate the time necessary to
    operate a full database migration.

    To estimate the migration cost in man-days, pgORA-Migrate allow you to use a
    configuration directive called ESTIMATE_COST that you can also enabled
    at command line:

            --estimate_cost

    This feature can only be used with the SHOW_REPORT, FUNCTION, PROCEDURE,
    PACKAGE and QUERY export type.

            pgora-migrate -t SHOW_REPORT  --estimate_cost

    The generated report is same as above but with a new 'Estimated cost'
    column as follow:

            ---------------------------------------------
            pgORA-Migrate: Oracle Database Content Report
            ---------------------------------------------
            Version Oracle Database 10g Express Edition Release 10.2.0.1.0
            Schema  HR
            Size  890.00 MB
         
            --------------------------------------
            Object  Number  Invalid Estimated cost  Comments
            --------------------------------------
            FUNCTION  2 0 7 Total size of function code: 369 bytes. HIGH_SALARY: 2, VALIDATE_SSN: 3.
            INDEX 21  0 11  11 index(es) are concerned by the export, others are automatically generated and will do so
                                            on PostgreSQL. 11 b-tree index(es). Note that bitmap index(es) will be exported as b-tree
                                            index(es) if any. Cluster, domain, bitmap join and IOT indexes will not be exported at all.
                                            Reverse indexes are not exported too, you may use a trigram-based index (see pg_trgm) or a
                                            reverse() function based index and search. You may also use 'varchar_pattern_ops', 'text_pattern_ops'
                                            or 'bpchar_pattern_ops' operators in your indexes to improve search with the LIKE operator
                                            respectively into varchar, text or char columns.
            MATERIALIZED VIEW 1 0 3 All materialized view will be exported as snapshot materialized views, they
                                                    are only updated when fully refreshed.
            PACKAGE BODY  0 2 54  Total size of package code: 2487 bytes. Number of procedures and functions found
                                                    inside those packages: 7. two_proc.get_table: 10, emp_mgmt.create_dept: 4,
                                                    emp_mgmt.hire: 13, emp_mgmt.increase_comm: 4, emp_mgmt.increase_sal: 4,
                                                    emp_mgmt.remove_dept: 3, emp_mgmt.remove_emp: 2.
            PROCEDURE 4 0 39  Total size of procedure code: 2436 bytes. TEST_COMMENTAIRE: 2, SECURE_DML: 3,
                                                    PHD_GET_TABLE: 24, ADD_JOB_HISTORY: 6.
            SEQUENCE  3 0 0 Sequences are fully supported, but all call to sequence_name.NEXTVAL or sequence_name.CURRVAL
                                                    will be transformed into NEXTVAL('sequence_name') or CURRVAL('sequence_name').
            TABLE 17  0 8.5 1 external table(s) will be exported as standard table. See EXTERNAL_TO_FDW configuration
                                            directive to export as file_fdw foreign tables or use COPY in your code if you just want to
                                            load data from external files. 2 binary columns. 4 unknow types.
            TRIGGER 1 1 4 Total size of trigger code: 123 bytes. UPDATE_JOB_HISTORY: 2.
            TYPE  7 1 5 5 type(s) are concerned by the export, others are not supported. 2 Nested Tables. 2 Object type.
                                            1 Subtype. 1 Type Boby. 1 Type inherited. 1 Varrays. Note that Type inherited and Subtype are
                                            converted as table, type inheritance is not supported.
            TYPE BODY 0 3 30  Export of type with member method are not supported, they will not be exported.
            VIEW  1 1 1 Views are fully supported, but if you have updatable views you will need to use INSTEAD OF triggers.
            DATABASE LINK 0 0 0 Database links will not be exported. You may try the dblink perl contrib module or
                                                    use the SQL/MED PostgreSQL features with the different Foreign Data Wrapper (FDW) extentions.
            JOB 0 0 0 Job are not exported. You may set external cron job with them.
            --------------------------------------
            Total 65  8 162.5 162.5 cost migration units means approximatively 2 man day(s).

    The last line shows the total estimated migration code in man-days
    following the number of migration units estimated for each object. This
    migration unit represent around five minutes for a PostgreSQL expert. If
    this is your first migration you can get it higher with the
    configuration directive COST_UNIT_VALUE or the --cost_unit_value command
    line option:

            pgora-migrate -t SHOW_REPORT  --estimate_cost --cost_unit_value 10

  Migration assessment method
    Migration unit scores given to each type of Oracle database object are
    defined in the Perl library lib/pgORA-Migrate/PLSQL.pm in the %OBJECT_SCORE
    variable definition.

    The number of PL/SQL lines associated to a migration unit is also
    defined in this file in the $SIZE_SCORE variable value.

    The number of migration units associated to each PL/SQL code
    difficulties can be found in the same Perl library lib/pgORA-Migrate/PLSQL.pm
    in the hash %UNCOVERED_SCORE initialization.

    This assessment method is a work in progress so I'm expecting feedbacks
    on migration experiences to polish the scores/units attribued in those
    variables.

  Hints
    Converting your queries with Oracle style outer join (+) syntax to ANSI
    standard SQL at the Oracle side can save you lot of time for the
    migration. You can use TOAD Query Builder can re-write these using the
    proper ANSI syntax, see:
    http://www.toadworld.com/products/toad-for-oracle/f/10/t/9518.aspx

    There's also an alternative with SQL Developer Data Modeler, see
    http://www.thatjeffsmith.com/archive/2012/01/sql-developer-data-modeler-
    quick-tip-use-oracle-join-syntax-or-ansi/

SUPPORT
  Author / Maintainer
    Denis Lussier <denisl AT pgora DOT com>

    Please report any bugs, patches, help, etc. to <gilles AT darold DOT
    net>.

  Feature request
    If you need new features let me know at <gilles AT darold DOT net>. This
    help a lot to develop a better/useful tool.

  How to contribute ?
    Any contribution to build a better tool is welcome, you just have to
    send me your ideas, features request or patches and there will be
    applied.

LICENSE
    Copyright (c) 2014 pgORA, Inc. - All rights reserved.
    Portions  Copyright (c) 2000-2014 Gilles Darold - All rights reserved.

            This program is free software: you can redistribute it and/or modify
            it under the terms of the GNU General Public License as published by
            the Free Software Foundation, either version 3 of the License, or
            any later version.

            This program is distributed in the hope that it will be useful,
            but WITHOUT ANY WARRANTY; without even the implied warranty of
            MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
            GNU General Public License for more details.

            You should have received a copy of the GNU General Public License
            along with this program.  If not, see < http://www.gnu.org/licenses/ >.

ACKNOWLEDGEMENT
    I thank the great contributors of the Ora2Pg project

