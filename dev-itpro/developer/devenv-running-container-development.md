---
title: "Running a Container-Based Development Environment"
description: "Overview of the running container-based development."
author: SusanneWindfeldPedersen
ms.custom: na
ms.date: 04/01/2021
ms.reviewer: na
ms.topic: conceptual
ms.author: solsen
---

# Running a Container-Based Development Environment
[!INCLUDE[d365fin_long_md](includes/d365fin_long_md.md)] is available as artifacts for running on Docker on a Windows system with Docker installed.


> [!TIP]  
> Use the `Get-BCArtifactUrl` from the BCContainerHelper PowerShell module to get the artifact URL for the version of [!INCLUDE[d365fin_long_md](includes/d365fin_long_md.md)] that you want.

## Install and configure Docker
Install Docker and configure it for Windows Containers.

1. Please choose the version of Docker that is appropriate for the host operating system.

    - For Windows 10, use [Docker Community Edition]( https://hub.docker.com/editions/community/docker-ce-desktop-windows).
        For more information, see [Install instructions](/virtualization/windowscontainers/quick-start/quick-start-windows-10).
    - For Windows Server, use [Docker Enterprise Edition](https://hub.docker.com/editions/enterprise/docker-ee-server-windows).
        For more information, see [Install instructions](/virtualization/windowscontainers/quick-start/quick-start-windows-server).

2. For Windows 10, switch Docker to use Windows containers. By default Docker Community Edition uses Linux containers.

    To switch to Windows containers, in the Taskbar, right-click the Docker icon ![Docker](media/docker-icon.png "Docker icon"), and then select **Switch to Windows Containers**. For more information, see [Switch between Windows and Linux containers](https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers).

> [!NOTE]
> You can run Business Central on Docker using Docker commands, or you can use the BCContainerHelper PowerShell module. The BCContainerHelper module removes a lot of the complexity of running Docker.

## Using Docker commands
Open a Command Prompt as Administrator. In the command prompt, identify your version of Windows (example 10.0.19041.329). Run this command to pull the latest version of the generic image used to to run Business Central on Docker:

```docker pull mcr.microsoft.com/businesscentral:10.0.19041.329```

Use this command to run a sandbox container with the US localization of version 16.3.14085.14363 on Docker:

```docker run -e accept_eula=Y -m 4G -e artifacturl=https://bcartifacts.azureedge.net/sandbox/16.3.14085.14363/us mcr.microsoft.com/businesscentral:10.0.19041.329```

> [!IMPORTANT]  
> You must specify the correct Windows Version in the generic image name. If your version of Windows doesn't have a corresponding generic Docker image, you might need to use Hyper-V isolation.

After starting the `docker run` command above, you will see log entries similar to the following:

```
Initializing...
Starting Container
Hostname is b8c6941bb168
...
Container IP Address: 172.21.4.208
Container Hostname  : b8c6941bb168
Container Dns Name  : b8c6941bb168
Web Client          : https://b8c6941bb168/BC/
Admin Username      : admin
Admin Password      : Zupa5925
Dev. Server         : https://b8c6941bb168
Dev. ServerInstance : BC

Files:
http://b8c6941bb168:8080/ALLanguage.vsix
http://b8c6941bb168:8080/certificate.cer

Initialization took 66 seconds
Ready for connections!
```

At this point, you can open your browser and type in the Web client URL from the log. You will be prompted to log in with the Admin Username/Password that is shown.

> [!NOTE]  
> The container image uses a so called self-signed certificate for HTTPS communication. Because of that, your browser might warn you that the page you are requesting is unsafe. In those specific circumstances, and only for test and development environments, it is safe to ignore this warning. If you want to resolve this warning, you can install the certificate on your PC. For more information, see the link under **Files** in the log entries.

## Using the BCContainerHelper PowerShell module
To support the use of containers, optional PowerShell scripts are available, which support setup of development environments. Use the `BCContainerHelper` to work with containers. On a Windows 10, Windows Server 2016 or Windows server 2019 machine, start Powershell as an Administrator and type:

```install-module BCContainerHelper -force```

To see which functions are available in the BCContainerHelper module use the following command:

```Write-BCContainerHelperWelcomeText```

To get quickly get started, run the following command from the BCContainerHelper module:

```
$artifactUrl = Get-BcArtifactUrl -type sandbox -country us -select Latest
New-BCContainer -accept_eula -containerName mysandbox -artifactUrl $artifactUrl
```

The `BCContainerHelper` will create a folder on the C:\ drive called *bcartifacts.cache* for caching artifacts. It will also create a folder under C:\ProgramData called BCContainerHelper and will place all working files underneath that folder. The C:\ProgramData\BCContainerHelper folder will be shared to the container for transfer of files etc. If you do not specify a username and a password, it will ask for your password and use the current Windows username. If you specify your windows password, the container setup will use Windows Authentication integrated with the host. The `BCContainerHelper` will also create shortcuts on the desktop for the [!INCLUDE[d365fin_long_md](includes/d365fin_long_md.md)] Web client, a container prompt, and a container PowerShell prompt.

The `BCContainerHelper` module also allows you to add the `-includeCSide` switch (For Business Central versions 14 or earlier) in order to add the [!INCLUDE[d365fin_long_md](includes/d365fin_long_md.md)] Windows client and C/SIDE to the desktop and export all objects to a folder underneath C:\ProgramData\BCContainerHelper\Extensions for the object handling functions from the module to work.

## See Also
[Get Started with AL](devenv-get-started.md)  
[Get started with the Container Sandbox Development Environment](devenv-get-started-container-sandbox.md)  
[Keyboard Shortcuts](devenv-keyboard-shortcuts.md)  
[AL Development Environment](devenv-reference-overview.md)  
[FAQ for Developing in AL](devenv-dev-faq.md)  
