# Operation

The Enterprise Manager includes job subtype definitions for the Cegid CBR/Y2 Application. The job subtype can be accessed by selecting the Cegid CBR/Y2 job subtype from the drop down list when the Windows Job Type has been selected. 

## Cegid CBR/Y2 Job definitions
The Cegid CBR/Y2 Definition, defines the database name, the job id and the failure criteria that are used to start a CBR/Y2 job.

When defining a Cegid CBR/Y2 job, select a Job Type of Windows and then a Job Sub-Type of Cegid CBR/Y2. 

The Cegid CBR/Y2 Definition screen will then appear.

Field | Description
--------- | -----------
**User Id**          | Required field, defining the Windows batch user that the connector will be executed under.
**Connector Path**   | Required field that contains the installed location of the Cegid CBR/Y2 Connector. This consists of a global property value which contains the root installation directory. Default value is **CBRY2Path**. If more than one Orli Connectors is installed on the same system, then an additional global property should be defined and the entry in this field updated. 
**Database Name**    | Required field that contains the name of the database to be used when starting the job (this equates to the CBR/Y2 environment).
**Job ID Number**    | Required field and defines the id of the job definition within the CBR/Y2 application to start.
**Domain**           | Optional field that defines a domain value that is passed to the CbrExport.
**Other**            | Not currently used.

In general the successful completion code for a job is 0. However, for older systems it is possible that this value maybe defined as another integer. This value would be defined in the Agent.configuration (see ***User Defined RC*** section).

## Job Finished processing 

Code | Text value | Description
-----| ----------------|---------------
0    | JOB_FINISHED_OK | The job completed processing successfully.
1    | JOB_FAILED      | The job failed.
100  |                 | CBPEXPORT Job returned a failure condition.
101  |                 | The CBPEXPORT or CGIMODE JOB was cancelled.
102  |                 | The inputfile.xml was not found by the CGIMODE job.
103  |                 | The inputfile.xml does not contain an ACTION value.
104  |                 | The auth.txt file was not found.
105  |                 | The defined completion description was not found in the ***file***.TXT file.

This means that to check for a successful completion, the Failure Criteria should be set to **Not Equal To**, the Value to **0** and the Result to **Fail**. 
Otherwise the Value should be set to the integer defined for successful completion in the ***User Defined RC*** section.

## Logging
The default logging implemented by the connector consists of a maximum cycle of five log files. The log files contain information about the Cegid CBR/Y2 Connector and any jobs run by the Cegid CBR/Y2 Connector. The log files (Log.log - Log.log.5) are located in the ***installation_dir_***\\log directory. Information is appended into the log files and any error messages, return codes can be viewed in these log files.

## Job Output
The job output contains information about the CBRConnector execution as well as the execution of the CBPEXPORT and the CGIMODE programs. It includes the following information:
•	CBRConnector execution messages.
•	CBPEXPORT stdout & stderr files.
•	CGIMODE stdout & stderr files.
•	OPCON-***timestamp***.DONE file.
•	OPCON-***timestamp***.TXT file.

```

14:00:30.102 [main] [CBRConnector] 20161229 14:00:30 : ------------------------------------------------------------------------------
14:00:30.102 [main] [CBRConnector] 20161229 14:00:30 : CBR Connector           : 16.02.01.00
14:00:30.102 [main] [CBRConnector] 20161229 14:00:30 : ------------------------------------------------------------------------------
14:00:30.102 [main] [CBRConnector] 20161229 14:00:30 : -e  (environment)       : DBTEST
14:00:30.102 [main] [CBRConnector] 20161229 14:00:30 : -j  (jobid)             : 1234
14:00:30.102 [main] [CBRConnector] 20161229 14:00:30 : ------------------------------------------------------------------------------
14:00:30.102 [main] [CBRJobExecutorImpl] 20161229 14:00:30 : Performing CBPEXPORT parameters -DB DBTEST -TASKID 1234 -OUTPUTFOLDER c:\test\cbr\DBTEST\1234 -FOLDER DBTEST
14:00:55.150 [main] [CBRJobExecutorImpl] 20161229 14:00:55 : ------------------------------------------------------------------------------
14:00:55.150 [main] [CBRJobExecutorImpl] 20161229 14:00:55 : Generating XML
14:00:55.150 [main] [CBRJobExecutorImpl] 20161229 14:00:55 : ------------------------------------------------------------------------------
14:00:55.150 [main] [CBRJobExecutorImpl] 20161229 14:00:55 : ------------------------------------------------------------------------------
14:00:55.150 [main] [CBRJobExecutorImpl] 20161229 14:00:55 : CBPEXPORT STDOUT : -----------------------------------------------------------
14:00:55.150 [main] [CBRJobExecutorImpl] 20161229 14:00:55 : 
14:00:55.150 [main] [CBRJobExecutorImpl] 20161229 14:00:55 : C:\test\connector>cd \test\cbr 
14:00:55.150 [main] [CBRJobExecutorImpl] 20161229 14:00:55 : 
14:00:55.150 [main] [CBRJobExecutorImpl] 20161229 14:00:55 : C:\test\cbr>CBPEXTRACT.exe -DB DBTEST -TASKID 1234 -OUTPUTFOLDER c:\test\cbr\DBTEST\1234 -FOLDER DBTEST 
14:00:55.150 [main] [CBRJobExecutorImpl] 20161229 14:00:55 : DB           DBTEST
14:00:55.150 [main] [CBRJobExecutorImpl] 20161229 14:00:55 : TASKID       1234
14:00:55.150 [main] [CBRJobExecutorImpl] 20161229 14:00:55 : OUTPUTFOLDER c:\test\cbr\DBTEST\1234
14:00:55.150 [main] [CBRJobExecutorImpl] 20161229 14:00:55 : FOLDER       DBTEST
14:00:55.150 [main] [CBRJobExecutorImpl] 20161229 14:00:55 : Creating inputfile.xml in directory c:\test\cbr\DBTEST\1234
14:00:55.150 [main] [CBRJobExecutorImpl] 20161229 14:00:55 : ------------------------------------------------------------------------------
14:00:55.150 [main] [CBRJobExecutorImpl] 20161229 14:00:55 : CBPEXPORT STDERR : -----------------------------------------------------------
14:00:55.150 [main] [CBRJobExecutorImpl] 20161229 14:00:55 : ------------------------------------------------------------------------------
14:00:55.150 [main] [CBRJobExecutorImpl] 20161229 14:00:55 : CBPEXPORT completed successfully with return code 0
14:00:55.197 [main] [CBRJobExecutorImpl] 20161229 14:00:55 : ------------------------------------------------------------------------------
14:00:55.197 [main] [CBRJobExecutorImpl] 20161229 14:00:55 : executing CGI mode
14:00:55.197 [main] [CBRJobExecutorImpl] 20161229 14:00:55 : ------------------------------------------------------------------------------
14:00:55.197 [main] [CBRJobExecutorImpl] 20161229 14:00:55 : Performing CGIMODE parameters -i c:\test\cbr\DBTEST\1234\inputfile.xml -o c:\test\cbr\CEGID\DBTEST\1234\output.xml -a Purge -auth c:\test\cbr\DBTEST\1234\auth.txt
14:01:40.270 [main] [CBRJobExecutorImpl] 20161229 14:01:40 : ------------------------------------------------------------------------------
14:01:40.270 [main] [CBRJobExecutorImpl] 20161229 14:01:40 : CGIMODE STDOUT   : -----------------------------------------------------------
14:01:40.270 [main] [CBRJobExecutorImpl] 20161229 14:01:40 : 
14:01:40.270 [main] [CBRJobExecutorImpl] 20161229 14:01:40 : C:\test\connector>cd \test\cbr 
14:01:40.270 [main] [CBRJobExecutorImpl] 20161229 14:01:40 : 
14:01:40.270 [main] [CBRJobExecutorImpl] 20161229 14:01:40 : C:\test\cbr>CGIMODE.exe -i c:\test\cbr\DBTEST\1234\inputfile.xml -o c:\test\cbr\CEGID\DBTEST\1234\output.xml -a Purge -auth c:\test\cbr\DBTEST\1234\auth.txt 
14:01:40.270 [main] [CBRJobExecutorImpl] 20161229 14:01:40 : i    c:\test\cbr\DBTEST\1234\inputfile.xml
14:01:40.270 [main] [CBRJobExecutorImpl] 20161229 14:01:40 : o    c:\test\cbr\CEGID\DBTEST\1234\output.xml
14:01:40.270 [main] [CBRJobExecutorImpl] 20161229 14:01:40 : a    Purge
14:01:40.270 [main] [CBRJobExecutorImpl] 20161229 14:01:40 : auth c:\test\cbr\DBTEST\1234\auth.txt
14:01:40.270 [main] [CBRJobExecutorImpl] 20161229 14:01:40 : Creating .DONE file {c:\test\cbr\CEGID\DBTEST\1234\OPCON-2016-12-29-14-01-35-382-TACHE-1234.DONE}
14:01:40.270 [main] [CBRJobExecutorImpl] 20161229 14:01:40 : Creating .TXT file {c:\test\cbr\CEGID\alternate\OPCON-2016-12-29-14-01-35-382-TACHE-1234.TXT}
14:01:40.270 [main] [CBRJobExecutorImpl] 20161229 14:01:40 : ------------------------------------------------------------------------------
14:01:40.270 [main] [CBRJobExecutorImpl] 20161229 14:01:40 : CGIMODE STDERR   : -----------------------------------------------------------
14:01:40.270 [main] [CBRJobExecutorImpl] 20161229 14:01:40 : ------------------------------------------------------------------------------
14:01:40.270 [main] [CBRJobExecutorImpl] 20161229 14:01:40 : searching for LOG File
14:01:40.270 [main] [CBRJobExecutorImpl] 20161229 14:01:40 : Checking file (c:\test\cbr\CEGID\DBTEST\1234\OPCON-2016-12-29-14-01-35-382-TACHE-1234.DONE) for completion code
14:01:40.270 [main] [CBRJobExecutorImpl] 20161229 14:01:40 : ------------------------------------------------------------------------------
14:01:40.270 [main] [CBRJobExecutorImpl] 20161229 14:01:40 : CGIMODE FILE c:\test\cbr\CEGID\DBTEST\1234\OPCON-2016-12-29-14-01-35-382-TACHE-1234.DONE
14:01:40.270 [main] [CBRJobExecutorImpl] 20161229 14:01:40 : Cegid Business Mode
14:01:40.270 [main] [CBRJobExecutorImpl] 20161229 14:01:40 : 2015-04-28
14:01:40.270 [main] [CBRJobExecutorImpl] 20161229 14:01:40 : 10.26.23
14:01:40.270 [main] [CBRJobExecutorImpl] 20161229 14:01:40 : 2016-12-29
14:01:40.270 [main] [CBRJobExecutorImpl] 20161229 14:01:40 : 14.01.35
14:01:40.270 [main] [CBRJobExecutorImpl] 20161229 14:01:40 : CGIMODE
14:01:40.270 [main] [CBRJobExecutorImpl] 20161229 14:01:40 : 0
14:01:40.270 [main] [CBRJobExecutorImpl] 20161229 14:01:40 : TÃ¢che 1234 - 
14:01:40.270 [main] [CBRJobExecutorImpl] 20161229 14:01:40 : EXPORT_DONNEES
14:01:40.270 [main] [CBRJobExecutorImpl] 20161229 14:01:40 : 51
14:01:40.270 [main] [CBRJobExecutorImpl] 20161229 14:01:40 : Fichier gÃ©nÃ©rÃ©
14:01:40.270 [main] [CBRJobExecutorImpl] 20161229 14:01:40 : ------------------------------------------------------------------------------
14:01:40.270 [main] [CBRJobExecutorImpl] 20161229 14:01:40 : CGIMODE FILE c:\test\cbr\CEGID\alternate\OPCON-2016-12-29-14-01-35-382-TACHE-1234.TXT
14:01:40.270 [main] [CBRJobExecutorImpl] 20161229 14:01:40 : erreur fatale * interruption forcÃ©e du processuss
14:01:40.270 [main] [CBRJobExecutorImpl] 20161229 14:01:40 : TÃ¢che 1234 :  
14:01:40.270 [main] [CBRJobExecutorImpl] 20161229 14:01:40 : Export du 28/04/2015 10:26:24
14:01:40.270 [main] [CBRJobExecutorImpl] 20161229 14:01:40 :  
14:01:40.270 [main] [CBRJobExecutorImpl] 20161229 14:01:40 : Fichier : C:\Tmp\Export_articles20150428_1026_24525.ASC
14:01:40.270 [main] [CBRJobExecutorImpl] 20161229 14:01:40 : GÃ©nÃ©ration du fichier le 28/04/2015 10:26:24
14:01:40.270 [main] [CBRJobExecutorImpl] 20161229 14:01:40 :  
14:01:40.270 [main] [CBRJobExecutorImpl] 20161229 14:01:40 : Export du 28/04/2015 10:26:24
14:01:40.270 [main] [CBRJobExecutorImpl] 20161229 14:01:40 :  
14:01:40.270 [main] [CBRJobExecutorImpl] 20161229 14:01:40 : Fichier : C:\Tmp\Export_articles20150428_1026_24525.ASC
14:01:40.270 [main] [CBRJobExecutorImpl] 20161229 14:01:40 : ------------------------------------------------------------------------------
14:01:40.270 [main] [CBRJobExecutorImpl] 20161229 14:01:40 : CGIMODE completed with return code -11
14:01:40.270 [main] [CBRJobExecutorImpl] 20161229 14:01:40 : ------------------------------------------------------------------------------
14:01:40.285 [main] [CBRConnector] 20161229 14:01:40 : CBR Connector Completion code : -11

```