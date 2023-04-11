# 1. Install Prerequisites

### 1.1 Install Chocolatey
[Chocolatey](https://chocolatey.org/) is a software management solution that allows you to manage 100% of your software, anywhere you have Windows, with any endpoint management tool. No other solution (including newly announced solutions) reach this level of management - most only can manage software in Programs and Features.

To install chocolatey open a Powershell terminal as an Administrator user, then copy & paste the next commands:

```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force;
[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072
Invoke-Expression ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
```

### 1.2 Install Powershell Core
[Powershell](https://github.com/Powershell/Powershell) is a cross-platform (Windows, Linux, and macOS) automation and configuration tool/framework that works well with your existing tools and is optimized for dealing with structured data (e.g. JSON, CSV, XML, etc.), REST APIs, and object models. It includes a command-line shell, an associated scripting language and a framework for processing cmdlets.

**Windows**

To install Powershell Core open a Powershell terminal as an Administrator user, then copy & paste the next command:

```powershell
choco install powershell-core
```

At this point your computer should already have the latest version of powershell, close the current terminal and open a new powershell core terminal as an administrator user

### 1.3 Install Git
[Git](https://git-scm.com/) is a free and open source distributed version control system designed to handle everything from small to very large projects with speed and efficiency.

**Windows**

To install Git open a Powershell Core termnal as an Administrator user, then copy & paste the next command:

```powershell
choco install git
```

### 1.4 Configure Git
In order to identify the owner of the different changes made in a Git repository, it is necessary to configure the following settings

```powershell
git config --global user.name "My User Name"
git config --global user.email "myemail@mydomain.com"
```

To allow Git to handle very long file pathnames it is necessary to enable this setting as well. To see other configuration options se the following [link](https://git-scm.com/docs/git-config)

```powershell
git config --global core.longpaths true
```

### 1.5 Install Visual Studio Code
[Visual Studio Code](https://code.visualstudio.com/) is a lightweight but powerful source code editor which runs on your desktop and is available for Windows, macOS and Linux. It comes with built-in support for JavaScript, TypeScript and Node.js and has a rich ecosystem of extensions for other languages and runtimes (such as C++, C#, Java, Python, PHP, Go, .NET).

**Windows**

To install Visual Studio Code open a Powershell Core terminal as an Administrator user, then copy & paste the next command:

```powershell
choco install vscode
```

### 1.6 Configure Visual Studio Code
The features that Visual Studio Code includes out-of-the-box are just the start. [VS Code extensions](https://marketplace.visualstudio.com/VSCode) let you add languages, debuggers, and tools to your installation to support your development workflow. VS Code's rich extensibility model lets extensions authors plug directly into the VS Code UI and contribute functionality through the same APIs used by VS Code.

For this Workshop we are going to work with the following extensions, which will facilitate the development tasks of Azure scripts and templates:

[Azure Account](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account)

[Azure Pipelines](https://marketplace.visualstudio.com/items?itemName=ms-azure-devops.azure-pipelines)

[Azure Resource Manager (ARM) Tools](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)

[Bicep](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-bicep)

[Powershell](https://marketplace.visualstudio.com/items?itemName=ms-vscode.Powershell)

[YAML](https://marketplace.visualstudio.com/items?itemName=redhat.vscode-yaml)

To install the extensions we are going to use the [CLI](https://code.visualstudio.com/docs/editor/command-line) of Visual Studio Code. Open a Powershell Core terminal as an Administrator user, then copy & paste the next commands:

```powershell
code --install-extension ms-vscode.powershell
code --install-extension ms-azure-devops.azure-pipelines
code --install-extension redhat.vscode-yaml
code --install-extension msazurermtools.azurerm-vscode-tools
code --install-extension ms-vscode.azure-account
code --install-extension ms-azuretools.vscode-bicep
```

### 1.7 Install Azure Az Powershell module

[Azure Powershell](https://learn.microsoft.com/en-us/powershell/azure/) is a collection of modules of managing Azure resources from PowerShell.

To install Azure Az Powershell module open a Powershell Core terminal, then copy & paste the next command:

```powershell
Install-Module -Name Az -Scope CurrentUser -Repository PSGallery -Force
```

### 1.8 Install Bicep

[Bicep](https://learn.microsoft.com/en-us/azure/azure-resource-manager/bicep/) is a language for declarativity deploying Azure resources. You can use Bicep instead of JSON for developing your Azure Resource Manager templates (ARM templates).

**Windows**

To install Bicep open a Powershell Core terminal as an Administrator user, then copy & paste the next command:

```powershell
choco install bicep
```
# bootcamp-arroyo
