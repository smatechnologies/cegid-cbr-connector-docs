---
title: Cegid CBR Connector Operation
sidebar_label: Operation
description: "Define and run Cegid CBR/Y2 jobs in OpCon, understand completion codes, and review connector log output."
tags:
  - Procedural
  - Automation Engineer
  - Operations Staff
  - Agents
---

# Cegid CBR Connector Operation

## What Is It?

The Cegid CBR Connector operation section covers how to define Cegid CBR/Y2 jobs in OpCon Enterprise Manager, interpret completion codes, and review job output and logs.

- Use this when you need to create or update a Cegid CBR/Y2 job definition in Enterprise Manager.
- Use this when diagnosing a job failure by reviewing completion codes or connector log output.

## Cegid CBR/Y2 Job Definitions

The Cegid CBR/Y2 job subtype is available in Enterprise Manager when a Windows job type is selected.

To define a Cegid CBR/Y2 job, complete the following steps:

1. In Enterprise Manager, open the **Job Master**.
2. Select a **Job Type** of **Windows**.
3. Select a **Job Sub-Type** of **Cegid CBR/Y2**.
4. Complete the fields in the Cegid CBR/Y2 Definition screen.

### Job Definition Fields

| Field | Required | Description |
|---|---|---|
| **User ID** | Yes | The Windows batch user that the connector runs under. |
| **Connector Path** | Yes | The installed location of the Cegid CBR/Y2 Connector. References the `CBRY2Path` global property by default. Update this field if you have defined a different property name for a multi-instance installation. |
| **Database Name** | Yes | The name of the database to use when starting the job. This value maps to the CBR/Y2 environment. |
| **Job ID Number** | Yes | The ID of the job definition within the CBR/Y2 application. |
| **Domain** | No | An optional domain value passed to the CBPEXPORT program. |
| **Other** | No | Not currently used. |

### Setting Failure Criteria

The standard successful completion code for a Cegid CBR/Y2 job is `0`. For older systems, a different integer may indicate success — check the **User Defined RC** section of `Connector.config` for your environment.

To configure failure criteria for a standard installation, set the **Failure Criteria** to **Not Equal To**, the **Value** to `0`, and the **Result** to **Fail**.

## Completion Codes

The following completion codes are returned by the Cegid CBR Connector to OpCon:

| Code | Text Value | Description |
|---|---|---|
| `0` | `JOB_FINISHED_OK` | The job completed successfully. |
| `1` | `JOB_FAILED` | The job failed. |
| `100` | — | CBPEXPORT returned a failure condition. |
| `101` | — | The CBPEXPORT or CGIMODE job was cancelled. |
| `102` | — | The `inputfile.xml` was not found by the CGIMODE job. |
| `103` | — | The `inputfile.xml` does not contain an ACTION value. |
| `104` | — | The `auth.txt` file was not found. |
| `105` | — | The defined completion description was not found in the `.TXT` file. |

## Logging

The connector writes log output to a cycle of five log files in the `<installation_dir>\log\` directory:

- **Log.log** — current log file
- **Log.log.1** through **Log.log.5** — previous log files (oldest is overwritten)

Log files contain execution messages, return codes, and error details for each connector run.

## Job Output

The OpCon job log for a Cegid CBR/Y2 job includes the following information:

- CBRConnector execution messages
- CBPEXPORT stdout and stderr output
- CGIMODE stdout and stderr output
- Contents of the `OPCON-<timestamp>-TACHE-<jobid>.DONE` file
- Contents of the `OPCON-<timestamp>-TACHE-<jobid>.TXT` file

### Example Job Output

```
14:00:30.102 [main] [CBRConnector] 20161229 14:00:30 : CBR Connector           : 16.02.01.00
14:00:30.102 [main] [CBRConnector] 20161229 14:00:30 : -e  (environment)       : DBTEST
14:00:30.102 [main] [CBRConnector] 20161229 14:00:30 : -j  (jobid)             : 1234
14:00:30.102 [main] [CBRJobExecutorImpl] 20161229 14:00:30 : Performing CBPEXPORT parameters -DB DBTEST -TASKID 1234 -OUTPUTFOLDER c:\test\cbr\DBTEST\1234 -FOLDER DBTEST
14:00:55.150 [main] [CBRJobExecutorImpl] 20161229 14:00:55 : CBPEXPORT completed successfully with return code 0
14:00:55.197 [main] [CBRJobExecutorImpl] 20161229 14:00:55 : executing CGI mode
14:00:55.197 [main] [CBRJobExecutorImpl] 20161229 14:00:55 : Performing CGIMODE parameters -i c:\test\cbr\DBTEST\1234\inputfile.xml -o c:\test\cbr\CEGID\DBTEST\1234\output.xml -a Purge -auth c:\test\cbr\DBTEST\1234\auth.txt
14:01:40.270 [main] [CBRJobExecutorImpl] 20161229 14:01:40 : CGIMODE completed with return code -11
14:01:40.285 [main] [CBRConnector] 20161229 14:01:40 : CBR Connector Completion code : -11
```

## FAQs

**What does completion code 100 mean?**

Code `100` means the CBPEXPORT program returned a failure condition. Check the `ExportOK` setting in `Connector.config` to confirm which return codes are considered successful for your CBPEXPORT version. The default values are `0` and `24`.

**What does completion code 105 mean?**

Code `105` means the CGIMODE program returned code `51` and the `.TXT` file did not contain a matching string from the **User Defined RC** section of `Connector.config`. Verify that all expected error description strings are defined in that section.

**Where do I find the raw CBPEXPORT and CGIMODE output?**

The full stdout and stderr for both programs is appended to the OpCon job log. Open the job in the Operations view and select **View Job Output**.

**How do I know if a job completed successfully?**

The connector returns `0` to OpCon for a successful job. Set the **Failure Criteria** to **Not Equal To 0** in the job definition. For older systems where a different code indicates success, verify the **User Defined RC** section and adjust the failure criteria accordingly.

**Why does the connector check timestamps before reading the .DONE file?**

The connector takes an initial timestamp before calling CGIMODE and compares it to the date and time embedded in the `.DONE` file. This comparison confirms that the `.DONE` file was generated by the current run, not a previous one.

## Glossary

**Failure criteria** — The condition in an OpCon job definition that determines whether a job run is marked as failed. For Cegid CBR/Y2 jobs, this is typically set to **Not Equal To 0**.

**Job Sub-Type** — An Enterprise Manager field that selects the specialized input form for a specific connector. For Cegid CBR/Y2 jobs, select **Cegid CBR/Y2** after selecting the **Windows** job type.

**Job ID Number** — The numeric identifier of a job definition within the Cegid CBR/Y2 application, passed to the connector as the `-j` argument.

**Database Name** — The Cegid CBR/Y2 environment name, passed to the connector as the `-e` argument.

**User Defined RC** — A `Connector.config` section that maps Cegid CBR/Y2 error description strings to integer return codes. Used when the `.DONE` file contains completion code `51`.

**.DONE file** — A file generated by CGIMODE that contains the job completion date, time, and completion code. The connector reads this file to determine the final job status.

**.TXT file** — A file generated by CGIMODE that contains descriptive completion text. The connector scans this file for matching strings when the `.DONE` file contains completion code `51`.
