
# ALM Client Deployment - App-V

## App-v Deployment

App-v is an application virtualization and application streaming solution.
In order to deploy App-v there is a need for MSDN subscription. Once you have it, install App-v components:
- [Sequencer](https://technet.microsoft.com/en-us/itpro/windows/manage/appv-install-the-sequencer) - converts applications into virtual packages to be deployed on app-v clients.
- [Server](https://technet.microsoft.com/en-us/itpro/windows/manage/appv-deploy-the-appv-server) - it is possible to install all server components on 1 machine, pay attention to prerequisites.
- [Client](https://technet.microsoft.com/en-us/itpro/windows/manage/appv-deploying-the-appv-sequencer-and-client)  - windows 10 and later, client already installed but needs to be enabled.

## ALM Client Deployment

In order to convert alm client into an app-v cirtual package, you will need ClientMSIGeneratorSetup, which usually exist on: `\\mydastr01.hpeswlab.net\products\TestDirector\ALMDev\$ALM_VERSION\Archives\$ALM_BUILD\ClientMSIGenerator`
After ClientMSIGenerator is installed, go to `C:\Program Files (x86)\Hewlett Packard Enterprise\ALM Client MSI Generator` and run `ClientMSIGenerator`.
