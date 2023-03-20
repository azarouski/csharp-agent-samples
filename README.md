# csharp-agent-samples

---
Your guide to run the first C# NUnit test with reporting to Zebrunner.

---

<!-- TOC -->
* [csharp-agent-samples](#csharp-agent-samples)
  * [Prerequisites](#prerequisites)
  * [Configuration](#configuration)
    * [_Step 1: Setup your authentication_](#step-1-setup-your-authentication)
    * [_Step 2: Select project for your launches_](#step-2-select-project-for-your-launches)
    * [_Step 3: Configure your system if needed_](#step-3-configure-your-system-if-needed)
    * [_Step 4: Configure your environment_](#step-4-configure-your-environment)
    * [_Step 5: Build the project_](#step-5-build-the-project)
    * [_Step 6: Initiate source_](#step-6-initiate-source)
    * [_Step 7: Run sample test_](#step-7-run-sample-test)
<!-- TOC -->

---

# Prerequisites

Before you start performing NUnit automation testing with Zebrunner, you would need to be installed:

#### _On Windows:_

- [.NET SDK 6.0](https://dotnet.microsoft.com/en-us/download/dotnet/6.0)
- [.NET Framework 4.8](https://dotnet.microsoft.com/en-us/download/dotnet-framework/net48)

#### _On Linux and MacOs:_

- [Mono 6.10.0 or higher](https://www.mono-project.com/download/stable/)

> Please note that on different systems and it's versions sets of commands may vary

---

# Configuration

### _Step 1: Setup your authentication_

##### _In Zebrunner:_

- Navigate to "Account and profile" section by clicking on the User icon from the top right side;
- Click on "API Tokens" tab;
- Press "Token" button, create a token and copy it before closing the dialog (you won't be able to see the token later).

### _Step 2: Select project for your launches_

##### _In Zebrunner:_

- Click on "Projects" dropdown from the top left side;
- Select "View all Projects";
- Find out a project where you would like to see your launches and copy its `KEY`.

> You need to be an Admin of the workspace to have the ability to create projects

### _Step 3: Configure your system if needed_

Install necessary certificates and permissions

#### _On Windows and MacOs:_

- You can skip this step

#### _On Linux_:

- Follow appropriate [documentation](https://www.mono-project.com/download/stable/#download-lin)

<details><summary style="font-style: italic">If you have some troubles with newest Ubuntu releases try out code below</summary><p>

```
sudo apt install -y gnupg ca-certificates; \
mkdir /root/.gnupg; \
sudo gpg --no-default-keyring --keyring gnupg-ring:/etc/apt/trusted.gpg.d/mono.gpg --keyserver keyserver.ubuntu.com --recv 3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF &&
sudo chmod 644 /etc/apt/trusted.gpg.d/mono.gpg &&
echo "deb https://download.mono-project.com/repo/ubuntu stable-focal main" | sudo tee /etc/apt/sources.list.d/mono-official-stable.list &&
sudo apt update &&
echo Certificates installation finished
```

</p></details>

### _Step 4: Configure your environment_

Update environment variables values with:

| Environment variable            |             Value              |
|---------------------------------|:------------------------------:|
| `REPORTING_SERVER_HOSTNAME`     | your Zebrunner `workspace URL` |
| `REPORTING_SERVER_ACCESS_TOKEN` |      `token` from step #1      |
| `REPORTING_PROJECT_KEY`         |       `KEY` from step #2       |

#### _On Windows:_

<details><summary style="font-style: italic; font-weight: bold">CMD</summary><p>

```
set REPORTING_SERVER_HOSTNAME=_server_hostname_
set REPORTING_SERVER_ACCESS_TOKEN=_token_
set REPORTING_ENABLED=true
set REPORTING_PROJECT_KEY=_key_
set REPORTING_RUN_BUILD=0.0.0.1-SNAPSHOT
set REPORTING_RUN_ENVIRONMENT=DEMO_NUnit
```

</p></details>

<details><summary style="font-style: italic; font-weight: bold">PowerShell</summary><p>

```
$env:REPORTING_SERVER_HOSTNAME='_server_hostname_'
$env:REPORTING_SERVER_ACCESS_TOKEN='_token_'
$env:REPORTING_ENABLED='true'
$env:REPORTING_PROJECT_KEY='_key_'
$env:REPORTING_RUN_BUILD='0.0.0.1-SNAPSHOT'
$env:REPORTING_RUN_ENVIRONMENT='DEMO_NUnit'
```

</p></details>

#### _On Linux and MacOs:_

The `zebrunner-env` file holds all the required configuration to enable reporting of your tests on Zebrunner.
Edit this file located inside root directory of cloned repository;

<details><summary style="font-style: italic; font-weight: bold">Bash</summary><p>

```
#!/bin/bash
export REPORTING_SERVER_HOSTNAME=_server_hostname_
# demo token below is auto-generated and will expire in 60 minutes
export REPORTING_SERVER_ACCESS_TOKEN=_token_
export REPORTING_ENABLED=true
export REPORTING_PROJECT_KEY=_key_
export REPORTING_RUN_BUILD=0.0.0.1-SNAPSHOT
export REPORTING_RUN_ENVIRONMENT=DEMO_NUnit

alias nunit3-console="mono packages/nunit.consolerunner/3.16.3/tools/nunit3-console.exe"
PS1="[Zebrunner-env] $ "
```

</p></details>

### _Step 5: Build the project_

- Recover dependencies(packages)
- Build the project

<details><summary style="font-style: italic; font-weight: bold">On Windows:</summary><p>

```
dotnet restore --packages packages
dotnet build
```

</p></details>

<details><summary style="font-style: italic; font-weight: bold">On Linux and MacOS:</summary><p>

```
msbuild -t:restore,build /p:RestorePackagesPath=packages
```

</p></details>

### _Step 6: Initiate source_

#### _On Windows:_

You can skip this step

<details><summary style="font-style: italic; font-weight: bold">On Linux and MacOS:</summary><p>

```
source zebrunner-env
```
</p></details>

### _Step 7: Run sample test_

Run a sample test with Zebrunner reporting:

<details><summary style="font-style: italic; font-weight: bold">On Windows:</summary><p>

```
packages\nunit.consolerunner\3.16.3\tools\nunit3-console.exe bin\Debug\net48\nunit-demo-sample.dll --where name=SimpleTest
```

</p></details>

<details><summary style="font-style: italic; font-weight: bold">On Linux and MacOS:</summary><p>

```
nunit3-console bin/Debug/net48/nunit-demo-sample.dll --where name=SimpleTest
```

</p></details>

---

For more information about framework refer to NUnit [documentation](https://docs.nunit.org/).<br>
For more information about reporting refer to Zebrunner [documentation](https://zebrunner.com/documentation/reporting/nunit/).

---
