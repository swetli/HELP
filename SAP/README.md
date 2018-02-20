# SAP BASIS TRAINING
ON HDB host as sudo - <sid>adm:
HDB info - shows status
HDB version - shows HANA version
HDB start  - start HANA DB
HDB stop ( must be done before stopping the VM )

ON APP host as sudo - <sid>adm:

R3trans -d - to see if the DB is running
````
startsap all or startsap - custom
sapcontrol -nr 00 -function GetProcessList
sapcontrol -nr 01 -function GetProcessList
stopsap all or stopsap
sapcontrol -nr <NN> -host <host/ip> -function GetSystemInstanceList # TO check on remote host
sapcontrol -nr <NN> -host <host/ip> -function StartSystem ALL
sapcontrol -nr <NN> -host <host/ip> -user <sid>adm <password> -function StartSystem ALL
````
# To see license
saplikey -get pf=/usr/sap/VSC/SYS/profile/DEFAULT.PFL ( to get the hw key )
saplikey -show pf=/usr/sap/VSC/SYS/profile/DEFAULT.PFL

GUI:

http://<APP-SERVER>:8000/sap/bc/gui/sap/its/webgui user DDIC/Password1
in GUI - /nslicense or /oslicense

# SAP BASIS TRAINING
SAP SYSTEM 3 LAYERS:
- PRESENTATION - GUI
- APPLICATION - JAVA APP SERVER or ABAP APP SERVER
- DATABASE

ABAP APP SERVER:
DISPATCHER / GATEWAY and SHARED MEMORY between processes
Dispatcher - gives tasks to WORK PROCESS
IF we have multiple APP systems there is a message server
ICM = Internet communicaton manager
on 7.0 or lower We have ABAP Central instance and multiple ABAP scalability instances

7.1+
we have only one ASCS ( ABAP System central server)
central instance renamed to Primary App server ( PAS )
the former dialog instance was renamed to Additional app server ( AAS )
JAva dispatchersis replaced by ICM process


NETWEAVER JAVA APP SERVER:
There is no dispatcher
server process = work process in the abap architecture

JAVA-based SAP 7.1+:
more or less the same as ABAP

SAP system config
env sap kernel overridden by default profile or <instance profile (SID_instance_hoshtname)
Most changes require restart of the sap system

Operation mode : day vs night diaglog vs background processes

DB->PAS->SAS

## ABAP SAP TRANSACTIONS
````
SM21 - See logs of the SAP system - F8.
SM20 - See audit logs of the SAP system - F8.
ST22 - See application errors. 
SM50 - See table of work process in current instances.
SM51 - See all instances in the SAP system.
SM66 - See work processes of all instances.
SM37 - See filtered list of all job executed in the SAP system.
SM04 - See list of logged users in current instance.
AL08 - See list of logged users in all instances.
SM12 - See and delete data locks - F8.
SM13 - See update request and failed updates.
SU01 - User maintenance 
DBACOCKPIT - Manage and monitor SAP database
RZ10 - Edit system profiles
````

## To stop SAP system:
1. Check SM50 - to see if there are any working processes
2. Check SM37 - to see the progress of the active job
3. Check SM04/AL08 - for active logged in users
4. Lock data SM12/13
5. Send a system message SM02 - to warn users for stopping the system.

## To start SAP system:
1. Check SM50/51 - to see all working processes are running
2. Check SM22 - for any errors
3. Check system log SM21 - good practice for something unusal