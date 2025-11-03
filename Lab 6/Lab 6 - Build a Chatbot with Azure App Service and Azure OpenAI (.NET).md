### Lab 6 - Build a Q&A chatbot with Azure App Service and Azure OpenAI chat completion model

### Time Duration: 50 mins

In this tutorial, you'll build an intelligent AI application by
integrating Azure OpenAI with a Java Spring Boot application and
deploying it to Azure App Service. You'll create a Razor page that sends
chat completion requests to a model in Azure OpenAI and streams the
response back to the page.

![Screenshot showing chatbot running in Azure App
Service.](./media/image1.png)

In this tutorial, you learn how to:

- Create an Azure OpenAI resource and deploy a language model

- Build a Blazor application with Azure OpenAI

- Deploy the application to Azure App Service

- Implement passwordless authentication both in the development
  environment and in Azure

## Prerequisites

- A GitHub account  for using GitHub Codespaces. If you don’t have
  Github account then you can create from
  [here](https://github.com/signup?ref_cta=Sign+up&ref_loc=header+logged+out&ref_page=%2F&source=header-home).

# Task 1: Register Resource provider

In this task, we will register all the resource providers used in this
lab.

1.  Open a web browser of your choice and navigate to the Azure portal
    +++https://portal.azure.com+++.

2.  Sign-in using the given cloud slice credentials:

    - Username - +++<@lab.CloudPortalCredential(User1).Username>+++

    - Password - +++<@lab.CloudPortalCredential(LabUser).AccessToken>+++

    ![](./media/image2.png)
    ![](./media/image3.png)
    ![](./media/image4.png)

3.  Click on the **Subscriptions** tile.

    ![](./media/image5.png)

4.  Click on the **subscription name**.

    ![](./media/image6.png)

5.  Expand Settings from the left navigation menu. Click on **Resource
    providers**, enter +++**Microsoft.Web**+++ and select three dots,
    and then click **Register**.

    ![](./media/image7.png)

    ![](./media/image8.png)

6.  Similarly register the following:

    - +++**Microsoft.CognitiveServices**+++

    - +++**Microsoft.ServiceLinker**+++

# Task 2: Create an Azure OpenAI resource

In this task, you'll use GitHub Codespaces to create an Azure OpenAI
resource with the Azure CLI.

1.  Go to **GitHub Codespaces** +++https://github.com/codespaces+++ and
    sign in with your GitHub account.

    ![](./media/image9.png)

2.  Find the **Blank** template on GitHub and select **Use this
    template** to create a new blank Codespace.

    ![](./media/image10.png)

3.  Wait for the Codespaces environment to setup. It takes few minutes
    to setup completely.

    ![](./media/image11.png)

4.  In the Codespace terminal, run the following command to install the
    Azure CLI:

    +++curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash+++

    ![](./media/image12.png)

5.  Run the following command to sign in to your Azure account. Copy the
    generated code, then open the provided URL and paste the code when
    prompted.

     +++az login+++

    ![](./media/image13.png)

6.  Enter the given authentication code and click on the **Next**
    button.  
    
    ![](./media/image14.png)

    **Note:** The authentication code is always different each time we log
    in.

7.  Select your Azure account.  
    
    ![](./media/image15.png)

8.  Click on the **Continue** button, and your Azure account is logged
    in successfully.

    ![](./media/image16.png)

    ![](./media/image17.png)

9.  Navigate back to the Codespace terminal and press +++**1**+++ to
    select your subscription and tenant.  
    
    ![](./media/image18.png)

    ![](./media/image19.png)

10. Set environment variables for your **resource group name**, **Azure
    OpenAI service name**, and **location**:

    +++export RESOURCE_GROUP="ResourceGroup1"+++

    +++export
    OPENAI_SERVICE_NAME="azure-openai-service-@lab.labinstanceid()"+++

    +++export APPSERVICE_NAME="nueralNest-xy@lab.labinstanceid()"+++

    +++export LOCATION="eastus2"+++  

    ![](./media/image20.png)

11. Create an Azure OpenAI resource with a custom domain, then add a
    gpt-4o-mini model in the existing resource group:

    ``` 
    # Azure OpenAI resource
    az cognitiveservices account create --name $OPENAI_SERVICE_NAME --resource-group $RESOURCE_GROUP --location $LOCATION --custom-domain $OPENAI_SERVICE_NAME --kind OpenAI --sku s0
    # gpt-4o-mini model
    az cognitiveservices account deployment create --name $OPENAI_SERVICE_NAME --resource-group $RESOURCE_GROUP --deployment-name gpt-4o-mini --model-name gpt-4o-mini --model-version 2024-07-18 --model-format OpenAI --sku-name Standard --sku-capacity 1
    # Cognitive Services OpenAI User role that lets the signed in Azure user to read models from Azure OpenAI
    az role assignment create --assignee $(az ad signed-in-user show --query id -o tsv) --role "Cognitive Services OpenAI User" --scope /subscriptions/$(az account show --query id -o tsv)/resourceGroups/$RESOURCE_GROUP/providers/Microsoft.CognitiveServices/accounts/$OPENAI_SERVICE_NAME

    ```

    ![](./media/image21.png)

Now that you have an Azure OpenAI resource, you'll create a web
application to interact with it.

# Task 3: Create and set up a Blazor web app

In this task, you'll create a new Blazor web application using the .NET
CLI.

1.  In your Codespace terminal, create a new Blazor app and try running
    it for the first time.

    ```
    dotnet new blazor -o .
    dotnet run
    ```

    These commands will add the project directories on the left-hand side.
    ![](./media/image22.png)

2.  While running the above commands, you will get a notification in
    GitHub Codespaces indicating that the app is available at a specific
    port. Select **Open in browser** to launch the app in a new browser
    tab.  
    
    ![](./media/image23.png)
    
    ![A screenshot of a computer AI-generated
    content may be incorrect.](./media/image24.png)

3.  Go back in the Codespace terminal, stop the app with **Ctrl+C**.

4.  Install the required **NuGet packages** for working with Azure
    OpenAI:

    ```
    dotnet add package Azure.AI.OpenAI
    dotnet add package Azure.Identity

    ```

    ![](./media/image25.png)

5.  Open **Components/Pages/Home.razor** and replace its content with
    the following code, for a simple chat completion stream call with
    Azure OpenAI:

    ```
    @page "/"
    @rendermode InteractiveServer
    @using Azure.AI.OpenAI
    @using Azure.Identity
    @using OpenAI.Chat
    @inject Microsoft.Extensions.Configuration.IConfiguration _config

    <h3>Azure OpenAI Chat</h3>
    <div class="mb-3 d-flex align-items-center" style="margin:auto;">
        <input class="form-control me-2" @bind="userMessage" placeholder="Type your message..." />
        <button class="btn btn-primary" @onclick="SendMessage">Send</button>
    </div>
    <div class="card p-3" style="margin:auto;">
        @if (!string.IsNullOrEmpty(aiResponse))
        {
            <div class="alert alert-info mt-3 mb-0">@aiResponse</div>
        }
    </div>

    @code {
        private string? userMessage;
        private string? aiResponse;

        private async Task SendMessage()
        {
            if (string.IsNullOrWhiteSpace(userMessage)) return;

            // Initialize the Azure OpenAI client
            var endpoint = new Uri(_config["AZURE_OPENAI_ENDPOINT"]!);
            var client = new AzureOpenAIClient(endpoint, new DefaultAzureCredential());
            var chatClient = client.GetChatClient("gpt-4o-mini");

            aiResponse = string.Empty;
            StateHasChanged();

            // Create a chat completion streaming request
            var chatUpdates = chatClient.CompleteChatStreamingAsync(
                [
                    new UserChatMessage(userMessage)
                ]);

                await foreach(var chatUpdate in chatUpdates)
                {
                    // Update the UI with the streaming response
                    foreach(var contentPart in chatUpdate.ContentUpdate)
                {
                    aiResponse += contentPart.Text;
                    StateHasChanged();
                }
            }
        }
    }
    ```

    ![](./media/image26.png)

6.  In the terminal, run the following commands to retrieve your
    **OpenAI endpoint** and save it in a notepad for later use:

    ```
    az cognitiveservices account show \
    --name $OPENAI_SERVICE_NAME \
    --resource-group $RESOURCE_GROUP \
    --query properties.endpoint \
    --output tsv
    ```

    ![](./media/image27.png)

7.  Run the app again by adding **AZURE_OPENAI_ENDPOINT **with the value
    saved in the previous step.

    +++AZURE_OPENAI_ENDPOINT=\<output-from-previous-cli-command\> dotnet run+++

    ![](./media/image28.png)

8.  Select **Open in browser** to launch the app in a new browser tab.

    ![](./media/image29.png)

    ![A screenshot of a chat AI-generated content may be
    incorrect.](./media/image30.png)

9.  Type the following prompt in the textbox and select **Send** and
    give the app a few seconds to reply with a message from Azure
    OpenAI.

    +++What is Azure?+++

    ![](./media/image31.png)

The application uses DefaultAzureCredential, which automatically uses
your Azure CLI signed-in user for token authentication. Later in this
lab, you'll deploy your Blazor app to Azure App Service and
configure it to securely connect to your Azure OpenAI resource using
managed identity. The same DefaultAzureCredential in your code can
detect the managed identity and use it for authentication. No extra code
is needed.

# Task 4: Deploy to Azure App Service and configure OpenAI connection

Now that your app works locally, let's deploy it to Azure App Service
and set up a service connection to Azure OpenAI using managed identity.

1.  Navigate back to the terminal and press **Ctrl+C** to terminate the
    running command. Now, deploy your app to Azure App Service using the
    Azure CLI command az webapp up. This command creates a new web app
    and deploys your code to it:

    ```
    az webapp up \
    --resource-group $RESOURCE_GROUP \
    --location $LOCATION \
    --name $APPSERVICE_NAME \
    --plan $APPSERVICE_NAME \
    --sku B1 \
    --os-type Linux \
    --track-status false
    ```

    ![](./media/image32.png)

    This command might take a few minutes to complete. It creates a new
    web app in the same resource group as your OpenAI resource.

    **Note:** You can safely ignore any errors(such as Zip Deployment
    failed) that are not related to App Service creation. If a timeout
    error occurs, simply re-run the command.

2.  After the app is deployed, create a service connection between your
    web app and the Azure OpenAI resource using managed identity:

    ```
    az webapp connection create cognitiveservices \
    --resource-group $RESOURCE_GROUP \
    --name $APPSERVICE_NAME \
    --target-resource-group $RESOURCE_GROUP \
    --account $OPENAI_SERVICE_NAME
    --connection azure-openai \
    --system-identity
    ```
    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image33.png)

    This command creates a connection between your web app and the Azure
    OpenAI resource by:

    - Generating a system-assigned managed identity for the web app.

    - Adding the Cognitive Services OpenAI Contributor role to the managed
    identity for the Azure OpenAI resource.

    - Adding the AZURE_OPENAI_ENDPOINT app setting to your web app.

    Your app is now deployed and connected to Azure OpenAI with a managed
    identity.

3.  Navigate to the **Azure portal** and sign in to your account.

    +++https://portal.azure.com/+++

4.  Navigate to **Resource Groups**, open your resource group that is
    **ResourceGroup1**, and select your app that is
    **nueralNest-xy@lab.labinstanceid().**

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image34.png)
    ![A screenshot of a computer
    AI-generated content may be incorrect.](./media/image35.png)

5.  On the Overview page of your app, click the **URL** to open the
    deployed web app in a new browser window.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image36.png)

    ![A screenshot of a chat AI-generated content may be
    incorrect.](./media/image37.png)

6.  Type the following prompt in the textbox and select **Send**, and
    give the app a few seconds to reply with a message from Azure
    OpenAI.

    +++What is RAG in Azure?+++

    ![](./media/image38.png)

# Summary:

In this lab, you provision Azure resources, including an Azure OpenAI
instance with the gpt-4o-mini model. You build a Blazor web app that
integrates with Azure OpenAI for Q&A chatbot functionality. The app is
tested locally in GitHub Codespaces using DefaultAzureCredential, then
deployed to Azure App Service with managed identity for secure access.
Finally, the chatbot is validated through its public URL, streaming
real-time AI responses.
