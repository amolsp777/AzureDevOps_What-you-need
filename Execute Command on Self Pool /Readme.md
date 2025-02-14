# Execute Command on Self Pool

You need to install few packages on the Self hosted pool if those are missing and you don't have access to SSH oh that machine.
You can push the package installation using this process or `yml` using pipeline.

``` yaml
# Starter pipeline
# Start with a minimal pipeline that you can customize to build and deploy your code.
# Add steps that build, run tests, deploy, and more:
# https://aka.ms/yaml

trigger:
- main

pool:
  #vmImage: ubuntu-latest
  name: mypool # custom pool name 

steps:
- script: echo Hello, world!
  displayName: 'Run a one-line script'

- script: |
    echo Add other tasks to build, test, and deploy your project.
    echo See https://aka.ms/yaml
  displayName: 'Run a multi-line script'
- task: Bash@3
  displayName: 'Install Unzip script'
  inputs:
    targetType: 'inline'
    script: 'sudo apt-get install unzip'
- task: Bash@3
  displayName: 'install zip script'
  inputs:
    targetType: 'inline'
    script: 'sudo apt-get install zip'

- task: CmdLine@2
  displayName: 'Installing PowerShell on Ubuntu'
  inputs:
    script: |
      ###################################
      # Prerequisites
      
      # Update the list of packages
      sudo apt-get update
      
      # Install pre-requisite packages.
      sudo apt-get install -y wget
      
      # Download the PowerShell package file
      wget https://github.com/PowerShell/PowerShell/releases/download/v7.5.0/powershell_7.5.0-1.deb_amd64.deb
      
      ###################################
      # Install the PowerShell package
      sudo dpkg -i powershell_7.5.0-1.deb_amd64.deb
      
      # Resolve missing dependencies and finish the install (if necessary)
      sudo apt-get install -f
      
      # Delete the downloaded package file
      rm powershell_7.5.0-1.deb_amd64.deb

```
