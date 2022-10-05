# Installation

The Cegid-CBR Connector installation consists of multiple steps that are required to complete the installation successfully. 

The connector requires a SMA OpCon Windows Agent to provide the connection between Notification Manager in the OpCon System and the ServiceNow Connector software. 

## Supported Software Levels
The following software levels are required to implement this version (21.x.x) of the Cegid-CBR Connector.

- OpCon Release 19.0 or higher.
- Embedded Java OpenJDK 8 (part of installation).
- Cegid-CBR / Y2 version xxx.

## Installation
The installation process consists of the following steps:

- OpCon Windows Agent Installation.
- Cegid-CBR/Y2 Connector Installation.
- Adding Cegid-CBR Connector job subtype to Enterprise Manager.
- Cegid-CBR/Y2 Connector Configuration.
 
### OpCon Windows Agent Installation
The ServiceNow Connector requires the installation of a Windows Agent on the same system as the Cegid-CBR Connector.
Either use an existing Windows Agent or complete the installation of the Windows Agent.

### Cegid-CBR/Y2 Connector Installation
The Cegid-CBR connector must be installed on the Windows Server that supports the Cegid-CBR/Y2 application. Ensure that the server has an installed OpCon Windows Agent.

Copy the downloaded install file CegidCBRConnector-win.zip and extract it into a temp directory (c:\\temp). Extract the information including sub-directories into the required directory.

After the extraction, the root installation directory contains the connector executable (CBRConnector.exe), the encryption software executable (Encrypt.exe), the Connector.config file and three directories, emplugins, java and log. The emplugins directory contains the Cegid-CBR Job Subtype, java directory contains the java software required to execute the connector (OpenJDK 8) and the log directory contains the connector log files.

### Job Subtype Installation
Copy the Enterprise Manager plug-in from the ***installation_dir***\\emplugins directory to the dropins directory of the Enterprise Manager installation. 
If the dropins directory does not exist, create the dropins directory off the root directory. 

Restart Enterprise Manager and a new Windows job subtype called Cegid CBR/Y2 will be visible.

If not restart Enterprise Manager using 'Run as Administrator'. After this Enterprise Manager can be used normally.

Create a global property **CBRY2Path** that contains the full path of the installation directory.

#### Create CBRY2Path Global Property
Create a global property **CBRY2Path** that contains the full path of the installation directory.

### Cegid-CBR/Y2 Connector Configuration
The configuration of the Cegid CBR/Y2 Connector requires setting the required values in the Connector.config file. The Connector.config file contains information for the Cegid CBR/Y2 Connector that enables the connector to communicate successfully with the CBPEXPORT & CGIMODE programs. 

All user and password values placed in the configuration and template files must be encrypted using the Encrypt.exe utility provided with the connector. 

#### Encrypt Utility
The Encrypt utility uses standard 64 bit encryption.

Supports a -v argument and displays the encrypted value

On Windows, example on how to encrypt the value "abcdefg":

```
Encrypt.exe -v abcdefg

```

#### Connector.config configuration
Configure the Connector.config file in the installation directory setting the required information.
The Connector.config contains the following values


Property Name | Value
--------- | -----------
**[GENERAL SETTINGS]**     | header
**FolderDone**             | This is the root folder for all generated completion files (***file***.DONE, ***file***.TXT & output.xml). The connector appends the environment and jobid arguments to create the working directory for this execution (***FolderDone value***\\***environment argument***\\***jobid argument***). NOTE : The backslash character (\\) is a special Java character and if used should be entered twice (\\\\) (i.e. c:\\\\utilities\\\\output\\\\). Alternatively the slash (/) character can be used instead (i.e. c:/utilities/output/).
**FolderLog**	           |
**FolderInpXml**           | This is the root folder for the generated inputfile.xml and auth.txt files. The connector appends the environment and jobid arguments to create the working directory for this execution (***FolderInpXml value***\\***environment argument***\\***jobid argument***). NOTE : The backslash character (\) is a special Java character and if used should be entered twice (\\\\) (i.e. c:\\\\utilities\\\\output\\\\). Alternatively the slash (/) character can be used instead (i.e. c:/utilities/output/).
**FolderAlternateTxt**     | In some previous versions of the CBR/Y2 application, the ***file***.TXT is not written to the ***FolderDone value***\\***environment***\\***jobid*** folder, but a specific CBR/Y2 folder. If this is the case, then the specific folder must be defined here. NOTE : The backslash character (\\) is a special Java character and if used should be entered twice (\\\\) (i.e. c:\\\\utilities\\\\output\\\\). Alternatively the slash (/) character can be used instead (i.e. c:/utilities/output/).
**CbpExport**              | The full path name to the CBPEXPORT executable. NOTE : The backslash character (\\) is a special Java character and if used should be entered twice (\\\\) (i.e. c:\\\\utilities\\\\output\\\\). Alternatively the slash (/) character can be used instead (i.e. c:/utilities/output/).
**CgiMode**                | The full path name to the CGIMODE executable. NOTE : The backslash character (\\) is a special Java character and if used should be entered twice (\\\\) (i.e. c:\\\\utilities\\\\output\\\\). Alternatively the slash (/) character can be used instead (i.e. c:/utilities/output/).
**PrefixLog**              | Logging must be enabled in the CBR/Y2 application and a prefix must be defined. Default value is **OPCON-**.
**ExportOK**               | The return codes that indicate if the CBPEXPORT program completed successfully. Default value is **0,24**.
**CgiModeOK**              | The return code to indicate that the CGIMODE program executed successfully. This is not the actual completion code as the real completion code is contained in the files generated by the CGIMODE program. Default value = **0**;
**ExportAddOpt**           |	
**OpConUserid**            | The OpCon user that is used to submit events to OpCon if the ConsoleDisplay configuration value is set to True. The OpCon user name must be encrypted using the Encrypt.exe utility. 
**OpConUserPassword**      | The event password of the OpCon user that is used to submit events to OpCon if the ConsoleDisplay configuration value is set to True. The password must be encrypted using the Encrypt.exe utility.
**PollDelayValue**         | The time in seconds between checks to see if the CBPEXPORT or CGIMODE programs have completed execution. Default value is **5** seconds.
**PollInitialValue**       | The time in seconds before the first check is made to see if the CBPEXPORT or CGIMODE programs have completed execution. Default value is **10** seconds.
**Debug**                  | Turns tracing on in the CBRConnector to assist with fault diagnosis. Default value **OFF**.
**SmaStatus**              | Turns on sending status messages to OpCon as the CBRConnector executes providing information on the progress of the execution. Default value **False**.
**ConsoleDisplay**         | Turns on sending CONSOLE:DISPLAY events to OpCon as the CBRConnector executes providing information on the progress of the execution. Default value **False**.
**[User Defined RC]**      | header - Contains a list of values defining error strings and their matching integer values. These values are used if the completion code in the ***file***.DONE is 51. The ***file***.TXT is then scanned for a matching string. If a match is found the integer value is returned to OpCon as the completion code. These values are defined as integer=description pairs.

Example configuration file. 

```

[General Settings]
FolderDone=c:\\test\\cbr\\CEGID
FolderLog=
FolderInpXml=c:\\test\\cbr
FolderAlternateTxt=c:\\test\\cbr\\CEGID\\alternate
CbpExport=c:\\test\\cbr\\CBPEXTRACT.cmd
CgiMode=c:\\test\\cbr\\CGIMODE.cmd
PrefixLog=OPCON-
ExportOK=0,24
CgiModeOK=0
ExportAddOpt=
OpConUserid= 6233426a6232343d
OpConUserPassword= 6233426a623235776432513d
PollDelayValue=10
PollIntervalValue=5
Debug=OFF
SmaStatus=True
ConsoleDisplay=True
[User Defined RC]
-10=erreur fatale * plantage du processus
-11=erreur fatale * interruption forcée du processus
-20=enregistrements comportant des erreurs 
-30=Exercice inexistant sur la 

```
