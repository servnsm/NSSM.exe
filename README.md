# NSSM.exe

## Introduction

NSSM.exe (Non-Sucking Service Manager) is a Windows utility designed to install, manage, and run regular applications as Windows services. It allows IT administrators and system engineers to configure background processes that start automatically during system boot and continue running without requiring an interactive user session. NSSM is particularly useful for applications that do not provide native Windows service functionality but need reliable, continuous execution on Windows Server and desktop operating systems.

The primary purpose of this Windows service manager is to simplify application deployment and process supervision. NSSM can register executable files, scripts, and command-line applications as system services, configure their startup behavior, and manage their execution through the standard Windows Service Control Manager (SCM). Once installed, a service can be started, stopped, paused where supported, and monitored using familiar Windows administration tools.

One of the main features of NSSM is automatic process recovery. Administrators can configure application restart behavior when a process terminates unexpectedly, helping maintain the availability of critical background workloads. The utility also supports standard input, output, and error stream redirection, allowing applications to write console output and diagnostic messages to log files.

Additional configuration options include service names, display names, descriptions, startup types, service accounts, environment variables, working directories, and process priority. NSSM also provides command-line management, making it suitable for PowerShell scripts, automated deployments, and server provisioning workflows.

Typical use cases include running Python scripts, Node.js applications, Java processes, custom executables, and scheduled background workers as persistent Windows services without rewriting application code to implement the Windows service interface.

## How to Install and Configure NSSM as a Windows Service

NSSM simplifies Windows service installation by allowing administrators to register an existing executable or script as a background service. The installation process does not require modifying the application itself. Before configuration, select the appropriate NSSM executable for the system architecture, place it in a directory accessible to administrators, and ensure that the target application has the required permissions and dependencies.

To install an application as a Windows service, open Command Prompt or PowerShell with administrator privileges and execute:

cmd

```cmd
nssm install MyApplication
```

The graphical configuration interface allows you to specify the application executable, startup directory, and command-line arguments. For example, to run a Python background worker, configure `python.exe` as the application path, specify the script directory as the startup directory, and enter the script filename in the arguments field.

NSSM also supports non-interactive installation through command-line parameters:

cmd

```cmd
nssm install MyApplication "C:\Apps\MyApp\app.exe"
nssm set MyApplication AppDirectory "C:\Apps\MyApp"
nssm set MyApplication DisplayName "My Background Application"
nssm set MyApplication Description "Background application managed by NSSM"
nssm set MyApplication Start SERVICE_AUTO_START
```

After installation, start the service using:

cmd

```cmd
nssm start MyApplication
```

Service status can be checked with:

cmd

```cmd
sc query MyApplication
```

To change the service configuration, use `nssm edit MyApplication` or individual `nssm set` commands. For example, the startup type can be changed to manual when automatic execution during system boot is not required.

When deploying services on Windows Server, verify executable paths, application dependencies, service account permissions, and access to required network resources. These settings help prevent startup failures and ensure that the application operates correctly outside an interactive desktop session.

## NSSM Service Management, Automatic Restart, and Application Logging

NSSM provides process management capabilities that help administrators maintain long-running applications as Windows services. One of its most important functions is automatic application recovery. If a managed process exits unexpectedly, NSSM can respond according to the configured exit actions and restart policy. This is useful for API workers, data processing applications, monitoring agents, and other continuously running processes.

For example, automatic restart can be configured with:

cmd

```cmd
nssm set MyApplication AppExit Default Restart
nssm set MyApplication AppRestartDelay 5000
```

The restart delay is specified in milliseconds. A value of `5000` introduces a five-second delay before restarting the application. Administrators can also define different exit actions for specific process exit codes, allowing normal termination and unexpected failures to be handled differently.

Application output redirection is another important feature for troubleshooting Windows services. Since background services do not normally display an interactive console, redirecting standard output and standard error makes diagnostic information available in log files.

Configure logging with:

cmd

```cmd
nssm set MyApplication AppStdout "C:\Logs\MyApplication-output.log"
nssm set MyApplication AppStderr "C:\Logs\MyApplication-error.log"
```

NSSM also provides options for configuring log rotation, including file-size-based rotation and timestamped log filenames. Administrators should ensure that the service account has write permissions to the logging directory and that log retention is managed to prevent unnecessary disk usage.

To stop or remove a service, execute:

cmd

```cmd
nssm stop MyApplication
nssm remove MyApplication confirm
```

Removing a service registration does not automatically uninstall the application files. For production environments, administrators should verify service status, review application logs, and test restart behavior before deploying configuration changes across multiple Windows servers.
