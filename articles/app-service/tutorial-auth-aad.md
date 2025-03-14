---
title: 'Tutorial: Authenticate users E2E' 
description: Learn how to use App Service authentication and authorization to secure your App Service apps end-to-end, including access to remote APIs.
keywords: app service, azure app service, authN, authZ, secure, security, multi-tiered, azure active directory, azure ad
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 09/23/2021
ms.custom: "devx-track-csharp, seodec18, devx-track-azurecli"
zone_pivot_groups: app-service-platform-windows-linux
---

# Tutorial: Authenticate and authorize users end-to-end in Azure App Service

::: zone pivot="platform-windows"  

[Azure App Service](overview.md) provides a highly scalable, self-patching web hosting service. In addition, App Service has built-in support for [user authentication and authorization](overview-authentication-authorization.md). This tutorial shows how to secure your apps with App Service authentication and authorization. It uses a ASP.NET Core app with an Angular.js front end as an example. App Service authentication and authorization support all language runtimes, and you can learn how to apply it to your preferred language by following the tutorial.

::: zone-end

::: zone pivot="platform-linux"

[Azure App Service](overview.md) provides a highly scalable, self-patching web hosting service using the Linux operating system. In addition, App Service has built-in support for [user authentication and authorization](overview-authentication-authorization.md). This tutorial shows how to secure your apps with App Service authentication and authorization. It uses an ASP.NET Core app with an Angular.js front end as an example. App Service authentication and authorization support all language runtimes, and you can learn how to apply it to your preferred language by following the tutorial.

::: zone-end

![Simple authentication and authorization](./media/tutorial-auth-aad/simple-auth.png)

It also shows you how to secure a multi-tiered app, by accessing a secured back-end API on behalf of the authenticated user, both [from server code](#call-api-securely-from-server-code) and [from browser code](#call-api-securely-from-browser-code).

![Advanced authentication and authorization](./media/tutorial-auth-aad/advanced-auth.png)

These are only some of the possible authentication and authorization scenarios in App Service. 

Here's a more comprehensive list of things you learn in the tutorial:

> [!div class="checklist"]
> * Enable built-in authentication and authorization
> * Secure apps against unauthenticated requests
> * Use Azure Active Directory as the identity provider
> * Access a remote app on behalf of the signed-in user
> * Secure service-to-service calls with token authentication
> * Use access tokens from server code
> * Use access tokens from client (browser) code

You can follow the steps in this tutorial on macOS, Linux, Windows.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## Prerequisites

To complete this tutorial:

- <a href="https://git-scm.com/" target="_blank">Install Git</a>
- <a href="https://dotnet.microsoft.com/download/dotnet-core/3.1" target="_blank">Install the latest .NET Core 3.1 SDK</a>
[!INCLUDE [azure-cli-prepare-your-environment-no-header.md](../../includes/azure-cli-prepare-your-environment-no-header.md)]

## Create local .NET Core app

In this step, you set up the local .NET Core project. You use the same project to deploy a back-end API app and a front-end web app.

### Clone and run the sample application

1. Run the following commands to clone the sample repository and run it.

    ```bash
    git clone https://github.com/Azure-Samples/dotnet-core-api
    cd dotnet-core-api
    dotnet run
    ```
    
1. Navigate to `http://localhost:5000` and try adding, editing, and removing todo items. 

    ![ASP.NET Core API running locally](./media/tutorial-auth-aad/local-run.png)

1. To stop ASP.NET Core, press `Ctrl+C` in the terminal.

1. Make sure the default branch is `main`.

    ```bash
    git branch -m main
    ```
    
    > [!TIP]
    > The branch name change isn't required by App Service. However, since many repositories are changing their default branch to `main`, this tutorial also shows you how to deploy a repository from `main`. For more information, see [Change deployment branch](deploy-local-git.md#change-deployment-branch).

## Deploy apps to Azure

In this step, you deploy the project to two App Service apps. One is the front-end app and the other is the back-end app.

### Configure a deployment user

[!INCLUDE [Configure deployment user](../../includes/configure-deployment-user-no-h.md)]

### Create Azure resources

::: zone pivot="platform-windows"  

In the Cloud Shell, run the following commands to create two Windows web apps. Replace _\<front-end-app-name>_ and _\<back-end-app-name>_ with two globally unique app names (valid characters are `a-z`, `0-9`, and `-`). For more information on each command, see [Host a RESTful API with CORS in Azure App Service](app-service-web-tutorial-rest-api.md).

```azurecli-interactive
az group create --name myAuthResourceGroup --location "West Europe"
az appservice plan create --name myAuthAppServicePlan --resource-group myAuthResourceGroup --sku FREE
az webapp create --resource-group myAuthResourceGroup --plan myAuthAppServicePlan --name <front-end-app-name> --deployment-local-git --query deploymentLocalGitUrl
az webapp create --resource-group myAuthResourceGroup --plan myAuthAppServicePlan --name <back-end-app-name> --deployment-local-git --query deploymentLocalGitUrl
```

::: zone-end

::: zone pivot="platform-linux"

In the Cloud Shell, run the following commands to create two web apps. Replace _\<front-end-app-name>_ and _\<back-end-app-name>_ with two globally unique app names (valid characters are `a-z`, `0-9`, and `-`). For more information on each command, see [Create a .NET Core app in Azure App Service](quickstart-dotnetcore.md).

```azurecli-interactive
az group create --name myAuthResourceGroup --location "West Europe"
az appservice plan create --name myAuthAppServicePlan --resource-group myAuthResourceGroup --sku FREE --is-linux
az webapp create --resource-group myAuthResourceGroup --plan myAuthAppServicePlan --name <front-end-app-name> --runtime "DOTNETCORE|3.1" --deployment-local-git --query deploymentLocalGitUrl
az webapp create --resource-group myAuthResourceGroup --plan myAuthAppServicePlan --name <back-end-app-name> --runtime "DOTNETCORE|3.1" --deployment-local-git --query deploymentLocalGitUrl
```

::: zone-end

> [!NOTE]
> Save the URLs of the Git remotes for your front-end app and back-end app, which are shown in the output from `az webapp create`.
>

### Push to Azure from Git

1. Since you're deploying the `main` branch, you need to set the default deployment branch for your two App Service apps to `main` (see [Change deployment branch](deploy-local-git.md#change-deployment-branch)). In the Cloud Shell, set the `DEPLOYMENT_BRANCH` app setting with the [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings#az_webapp_config_appsettings_set) command. 

    ```azurecli-interactive
    az webapp config appsettings set --name <front-end-app-name> --resource-group myAuthResourceGroup --settings DEPLOYMENT_BRANCH=main
    az webapp config appsettings set --name <back-end-app-name> --resource-group myAuthResourceGroup --settings DEPLOYMENT_BRANCH=main
    ```

1. Back in the _local terminal window_, run the following Git commands to deploy to the back-end app. Replace _\<deploymentLocalGitUrl-of-back-end-app>_ with the URL of the Git remote that you saved from [Create Azure resources](#create-azure-resources). When prompted for credentials by Git Credential Manager, make sure that you enter [your deployment credentials](deploy-configure-credentials.md), not the credentials you use to sign in to the Azure portal.

    ```bash
    git remote add backend <deploymentLocalGitUrl-of-back-end-app>
    git push backend main
    ```

1. In the local terminal window, run the following Git commands to deploy the same code to the front-end app. Replace _\<deploymentLocalGitUrl-of-front-end-app>_ with the URL of the Git remote that you saved from [Create Azure resources](#create-azure-resources).

    ```bash
    git remote add frontend <deploymentLocalGitUrl-of-front-end-app>
    git push frontend main
    ```

### Browse to the apps

Navigate to the following URLs in a browser and see the two apps working.

```
http://<back-end-app-name>.azurewebsites.net
http://<front-end-app-name>.azurewebsites.net
```

:::image type="content" source="./media/tutorial-auth-aad/azure-run.png" alt-text="Screenshot of an Azure App Service Rest API Sample in a browser window, which shows a To do list app.":::

> [!NOTE]
> If your app restarts, you may have noticed that new data has been erased. This behavior by design because the sample ASP.NET Core app uses an in-memory database.
>
>

## Call back-end API from front end

In this step, you point the front-end app's server code to access the back-end API. Later, you enable authenticated access from the front end to the back end.

### Modify front-end code

1. In the local repository, open _Controllers/TodoController.cs_. At the beginning of the `TodoController` class, add the following lines and replace _\<back-end-app-name>_ with the name of your back-end app:

    ```cs
    private static readonly HttpClient _client = new HttpClient();
    private static readonly string _remoteUrl = "https://<back-end-app-name>.azurewebsites.net";
    ```

1. Find the method that's decorated with `[HttpGet]` and replace the code inside the curly braces with:

    ```cs
    var data = await _client.GetStringAsync($"{_remoteUrl}/api/Todo");
    return JsonConvert.DeserializeObject<List<TodoItem>>(data);
    ```

    The first line makes a `GET /api/Todo` call to the back-end API app.

1. Next, find the method that's decorated with `[HttpGet("{id}")]` and replace the code inside the curly braces with:

    ```cs
    var data = await _client.GetStringAsync($"{_remoteUrl}/api/Todo/{id}");
    return Content(data, "application/json");
    ```

    The first line makes a `GET /api/Todo/{id}` call to the back-end API app.

1. Next, find the method that's decorated with `[HttpPost]` and replace the code inside the curly braces with:

    ```cs
    var response = await _client.PostAsJsonAsync($"{_remoteUrl}/api/Todo", todoItem);
    var data = await response.Content.ReadAsStringAsync();
    return Content(data, "application/json");
    ```

    The first line makes a `POST /api/Todo` call to the back-end API app.

1. Next, find the method that's decorated with `[HttpPut("{id}")]` and replace the code inside the curly braces with:

    ```cs
    var res = await _client.PutAsJsonAsync($"{_remoteUrl}/api/Todo/{id}", todoItem);
    return new NoContentResult();
    ```

    The first line makes a `PUT /api/Todo/{id}` call to the back-end API app.

1. Next, find the method that's decorated with `[HttpDelete("{id}")]` and replace the code inside the curly braces with:

    ```cs
    var res = await _client.DeleteAsync($"{_remoteUrl}/api/Todo/{id}");
    return new NoContentResult();
    ```

    The first line makes a `DELETE /api/Todo/{id}` call to the back-end API app.

1. Save all your changes. In the local terminal window, deploy your changes to the front-end app with the following Git commands:

    ```bash
    git add .
    git commit -m "call back-end API"
    git push frontend main
    ```

### Check your changes

1. Navigate to `http://<front-end-app-name>.azurewebsites.net` and add a few items, such as `from front end 1` and `from front end 2`.

1. Navigate to `http://<back-end-app-name>.azurewebsites.net` to see the items added from the front-end app. Also, add a few items, such as `from back end 1` and `from back end 2`, then refresh the front-end app to see if it reflects the changes.

    :::image type="content" source="./media/tutorial-auth-aad/remote-api-call-run.png" alt-text="Screenshot of an Azure App Service Rest API Sample in a browser window, which shows a To do list app with items added from the front-end app.":::

## Configure auth

In this step, you enable authentication and authorization for the two apps. You also configure the front-end app to generate an access token that you can use to make authenticated calls to the back-end app.

You use Azure Active Directory as the identity provider. For more information, see [Configure Azure Active Directory authentication for your App Services application](configure-authentication-provider-aad.md).

### Enable authentication and authorization for back-end app

1. In the [Azure portal](https://portal.azure.com) menu, select **Resource groups** or search for and select *Resource groups* from any page.

1. In **Resource groups**, find and select your resource group. In **Overview**, select your back-end app's management page.

    :::image type="content" source="./media/tutorial-auth-aad/portal-navigate-back-end.png" alt-text="Screenshot of the Resource groups window, showing the Overview for an example resource group and a back-end app's management page selected.":::

1. In your back-end app's left menu, select **Authentication**, and then click **Add identity provider**.

1. In the **Add an identity provider** page, select **Microsoft** as the **Identity provider** to sign in Microsoft and Azure AD identities.

1. Accept the default settings and click **Add**.

    :::image type="content" source="./media/tutorial-auth-aad/configure-auth-back-end.png" alt-text="Screenshot of the back-end app's left menu showing Authentication/Authorization selected and settings selected in the right menu.":::

1. The **Authentication** page opens. Copy the **Client ID** of the Azure AD application to a notepad. You need this value later.

    :::image type="content" source="./media/tutorial-auth-aad/get-application-id-back-end.png" alt-text="Screenshot of the Azure Active Directory Settings window showing the Azure AD App, and the Azure AD Applications window showing the Client ID to copy.":::

If you stop here, you have a self-contained app that's already secured by the App Service authentication and authorization. The remaining sections show you how to secure a multi-app solution by "flowing" the authenticated user from the front end to the back end. 

### Enable authentication and authorization for front-end app

Follow the same steps for the front-end app, but skip the last step. You don't need the client ID for the front-end app. However, stay on the **Authentication** page for the front-end app because you'll use it in the next step.

If you like, navigate to `http://<front-end-app-name>.azurewebsites.net`. It should now direct you to a secured sign-in page. After you sign in, *you still can't access the data from the back-end app*, because the back-end app now requires Azure Active Directory sign-in from the front-end app. You need to do three things:

- Grant the front end access to the back end
- Configure App Service to return a usable token
- Use the token in your code

> [!TIP]
> If you run into errors and reconfigure your app's authentication/authorization settings, the tokens in the token store may not be regenerated from the new settings. To make sure your tokens are regenerated, you need to sign out and sign back in to your app. An easy way to do it is to use your browser in private mode, and close and reopen the browser in private mode after changing the settings in your apps.

### Grant front-end app access to back end

Now that you've enabled authentication and authorization to both of your apps, each of them is backed by an AD application. In this step, you give the front-end app permissions to access the back end on the user's behalf. (Technically, you give the front end's _AD application_ the permissions to access the back end's _AD application_ on the user's behalf.)

1. In the **Authentication** page for the front-end app, select your front-end app name under **Identity provider**. This app registration was automatically generated for you. Select **API permissions** in the left menu.

1. Select **Add a permission**, then select **My APIs** > **\<back-end-app-name>**.

1. In the **Request API permissions** page for the back-end app, select **Delegated permissions** and **user_impersonation**, then select **Add permissions**.

    :::image type="content" source="./media/tutorial-auth-aad/select-permission-front-end.png" alt-text="Screenshot of the Request API permissions page showing Delegated permissions, user_impersonation, and the Add permission button selected.":::

### Configure App Service to return a usable access token

The front-end app now has the required permissions to access the back-end app as the signed-in user. In this step, you configure App Service authentication and authorization to give you a usable access token for accessing the back end. For this step, you need the back end's client ID, which you copied from [Enable authentication and authorization for back-end app](#enable-authentication-and-authorization-for-back-end-app).

In the Cloud Shell, run the following command on the front-end app to add the `scope` parameter to the authentication setting `identityProviders.azureActiveDirectory.login.loginParameters`. Replace *\<front-end-app-name>* and *\<back-end-client-id>*.

```azurecli-interactive
az webapp auth set --resource-group myAuthResourceGroup --name <front-end-app-name> --body '{"identityProviders":{"azureActiveDirectory":{"login":{"loginParameters":["scope=openid profile email offline_access api://<back-end-client-id>/user_impersonation"]}}}}'
```

Here's an explanation of the requested scopes:

- `openid`, `profile`, and `email` are requested by App Service by default already. For information, see [OpenID Connect Scopes](../active-directory/develop/v2-permissions-and-consent.md#openid-connect-scopes).
- `api://<back-end-client-id>/user_impersonation` is an exposed API in your back-end app registration. It's the scope that gives you a JWT token that includes the back end app as a [token audience](https://wikipedia.org/wiki/JSON_Web_Token). 
- [offline_access](../active-directory/develop/v2-permissions-and-consent.md#offline_access) is included here for convenience (in case you want to [refresh tokens](#when-access-tokens-expire)).

> [!TIP]
> - To view the `api://<back-end-client-id>/user_impersonation` scope in the Azure portal, go to the **Authentication** page for the back-end app, click the link under **Identity provider**, then click **Expose an API** in the left menu.
> - To configure the required scopes using a web interface instead, see the Microsoft steps at [Refresh auth tokens](configure-authentication-oauth-tokens.md#refresh-auth-tokens).
> - Some scopes require admin or user consent. This requirement causes the consent request page to be displayed when a user signs into the front-end app in the browser. To avoid this consent page, add the front end's app registration as an authorized client application in the **Expose an API** page by clicking **Add a client application** and supplying the client ID of the front end's app registration.

::: zone pivot="platform-linux"

> [!NOTE]
> For Linux apps, There's a temporary requirement to configure a versioning setting for the back-end app registration. In the Cloud Shell, configure it with the following commands. Be sure to replace *\<back-end-client-id>* with your back end's client ID.
>
> ```azurecli-interactive
> id=$(az ad app show --id <back-end-client-id> --query objectId --output tsv)
> az rest --method PATCH --url https://graph.microsoft.com/v1.0/applications/$id --body "{'api':{'requestedAccessTokenVersion':2}}" 
> ```    

::: zone-end
    
Your apps are now configured. The front end is now ready to access the back end with a proper access token.

For information on how to configure the access token for other providers, see [Refresh identity provider tokens](configure-authentication-oauth-tokens.md#refresh-auth-tokens).

## Call API securely from server code

In this step, you enable your previously modified server code to make authenticated calls to the back-end API.

Your front-end app now has the required permission and also adds the back end's client ID to the login parameters. Therefore, it can obtain an access token for authentication with the back-end app. App Service supplies this token to your server code by injecting a `X-MS-TOKEN-AAD-ACCESS-TOKEN` header to each authenticated request (see [Retrieve tokens in app code](configure-authentication-oauth-tokens.md#retrieve-tokens-in-app-code)).

> [!NOTE]
> These headers are injected for all supported languages. You access them using the standard pattern for each respective language.

1. In the local repository, open _Controllers/TodoController.cs_ again. Under the `TodoController(TodoContext context)` constructor, add the following code:

    ```cs
    public override void OnActionExecuting(ActionExecutingContext context)
    {
        base.OnActionExecuting(context);
    
        _client.DefaultRequestHeaders.Accept.Clear();
        _client.DefaultRequestHeaders.Authorization =
            new AuthenticationHeaderValue("Bearer", Request.Headers["X-MS-TOKEN-AAD-ACCESS-TOKEN"]);
    }
    ```

    This code adds the standard HTTP header `Authorization: Bearer <access-token>` to all remote API calls. In the ASP.NET Core MVC request execution pipeline, `OnActionExecuting` executes just before the respective action does, so each of your outgoing API call now presents the access token.

1. Save all your changes. In the local terminal window, deploy your changes to the front-end app with the following Git commands:

    ```bash
    git add .
    git commit -m "add authorization header for server code"
    git push frontend main
    ```

1. Sign in to `https://<front-end-app-name>.azurewebsites.net` again. At the user data usage agreement page, click **Accept**.

    You should now be able to create, read, update, and delete data from the back-end app as before. The only difference now is that both apps are now secured by App Service authentication and authorization, including the service-to-service calls.

Congratulations! Your server code is now accessing the back-end data on behalf of the authenticated user.

## Call API securely from browser code

In this step, you point the front-end Angular.js app to the back-end API. This way, you learn how to retrieve the access token and make API calls to the back-end app with it.

While the server code has access to request headers, client code can access `GET /.auth/me` to get the same access tokens (see [Retrieve tokens in app code](configure-authentication-oauth-tokens.md#retrieve-tokens-in-app-code)).

> [!TIP]
> This section uses the standard HTTP methods to demonstrate the secure HTTP calls. However, you can use [Microsoft Authentication Library for JavaScript](https://github.com/AzureAD/microsoft-authentication-library-for-js) to help simplify the Angular.js application pattern.
>

### Configure CORS

In the Cloud Shell, enable CORS to your client's URL by using the [`az webapp cors add`](/cli/azure/webapp/cors#az_webapp_cors_add) command. Replace the _\<back-end-app-name>_ and _\<front-end-app-name>_ placeholders.

```azurecli-interactive
az webapp cors add --resource-group myAuthResourceGroup --name <back-end-app-name> --allowed-origins 'https://<front-end-app-name>.azurewebsites.net'
```

This step is not related to authentication and authorization. However, you need it so that your browser allows the cross-domain API calls from your Angular.js app. For more information, see [Add CORS functionality](app-service-web-tutorial-rest-api.md#add-cors-functionality).

### Point Angular.js app to back-end API

1. In the local repository, open _wwwroot/index.html_.

1. In Line 51, set the `apiEndpoint` variable to the HTTPS URL of your back-end app (`https://<back-end-app-name>.azurewebsites.net`). Replace _\<back-end-app-name>_ with your app name in App Service.

1. In the local repository, open _wwwroot/app/scripts/todoListSvc.js_ and see that `apiEndpoint` is prepended to all the API calls. Your Angular.js app is now calling the back-end APIs. 

### Add access token to API calls

1. In _wwwroot/app/scripts/todoListSvc.js_, above the list of API calls (above the line `getItems : function(){`), add the following function to the list:

    ```javascript
    setAuth: function (token) {
        $http.defaults.headers.common['Authorization'] = 'Bearer ' + token;
    },
    ```

    This function is called to set the default `Authorization` header with the access token. You call it in the next step.

1. In the local repository, open _wwwroot/app/scripts/app.js_ and find the following code:

    ```javascript
    $routeProvider.when("/Home", {
        controller: "todoListCtrl",
        templateUrl: "/App/Views/TodoList.html",
    }).otherwise({ redirectTo: "/Home" });
    ```

1. Replace the entire code block with the following code:

    ```javascript
    $routeProvider.when("/Home", {
        controller: "todoListCtrl",
        templateUrl: "/App/Views/TodoList.html",
        resolve: {
            token: ['$http', 'todoListSvc', function ($http, todoListSvc) {
                return $http.get('/.auth/me').then(function (response) {
                    todoListSvc.setAuth(response.data[0].access_token);
                    return response.data[0].access_token;
                });
            }]
        },
    }).otherwise({ redirectTo: "/Home" });
    ```

    The new change adds the `resolve` mapping that calls `/.auth/me` and sets the access token. It makes sure you have the access token before instantiating the `todoListCtrl` controller. That way all API calls by the controller includes the token.

### Deploy updates and test

1. Save all your changes. In the local terminal window, deploy your changes to the front-end app with the following Git commands:

    ```bash
    git add .
    git commit -m "add authorization header for Angular"
    git push frontend main
    ```

1. Navigate to `https://<front-end-app-name>.azurewebsites.net` again. You should now be able to create, read, update, and delete data from the back-end app, directly in the Angular.js app.

Congratulations! Your client code is now accessing the back-end data on behalf of the authenticated user.

## When access tokens expire

Your access token expires after some time. For information on how to refresh your access tokens without requiring users to reauthenticate with your app, see [Refresh identity provider tokens](configure-authentication-oauth-tokens.md#refresh-auth-tokens).

## Clean up resources

In the preceding steps, you created Azure resources in a resource group. If you don't expect to need these resources in the future, delete the resource group by running the following command in the Cloud Shell:

```azurecli-interactive
az group delete --name myAuthResourceGroup
```

This command may take a minute to run.

<a name="next"></a>
## Next steps

What you learned:

> [!div class="checklist"]
> * Enable built-in authentication and authorization
> * Secure apps against unauthenticated requests
> * Use Azure Active Directory as the identity provider
> * Access a remote app on behalf of the signed-in user
> * Secure service-to-service calls with token authentication
> * Use access tokens from server code
> * Use access tokens from client (browser) code

Advance to the next tutorial to learn how to map a custom DNS name to your app.

> [!div class="nextstepaction"]
> [Map an existing custom DNS name to Azure App Service](app-service-web-tutorial-custom-domain.md)
