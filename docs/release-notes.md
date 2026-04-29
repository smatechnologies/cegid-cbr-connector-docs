---
sidebar_label: 'Release Notes'
title: Cegid CBR Connector Release Notes
description: "Version history and change details for the Cegid CBR Connector, including new features, improvements, and bug fixes."
tags:
  - Reference
  - Automation Engineer
  - Agents
---

# Cegid CBR Connector Release Notes

## 21

### 21.0.0

#### What's new

:eight_spoked_asterisk: Removed log4j and replaced it with slf4j and logback.

:eight_spoked_asterisk: New format installer extracts files from the zip archive into the desired directory.

:eight_spoked_asterisk: Embedded Java (OpenJDK) is now bundled with the connector, removing the dependency on installed Java versions.

:eight_spoked_asterisk: New encryption capabilities added. Use the `Encrypt.exe` utility to encrypt configuration values.

:eight_spoked_asterisk: Configuration file renamed from `Agent.config` to `Connector.config`.

#### Why this matters

Replacing log4j with slf4j and logback removes the log4j vulnerability exposure. Bundling an embedded Java runtime simplifies deployment and eliminates version conflicts with system-installed Java. The new encryption mechanism and renamed configuration file align the connector with current SMA Technologies security and configuration standards.

#### Migration considerations

- Extract the new installer zip into the desired installation directory.
- Rename your existing `Agent.config` to `Connector.config` before configuring the connector.
- Re-encrypt all user and password values using the new `Encrypt.exe` utility.
