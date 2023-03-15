#!/bin/bash

# Set Oracle environment variables
export ORACLE_HOME=/path/to/oracle/home
export PATH=$ORACLE_HOME/bin:$PATH
export LD_LIBRARY_PATH=$ORACLE_HOME/lib:$LD_LIBRARY_PATH

# Connect to Oracle database
echo "Enter username: "
read username
echo "Enter password: "
read -s password
echo "Enter database name: "
read dbname

sqlplus -S ${username}/${password}@${dbname} <<EOF
SET PAGESIZE 0
SET FEEDBACK OFF
SET VERIFY OFF
SET HEADING OFF
SET TRIMSPOOL ON
SET LINESIZE 1000
SPOOL table_name.xml
SELECT DBMS_XMLGEN.getXML('SELECT * FROM table_name') FROM dual;
SPOOL OFF
EOF

echo "Table exported to table_name.xml"