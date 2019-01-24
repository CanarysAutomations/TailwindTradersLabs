# Developing Intelligent Apps with Azure


## Key Takeaway

The key feature for the audiance to consider as key takeaway from this demo -

   - With the flexible Azure platform and a wide portfolio of AI productivity tools, developers can build the next generation of smart applications where their data lives, in the intelligent cloud, on-premises, and on the intelligent edge.

   - Teams can achieve more with the comprehensive set of flexible and trusted AI services - from pre-built APIs, such as Cognitive Services and Conversational AI with Bot tools, to building custom models with Azure Machine Learning for any scenario.

 # Before you begin

1. **Visual Studio 2019 Preview 1** with Xamarin workloads installed

1. **Android Emulator** with Hyper-V Windows Feature installed as per this <a href="https://docs.microsoft.com/en-us/xamarin/android/get-started/installation/android-emulator/hardware-acceleration?tabs=vswin&pivots=windows" target="blank">document</a>

1. **Android SDK 28** installed (Android Pie 9)

1. Active Azure subscription

1. Install <a href="https://azurecliprod.blob.core.windows.net/msi/azure-cli-2.0.45.msi">Azure CLI version 2.0.45</a>

 ## Walkthrough: Configure AIVisualProvision App

  AI Visual Provision mobile app uses Azure Cognitive Services (Computer Vision and Custom Vision) to deploy whiteboard drawings to an Azure architecture. It uses Cognitive Services to detect the Azure services among Azure Service logos and handwriting using the phone camera. The captured image is analysed and the identified services are deployed to Azure taking away all the pain and complexity from the process. You can see how to create Azure Cognitive Services and configure them in this mobile app.

 1. Open your Visual Studio 2019 Preview in Administrator mode and click on **Clone or checkout code**.

    ![](images/LandingPage_VS2019_1.png)
 
 1. Copy and paste the URL: https://github.com/Microsoft/AIVisualProvision in **Code repository location** textbox and click **Clone** button. 

    ![](images/CloningRepo_2.png)
 
 1. While the cloning is still in progress, the code is already available for use. It has brought up the folder view. Before loading the solution in Visual Studio 2019 Preview, navigate to ***AIVisualProvision/Source/VisualProvision.iOS/*** and **delete** the folder ***Assets.xcassets*** as per workaround mentioned <a href="https://developercommunity.visualstudio.com/content/problem/398522/vs-2019-preview-xamarin-load-fails.html" target="blank">here</a> (which is supposed to be fixed in coming update)

    ![](images/FolderView_3.png)

    ![](images/DeleteAssets.xcassets_4.png)
 
 1. Double click on the solution ***VisualProvision.sln*** to load it in Solution Explorer. Now you can see that 4 projects are successfully loaded under the solution.

    ![](images/LoadSolution_5.png)

1. Navigate to **AppSettings.cs** file under **VisualProvision** project. You need to provide the **Client ID** and **tenantId** if you are running the app in Emulator in the Debug mode so that you don't have to enter it all the time you debug. Also, the **CustomVisionPredictionUrl**, **CustomVisionPredictionKey**, **ComputerVisionEndpoint** and **ComputerVisionKey** has to be entered.

    ![](images/AppSettingsFile_6.png)

1. In order to get the **Client ID** and **tenantId**, type **az login** in the command prompt and press Enter. Authorize your login in the browser.

   Type **az ad sp create-for-rbac -n “MySampleApp” -p P2SSWORD** in the command prompt to get the Service Principal Client ID and the Service Principal Client Secret.

   Copy and note down the **appId** which is the **Client ID**
   
   Copy and note down the **tenantId**

   **P2SSWORD** is the Client Secret or **Password**, which will be required to run the app.

   ![](images/SPNDetails_7.png)

1. Switch to Visual Studio 2019 and paste the **Client ID** and **TenantId** in the **AppSettings.cs** file.

   ![](images/PasteSPNDetails_8.png)

1. You need a Computer Vision service in order to use the handwriting recognition features of the app. To create your own Computer Vision instance navigate to this <a href="https://azure.microsoft.com/en-us/try/cognitive-services/" target="blank">page</a>. Click **Get API Key** button under Computer Vision.

   ![](images/ComputerVision1_9.png)

   You can refer this <a href="https://docs.microsoft.com/en-us/azure/cognitive-services/computer-vision/vision-api-how-to-topics/howtosubscribe" target="blank">link</a> more details on Computer Vision service.

1. Click **Sign In** and login to your Azure account.

   ![](images/ComputerVision2_10.png)

1. In the Azure portal, you will be prompted to create a new **Computer Vision** service. Provide **Name**, **Subscription**, **Location**, **Pricing tier** and **Resource group**. Click **Create**.

   ![](images/CreateComputerVision_11.png) 

1. Once you have provisioned the service, note down the service endpoint **URL** and endpoint **Key** as per the images below:

   ![](images/ComputerVision3_12.png)

   ![](images/ComputerVision4_13.png)

1. Switch to Visual Studio 2019 and paste the copied **URL** under **ComputerVisionEndpoint** and **Key** under **ComputerVisionKey** in the **AppSettings.cs** file.

   ![](images/PasteComputerVision_14.png)

1. If you wish to use only the handwriting recognition service in your app, skip to next section to Build and Run the app. But, if you wish to add in-app logo recognition to your app, continue with next step.

1. The in-app logo recognition is accomplished by using Azure Custom Vision. In order to use the service in the app, you need to create a new Custom Vision project and train it with the images provided in the repo under the **documents/training_dataset** folder. Go to <a href="https://azure.microsoft.com/en-us/services/cognitive-services/custom-vision-service/" target="blank">Azure Custom Vision</a> page and click **Get started**, and **Sign In** using your Azure credentials.

   ![](images/CustomVision1_15.png)

   ![](images/CustomVision2_16.png)

1. Agree to terms by clicking **Yes** and **I agree** respectively.

   ![](images/CustomVision3_17.png)

   ![](images/CustomVision4_18.png)

1. You will be taken to the landing page of your Azure Custom Vision account. Click **New Project** and provide a **Name**. Leave rest options as it is and click **Create project**.

   ![](images/CustomVision5_19.png)

1. Once you go inside your project, click on **Add images**. Upload a image of **Azure App Service** logo and name it as APP_SERVICE. If you don't have the magnets or images handy, refer to included 2 PDFs (<a href="https://github.com/Microsoft/AIVisualProvision/blob/master/Documents/AzureMagnets1.pdf" target="blank">sheet1</a>, <a href="https://github.com/Microsoft/AIVisualProvision/blob/master/Documents/AzureMagnets2.pdf" target="blank">sheet2</a>) with all the magnet logos. Make sure you cut only the App Service image from the pdf. Select **Done** once the images have been uploaded.

   ![](images/CustomVision6_20.png)

   ![](images/CustomVision7_21.png)

   ![](images/CustomVision8_22.png)

1. Inorder to **Train** you need to have at least 2 tags and 5 images for every tag.  Return to the previous step of this section and repeat the step to add atleast 5 images for **APP_SERVICE** and **SQL_DATABASE** tags. 

   ![](images/CustomVision9_23.png)

   When training the model, you should use following set of tags, as they are the expected tags in the application. Tags are located at **VisualProvision\Services\Recognition\RecognitionService.cs** file.

   ![](images/CustomVision10_24.png)

1. To train the classifier, select the **Train** button. The classifier uses all of the current images to create a model that identifies the visual qualities of each tag.

   ![](images/CustomVision11_25.png)

1. Optionally, to verify the accuracy click on **Quick Test** and provide any similar **Image URL** of either App Service or SQL Database. 

   ![](images/CustomVision12_26.png)

1. Under **Performance** tab, click on **Prediction URL** and note down the service endpoint **URL** and endpoint **Key** as per the images below:   

   ![](images/CustomVision13_27.png)

1. Switch to Visual Studio 2019 and paste the copied **URL** under **CustomVisionPredictionUrl** appended by **/image**, and **Key** under **CustomVisionPredictionKey** in the **AppSettings.cs** file. Click **Save**.

   ![](images/CustomVision14_28.png)

 
## Walkthrough: Build & Run AIVisualProvision App

1. Right click on the solution and select **Rebuild Solution**.

   ![](images/BuildSolution.png)

 1. On a whiteboard, design a simple web app architecture consisting of below resources:
    - App service to host the app
    - SQL database for the data
    - Key Vault to store certificates and sensitive data

    ![](images/Whiteboard.png)

1. Go to **Tools > Android > Android Device Manager**. Click **Start**. Android Emulator will show up.

   ![](images/Run_StartEmulator.png)

1. Click **Run** to deploy your app to Android Emulator (launches in Debug mode). Wait for a while until AI Visual Provision app shows up on the emulator.

   ![](images/Run_Emulator.png)

2. Click **Allow** in order to access camera and photos.

   ![](images/Run_AllowCameraAccess1.png)

   ![](images/Run_AllowCameraAccess2.png)

   Note: If you get the error: "Guest isn't online after 7 seconds, retrying..", open the **Xamarin Android SDK Manager** in Visual Studio by going to Tools > Android > SDK Manager. Update the Android SDK Tools and Android Emulator to the latest Version.

1. Enter the password as **P2ssword** which was noted down earlier. Client ID and Tenant ID will be auto populated. Click **Login**.

   ![](images/Run_EnterPassword.png)

   ![](images/Run_Login.png)

1. Select your **Azure Subscription** and click **Continue**.

   ![](images/Run_SelectSubscription.png)

   ![](images/Run_SelectSubscription2.png)

1. Take a picture of whiteboard which has your architecture diagram consisting of Azure Service logos of App Service and SQL Database. Where as Key Valut is mentioned in handwriting. 

   ![](images/Run_Whiteboard.png)

1. You can see that Azure resources haveen been identified by the app. Click **Next**. 

   ![](images/Run_IdentifyResources.png)

1. Select **Region** and **Resource group**. Click **Deploy**.

   ![](images/Run_Deploy.png)

1. You can see the progress of your deployment right from your emulator or mobile device.

   ![](images/Run_DeployInProgress.png)

   ![](images/Run_DeployInProgress2.png)

   ![](images/Run_SuccessfulDeployment.png)

1. Switch to your Azure portal and navigate to the newly created resource group to see the Azure resources created through your emulator or mobile device.

   ![](images/Run_PostDeploymentRG.png)

## Summary

Today you have more power at your fingertips than entire generations that came before you. Powerful mobile applications can be built using the power of Azure, AI and .NET