# Deploying CrowdStrike Falcon Sensor to Amazon WorkSpaces

## Prerequisites

- Have an AWS account to create or administer an Amazon WorkSpace. Users do not need an AWS account or AWS console access to connect to and use their Amazon WorkSpaces.
- Decide the billing model - choose between yearly sensor licensing model or AWS metered billing. You are required create a separate image for each of the licensing models.
- Require a WorkSpaces compliant Directory Service for user management and logins.
- On launching the WorkSpace, you must specify profile information for the user, including a username and email address, or you can access the directory service and
  create your usernames to be linked in the Workspace build. Users complete their profiles by specifying a password that will accessible from a system generated email.
  Information about WorkSpaces and users is stored in a directory.
- Verify the supported regions, and select a region for your WorkSpaces. Amazon WorkSpaces is available in most AWS Regions. For more information about the
  supported regions, see [Amazon’s Region Table](https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/). 
- It is required to have a Fully Qualified Domain Name (FQDN) for the deployment of WorkSpaces.
- The WorkSpace must be a domain member to the directory service.

> [!NOTE]
>  - Login to the CrowdStrike Falcon Console or CrowdStrike Support portal for product related questions and help.
>  - Email aws@crowdstrike.com for any questions regarding CrowdStrike integrations with AWS.

## Create Amazon WorkSpace Custom Image

Customers can take their current WorkSpaces golden image and create a new image with the
CrowdStrike Falcon sensor installed on the image. This will allow you to use the golden image
to create end user WorkSpaces with the CrowdStrike sensor installed.

1. Select Region to Host the WorkSpace

   To begin, select an AWS region that supports WorkSpaces. Please refer to the list of regions
   that support WorkSpaces on [Amazon’s Region Table](https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/). 
   For this example, we will be using `us-east-1 (N. Virginia)` region.
   
   Open the Amazon WorkSpaces console at https://console.aws.amazon.com/WorkSpaces/

   ![AWS WorkSpaces](./assets/workspaces.png)

2. Validate Directory Services

   From the WorkSpaces console, select the `Directories` tab to validate your Directory services in
   your AWS region. You should have an active directory services that is registered as pictured
   below:

   ![AWS Directory Services](./assets/directories.png)

   If you do not have a directory service, please see the following links from AWS to setup a
   Directory Services:

   - https://aws.amazon.com/directoryservice/
   - https://aws.amazon.com/quickstart/architecture/active-directory-ds/

3. WorkSpace Golden Image Creation

   If you do not have a current image and this is a new install, you will need to create a WorkSpace
   to connect and create your golden image. If you have a current Golden Image that you would
   like to modify and add the CrowdStrike Falcon Sensor, see [Install the CrowdStrike Sensor](#install-the-crowdstrike-falcon-sensor).

   We will step through the creation of the WorkSpace in the next steps:

   From the WorkSpaces console, click `WorkSpaces` from the navigation pane.

   ![AWS WorkSpaces Console](./assets/workspaces_console.png)

   Click the `Launch WorkSpaces` button to create a new WorkSpace.

4. Select Directory

   In the first screen, select your directory service from the drop down and select `Next`.

   ![Launch WorkSpaces - Select Directory](./assets/launch-select_directory.png)

5. Identify Users

   You will create your users or select existing users in your Active Directory. To create a new
   user, enter the username, first name, last name, and email address and click `Create User`.
   You can create multiple users by clicking the `Create Additional Users` button before clicking
   the `Create Users` button.

   ![Launch WorkSpaces - Identify Users](./assets/launch-select_users.png)

   Once you have created your user account, you should see the user listed as below. Once you
   have added your users, click the `Next Step` button.

   > :warning: IMPORTANT
   >
   > If you are new to WorkSpaces, you will have a WorkSpaces limit to 2 WorkSpaces
   > on your account. Each user will be assigned one WorkSpace. You will need to submit a support
   > ticket to AWS to request a WorkSpace resource increase to your desired number. Please see
   > AWS Support page for creating a ticket and resource increase.

   ![Launch WorkSpaces - Identify Users](./assets/launch-full_users.png)

6. WorkSpace Bundle Selection

   In this step, you will select a bundle with the operating system version and resources desired. Based on the
   purpose of the WorkSpace, select a version of the operating system and software required for your users.
   In this example, we will use the standard Windows 10 bundle to create our image. Once you
   select your bundle, click the `Next Step` button at the bottom of the screen.

   ![Launch WorkSpaces - Bundle Selection](./assets/launch-bundle_selection.png)

7. WorkSpace Configuration

   In this step, you will select the `Running Mode`, `Encryption` for volumes, and `Manage Tags`, if
   required.

   > :warning: IMPORTANT
   >
   > Do not encrypt your volumes as we will be using this to create your golden image!

   Once you make your choices for tags and running mode, click the `Next Step` button.

   ![Launch WorkSpaces - WorkSpace Configuration](./assets/launch-workspace_config.png)

8. Review and WorkSpace Launch

   In this step, review the choice you have made for the creation of the WorkSpace. Once you have
   completed review, click the `Launch WorkSpaces` button at the bottom of the screen.

   ![Launch WorkSpaces - Review](./assets/launch-review.png)

9. WorkSpace Creation

   Your WorkSpaces environment is now creating. You will see your WorkSpace being created, beginning with the `pending` state.

   ![WorkSpace Creation - Pending](./assets/creation-pending.png)
   
   Once the WorkSpace build is complete, you will see the WorkSpace status `Available`. The user
   email address should also receive an email with information on how to set the username
   password and download instructions for the WorkSpace’s client. This will allow you to connect to the WorkSpace.
   
   ![WorkSpace Creation - Client](./assets/creation-client.png)

   Below is an example of the email generated by WorkSpaces creation.

   ![WorkSpace Creation - Email](./assets/creation-email.png)

10. Set WorkSpace Password

    Click the link in the email to set the password for the user account used during the WorkSpace creation.
   
    ![WorkSpace Creation - Login](./assets/creation-login.png)

    Once you have created your password, you will be taken to a website that will allow you to
    download the WorkSpaces Client for your operating system.

    ![WorkSpace Creation - Selection](./assets/creation-selection.png)

    Once you have downloaded and installed the client, you are now ready to connect to your WorkSpace.

11. Connecting to your WorkSpace

    To connect to your WorkSpace, launch the WorkSpaces client software. You will need to copy
    your registration code that is listed in bullet 2 on the email sent from AWS.

    ![WorkSpace Creation - Registration](./assets/creation-selection.png)
   
    In the initial login box for WorkSpaces, paste the registration code into the box and click `Register`.

    ![WorkSpace Creation - Register](./assets/creation-register.png)

    From the next screen, you will enter the username and password that you created for the account.
   
    ![WorkSpace Creation - Login](./assets/creation-workspace_login.png)

    This will connect you to the Amazon WorkSpaces environment. You will now be ready to install
    the Falcon Sensor and build your new Golden Image.

## Install the CrowdStrike Falcon Sensor

1. Install the CrowdStrike Falcon Sensor

   To install the CrowdStrike Falcon sensor, log into your Falcon Console to download the sensor
   installer file. Download the installer file and place the installer file into a folder on the `D:` drive
   of the WorkSpaces instance.

   > :warning: IMPORTANT
   >
   > It is important to note that the entire contents of the `C:\Drive` and the entire
   > `D:\Users\username` are included except for the some of the personal folder directories such as
   > `Contacts`, `Downloads`, `Music`, etc. Since the downloads from the web browser usually are
   > downloaded to the `D:\Users\username\downloads` folder, you will need to create a new folder
   > and move the Falcon sensor installer file to that new folder.
   
   Download the Falcon Sensor installer file
   
   ![CrowdStrike Sensor - Download](./assets/install-sensor_download.png)

   Once the file is downloaded, create a new folder in the `D:\Users` directory

   ![CrowdStrike Sensor - User Directory](./assets/install-dir.png)

   Move the Falcon Sensor installer file to a new `CrowdStrike` folder.

   ![CrowdStrike Sensor - Installer](./assets/install-dir_installer.png)

   Once the installer file is in the CrowdStrike folder, we are ready to install the Falcon Sensor.

2. Decide Pricing Model between Yearly or Metered Billing

   You will need to decide the licensing model you would like to use for your sensor install. There
   are two models that you can choose from - yearly licensing for existing license agreement or AWS
   metered billing that will be billed through AWS Marketplace based on hourly usage.

   Sensor installation and building of the golden image of Amazon WorkSpace will depend on the
   selected licensing model. You have the option for creating two separate images - one for metered
   billing and one for yearly licensing.

   For this document, we will detail both the installations of yearly and AWS Marketplace metered billing.

   ### Option 1: Yearly Licensing model
   
   From the Amazon WorkSpace open up a Windows command prompt and navigate to `D:\Users\Crowdstrike\`

   ![CrowdStrike Sensor - Installer](./assets/install-cmddir.png)

   To install the Falcon sensor with a yearly license, you will need to run the following
   command to make sure that we identify the sensor as a VDI build that will need unique AID
   hardware identifiers in the console:

   ```powershell
   WindowsSensor.exe /install /quiet /norestart CID=<your CID> NoFA=1 NoDC=1 VDI=1 NO_START=1
   ```

   In the above command, you will need to insert your Customer ID (CID) in the command above
   replacing <your CID> with your CID plus checksum. Your CID can be found on the sensor
   download page of the CrowdStrike Console.

   Once you run the command, you will see a window pop-up to allow the Falcon Sensor to make
   changes to the system. CLick `Yes` to allow the Falcon Sensor to complete the installation.

   ![CrowdStrike Sensor - UAC Prompt](./assets/install-uac.png)

   Once the sensor install completes, you can check to validate that the sensor deployed by using
   the `SC` command at the command prompt. You will notice that the agent state
   shows a stopped state. This is expected due the `NO_START=1` flag used in the sensor install
   command. This allows a new unique AID to be generated per WorkSpace that created from the
   image.

   ```powershell
   sc query csagent
   ```

   ![CrowdStrike Sensor - Stopped Service](./assets/install-sc_stopped.png)

   Add Windows updates and software as needed for the WorkSpace image.

   ### Option 2: Metered billing Licensing model:

   From the Amazon WorkSpace, open up a Windows command prompt and navigate to `D:\Users\Crowdstrike\`

   ![CrowdStrike Sensor - Installer](./assets/install-cmddir.png)

   To install the Falcon sensor with metered billing, you will need to run the following
   command to make sure that we identify the sensor as a VDI build that will need unique AID
   hardware identifiers in the console:

   ```powershell
   WindowsSensor.exe /install /quiet /norestart CID=<your CID> NoFA=1 NoDC=1 VDI=1 NO_START=1 BILLINGTYPE=Metered
   ```

   In the above command, you will need to insert your Customer ID (CID) in the command above
   replacing `<your CID>` with your CID with checksum. Your CID can be found on the sensor
   download page of the CrowdStrike Console.

   Once you run the command, you will see a window pop-up to allow the Falcon Sensor to make
   changes to the system. Click `Yes` to allow the Falcon Sensor to complete the installation.

   ![CrowdStrike Sensor - UAC Prompt](./assets/install-uac.png)

   Once the sensor install completes, you can check to validate that the sensor deployed by using
   the `SC` command at the command prompt. You will notice that the agent state
   shows a stopped state. This is expected due the `NO_START=1` flag used in the sensor install
   command. This allows a new unique AID to be generated per WorkSpace that created from the
   image.

   ```powershell
   sc query csagent
   ```

   ![CrowdStrike Sensor - Stopped Service](./assets/install-sc_stopped.png)

   Add Windows updates and software as needed for the WorkSpace image.

3. Create Golden Image

   You are now ready to create your WorkSpace image. Make sure you have logged out of your
   WorkSpace environment from the WorkSpace client. Validate that the WorkSpace status is in
   the `Available` status in the AWS WorkSpaces Console.

   ![WorkSpace - Create Image](./assets/workspace-status.png)

   Make sure the box for the WorkSpace you created for the golden image is selected. Click on
   the `Actions` button and from the drop-down menu, select `Create Image`.

   ![WorkSpace - Create Image Launch](./assets/workspace-create_image_launch.png)

   You will be prompted to name your new image and provide a description of the image. After
   completion, click the `Create Image` button.

   ![WorkSpace - Create Image](./assets/workspace-create_image.png)

   A new image will be created from your WorkSpace. This process can take up to 45 minutes to
   complete. You can check the status of the image build from the WorkSpaces console. You will
   see the status in a `Validating` state.

   ![WorkSpace - Create Image Status](./assets/workspace-image_status.png)

   Once the image reaches the `Available` status, you can now create a new bundle that allows you
   to create a new WorkSpace with the new golden image.

4. Create Bundle
   
   When the `Image` status shows `Available`, select the box next to the image, click the `Actions` dropdown menu, and select `Create Bundle`.

   ![WorkSpace - Create Bundle Launch](./assets/workspace-create_bundle_launch.png)

   The image you created in the last step will be listed as the `Selected Image`. Provide a `Bundle Name` and `Description` of the bundle.

   Next, select the `Bundle Type`. This will allow you to select the `Resources` (virtual CPU and
   Memory) required to run the applications in the WorkSpace. For this build, the standard bundle type was selected.

   Next set the `Root` and `User Volume Sizes` you would like. Once you have entered the
   information select `Create Bundle` at the bottom of the page.

   ![WorkSpace - Create Bundle](./assets/workspace-create_bundle.png)

   Once the bundle is created, if you click on the `Bundles` tab in the WorkSpaces console, you will
   see your new bundle and image pair.

   ![WorkSpace - Bundles](./assets/workspace-bundles.png)

5. Deploy a WorkSpace using the new Bundle with the baked-in Falcon Sensor

   Next, we will deploy a new WorkSpace with the created bundled image. Click `WorkSpaces` from the WorkSpaces console submenu.

   > :warning: IMPORTANT
   >
   > You may need to delete the WorkSpace created in the first step with the temp user
   > `csuser`. You can use the `Migrate WorkSpaces` option, if a snapshot has been created for the
   > WorkSpace. If a snapshot has not been created, you will have to remove the WorkSpace and
   > build a new WorkSpace.

   Select the box next to your WorkSpace. Select the `Modify WorkSpaces` option from the `Actions` drop-down menu.

   ![WorkSpace - Modify WorkSpaces](./assets/workspace-modify.png)

   In the `Migrate WorkSpace` menu, select the `Bundle` you created above (Bundle-FalconSensor
   (PCoIP)) and click the` Migrate WorkSpace` button.

   ![WorkSpace - Migrate WorkSpaces](./assets/workspace-migrate.png)

   Once the migration is complete, your WorkSpace should show a status of `Available`.

   ![WorkSpace - Migrate WorkSpaces Status](./assets/workspace-migrate_status.png)

6. Validate the Sensor is installed and running

   Follow the steps above (Steps 9 – 11) to log into the WorkSpaces environment with the client.
   Once you have logged in, open a command prompt to validate the sensor is installed and
   running in the image. From the command prompt, type the following command:

   ```powershell
   sc query csagent
   ```

   The state should show the Falcon sensor as `Running`.

   This completes the build of your WorkSpace’s environment with the CrowdStrike Falcon sensor deployed.

   As you build new WorkSpaces, pointing the build to the custom bundle and image that was
   built will ensure that the Falcon sensor is running and protecting your WorkSpaces Desktop-as-a-Service (DaaS) environments.

## Additional Resources

Learn how Desktop-as-a-Service (DaaS) solves the challenges you face with Virtual Desktop Infrastructure (VDI), and how to deploy DaaS on AWS in minutes.

- [Desktop-as-a-Service](https://aws.amazon.com/workspaces-family/)

Learn about Directory Services from AWS and how to deploy it in AWS Cloud

- [AWS Directory Service](https://aws.amazon.com/directoryservice/)
- [Active Directory Directory Services on AWS - Quick Start](https://aws.amazon.com/quickstart/architecture/active-directory-ds/)

Visit CrowdStrike products on AWS Marketplace

- [AWS Marketplace CrowdStrike Listings](https://aws.amazon.com/marketplace/search/results?searchTerms=crowdstrike)
