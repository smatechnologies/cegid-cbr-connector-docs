---
title: Cegid CBR Connector Installation
sidebar_label: Installation
description: "Install and configure the Cegid CBR Connector so OpCon can schedule and monitor Cegid CBR/Y2 jobs on Windows."
tags:
  - Procedural
  - System Administrator
  - Agents
  - Installation
---

# Cegid CBR Connector Installation

## What Is It?

The Cegid CBR Connector installation places the connector software and its required components on a Windows server that runs the Cegid CBR/Y2 application. After installation, OpCon can schedule and monitor Cegid CBR/Y2 jobs through the connector.

- Use this procedure when installing the Cegid CBR Connector for the first time on a Windows server.
- Use this when upgrading to a new connector release that changes the configuration file format or installer structure.

## Supported Software Levels

The following software levels are required to implement version 21.x.x of the Cegid CBR Connector:

| Requirement | Version |
|---|---|
| OpCon | Release 19.0 or higher |
| Java | Embedded OpenJDK (included in installer) |
| Cegid CBR/Y2 | Contact your Cegid representative for the required version |

## How To Install

The installation consists of the following steps:

1. Install the OpCon Windows Agent.
2. Install the Cegid CBR/Y2 Connector.
3. Add the Cegid CBR Connector job subtype to Enterprise Manager.
4. Configure the Cegid CBR/Y2 Connector.

### Step 1 — Install the OpCon Windows Agent

The Cegid CBR Connector requires a Windows Agent installed on the same server as the Cegid CBR/Y2 application. Use an existing Windows Agent if one is already installed, or complete a new Windows Agent installation before proceeding.

### Step 2 — Install the Cegid CBR/Y2 Connector

To install the connector, complete the following steps:

1. Copy the downloaded install file `CegidCBRConnector-win.zip` to a temporary directory (for example, `c:\temp`).
2. Extract the contents, including subdirectories, into the desired installation directory.

After extraction, the root installation directory contains:

| Item | Description |
|---|---|
| `CBRConnector.exe` | The connector executable |
| `Encrypt.exe` | The encryption utility |
| `Connector.config` | The connector configuration file |
| `emplugins\` | Contains the Cegid CBR job subtype plug-in for Enterprise Manager |
| `java\` | Contains the embedded OpenJDK runtime |
| `log\` | Contains connector log files |

### Step 3 — Add the Job Subtype to Enterprise Manager

To add the Cegid CBR/Y2 job subtype to Enterprise Manager, complete the following steps:

1. Copy the Enterprise Manager plug-in from `<installation_dir>\emplugins\` to the `dropins` directory in your Enterprise Manager installation.
   - If the `dropins` directory does not exist, create it in the Enterprise Manager root directory.
2. Restart Enterprise Manager.
   - If the job subtype does not appear, restart Enterprise Manager using **Run as Administrator**.
3. Verify that the **Cegid CBR/Y2** job subtype is visible when you select a Windows job type.

### Step 4 — Create the CBRY2Path Global Property

To configure the global property required by all Cegid CBR/Y2 job definitions, complete the following steps:

1. In OpCon, create a global property named **CBRY2Path**.
2. Set the value to the full path of the connector installation directory.
   - If more than one Cegid CBR Connector is installed on the same server, create an additional global property with a distinct name and update the **Connector Path** field in each affected job definition.

### Step 5 — Configure the Connector

The `Connector.config` file in the installation directory controls connector behavior. Before editing, encrypt any user and password values using `Encrypt.exe`.

#### Encrypt Utility

The `Encrypt.exe` utility uses 64-bit encryption. To encrypt a value, run the following command:

```
Encrypt.exe -v <value>
```

The encrypted output is displayed and ready to paste into the configuration file.

#### Connector.config Settings

| Property | Description | Default |
|---|---|---|
| **[GENERAL SETTINGS]** | — | — |
| `FolderDone` | Root folder for generated completion files (`.DONE`, `.TXT`, `output.xml`). The connector appends `<environment>\<jobid>` to create the working subdirectory. Use forward slashes or double backslashes in the path. | — |
| `FolderLog` | Log file folder. | — |
| `FolderInpXml` | Root folder for the generated `inputfile.xml` and `auth.txt`. The connector appends `<environment>\<jobid>` to create the working subdirectory. | — |
| `FolderAlternateTxt` | Alternate folder for the `.TXT` file, used when older CBR/Y2 versions write the file to a different location. | — |
| `CbpExport` | Full path to the CBPEXPORT executable. | — |
| `CgiMode` | Full path to the CGIMODE executable. | — |
| `PrefixLog` | Log file prefix required by the CBR/Y2 application. | `OPCON-` |
| `ExportOK` | Comma-separated return codes that indicate CBPEXPORT completed successfully. | `0,24` |
| `CgiModeOK` | Return code indicating CGIMODE completed successfully (not the job completion code). | `0` |
| `ExportAddOpt` | Additional options passed to CBPEXPORT. | — |
| `OpConUserid` | Encrypted OpCon user name, used to submit events when `ConsoleDisplay=True`. | — |
| `OpConUserPassword` | Encrypted OpCon event password, used to submit events when `ConsoleDisplay=True`. | — |
| `PollDelayValue` | Seconds between checks for CBPEXPORT or CGIMODE completion. | `5` |
| `PollInitialValue` | Seconds before the first completion check. | `10` |
| `Debug` | Enables trace logging to assist with fault diagnosis. | `OFF` |
| `SmaStatus` | Sends progress messages to OpCon Operations views during execution. | `False` |
| `ConsoleDisplay` | Sends `CONSOLE:DISPLAY` events to OpCon during execution. | `False` |
| **[User Defined RC]** | Maps error description strings to integer completion codes. Used when the `.DONE` file returns completion code 51. Format: `<integer>=<description>`. | — |

#### Example Configuration File

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
OpConUserid=6233426a6232343d
OpConUserPassword=6233426a623235776432513d
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

## FAQs

**Do I need to install Java separately?**

No. The installer bundles an embedded OpenJDK runtime in the `java\` directory. You do not need a separately installed Java version on the server.

**Where should the connector be installed?**

Install the connector on the same Windows server where the Cegid CBR/Y2 application runs. The Windows Agent must also be installed on that server.

**How do I handle path characters in the configuration file?**

Java treats the backslash (`\`) as an escape character. Either double the backslashes (for example, `c:\\test\\cbr`) or use forward slashes (for example, `c:/test/cbr`).

**What happens if the Cegid CBR/Y2 job subtype does not appear in Enterprise Manager after restarting?**

Restart Enterprise Manager using **Run as Administrator**. After that, you can use Enterprise Manager normally without administrator privileges.

**Can I install multiple Cegid CBR Connectors on the same server?**

Yes. Create a separate global property for each installation directory and configure the **Connector Path** field in each job definition to reference the correct property.

## Glossary

**Connector.config** — The configuration file that stores all connector settings, including folder paths, executable locations, and runtime options. Replaces the older `Agent.config` file name.

**CBRY2Path** — A global OpCon property that stores the full installation directory path. Referenced in the **Connector Path** field of every Cegid CBR/Y2 job definition.

**dropins** — An Enterprise Manager directory that contains plug-in files. Placing the job subtype plug-in here registers it with Enterprise Manager on the next restart.

**Encrypt.exe** — The encryption utility bundled with the connector. Encrypts values using 64-bit encryption for use in the `Connector.config` file.

**Global property** — An OpCon variable that stores a value accessible across all job definitions. Used here to store the connector installation path.
