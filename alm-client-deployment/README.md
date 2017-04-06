
# ALM Client Deployment - App-V

## App-V Deployment

App-V is an application virtualization and application streaming solution.
In order to deploy App-v there is a need for MSDN subscription. Once you have it, install App-v components:
- [Sequencer](https://technet.microsoft.com/en-us/itpro/windows/manage/appv-install-the-sequencer) - converts applications into virtual packages to be deployed on app-v clients.
- [Server](https://technet.microsoft.com/en-us/itpro/windows/manage/appv-deploy-the-appv-server) - it is possible to install all server components on 1 machine, pay attention to prerequisites.
- [Client](https://technet.microsoft.com/en-us/itpro/windows/manage/appv-deploying-the-appv-sequencer-and-client)  - windows 10 and later, client already installed but needs to be enabled.

## ALM Client Deployment

In order to convert alm client into an app-v virtual package, you will need ClientMSIGeneratorSetup, which usually exist on: `\\mydastr01.hpeswlab.net\products\TestDirector\ALMDev\$ALM_VERSION\Archives\$ALM_BUILD\ClientMSIGenerator`
After ClientMSIGenerator is installed, go to `C:\Program Files (x86)\Hewlett Packard Enterprise\ALM Client MSI Generator` and run `ClientMSIGenerator` as follows:

![install](https://github.com/HPSoftware/alm-vayu-docs/blob/master/alm-client-deployment/alm-client-generator.JPG)

On the sequencer machine:
- Run Microsoft Application Virtualization (App-V) Sequencer Tool
  - Create standalone application
  - Select alm client installation from `C:\Program Files (x86)\Hewlett Packard Enterprise\ALM Client MSI Generator\output`
  - On the actual installation phase, you will see alm client installation, follow the installation steps as usual.

- Put the result `alm-client.appv` file on a shared location.

On the client machine:
- Open power shell:
  - Run `Add-AppvClientPackage $ALM_CLIENT.appv` - replace $ALM_CLIENT.appv with the .appv file location, should be shared location or http address.
  - Run `Publish-AppvClientPackage alm-client`
- If successfull - virtual package should be located on `C:\ProgramData\App-V`, in there you will see folders with GUID names, e.g.: `C:\ProgramData\App-V\B1668A0F-7F1A-4909-9EB5-4C5DD5CAB9CF\871F7B65-7771-4A90-AF19-CAD2A39CB125\Root\VFS\Common AppData\HP\ALM-Client\12.55.0.0_952`
- In order to run alm-client, go to alm-client.exe folder and open command line, run: `ALM-Client.exe TDtesttypes=. AdditionalParams="Brand=ALM&BrandDisplayName=Application Lifecycle Management" ApplicationType="Mercury.TD.Client.UI.Core.Application,QCClient.UI.Core" ConfigurationFile="ALM-Client.exe" PrivatePath="3rdParty" URL=$ALM_SERVER`, replace $ALM_SERVER with your server.
