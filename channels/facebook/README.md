# Deploying a Facebook Messenger app

### NOTICE: This software is still under development and should not be used until this notice is removed.

You can use either of two methods to deploy your workspace to a Facebook Messenger app:

- Use [automated deployment](#automated-deployment) if you want to quickly deploy your app by filling out a form with the required information. The deployment process is automated using a Bluemix DevOps toolchain.

- Use [manual deployment](#manual-deployment) if you want to deploy your app by modifying configuration files and running scripts. You might want to use this method if you are customizing the Conversation connector, or if you need to repair or update components of an existing deployment.

## Automated deployment

1.  If you are not already logged in to IBM Bluemix, [log in](https://console.bluemix.net/login) before continuing.

1.  Right-click this button to launch the deployment in a new tab or window:

    [![Deploy to Bluemix](https://bluemix.net/deploy/button.png)](https://console.bluemix.net/devops/setup/deploy/?repository=https://github.com/watson-developer-cloud/conversation-connector)

    Leave this page open so you can follow the instructions as you complete the process.

1.  Under **Tool Integrations**, click the GitHub tile, and then click **Authorize** to grant Bluemix access to your GitHub account. Log in with your GitHub credentials and complete the authorization process.

    **Note:** If you do not see an **Authorize** button, IBM Bluemix might already be authorized. In this case, you can skip this step.

1.  Click the Delivery Pipeline tile.

1.  Under **Tool Integrations**, select the Bluemix region, organization, and space where you want to deploy. This space will be used to deploy the IBM Cloud Functions artifacts, as well as a Cloudant Lite instance.

    **Note:** Currently, only the US South region is supported.

1.  In the **App name** field, type a name for your deployment. This name will help you find your Cloud Functions assets later. You can type any string, but only alphanumeric characters (A-Z and 0-9) are kept.

1.  Go to [https://developers.facebook.com/apps/](https://developers.facebook.com/apps/). Log in with your Facebook credentials if necessary.

1.  Click **Add a New App**. Specify a display name for your app and your contact email address, and then click **Create App ID**.

    **Note:** If you have already created the app you want to use, select it from the **My Apps** menu.

1.  In the navigation pane, click **Dashboard**.

1.  Click **Show** and then copy the displayed app secret to the clipboard.

1.  In the deployment form, paste the app secret into the **Facebook App Secret** field.

1.  In the Facebook app page, click **+ Add Product**. Under **Select a product**, hover over **Messenger** and click **Set Up**. 

1.  In the Messenger settings, scroll down to **Token Generation**. Click **Select a Page** and choose the Facebook page you want to use for your app. 

    **Note:** If you don't already have a page for your app, click **Create a new page**. After you finish creating the page, return to the [Facebook apps page](https://developers.facebook.com/apps/) and navigate back to the Messenger settings for your app. You can then select the page you created.

1.  Copy the page access token and paste it into the **Facebook Page Access Token** field of the deployment form.

1.  In the **Facebook Verification Token** field, type any string you want to use as a verification token. Make a record of this value, which you will need later. (Facebook will use this verification token to verify your webhook URL.)

1.  Open the [Conversation tool](https://watson-conversation.ng.bluemix.net), and then open the Conversation workspace you want to deploy.

1.  Select **Deploy** from the menu and then go to the **Credentials** tab. Copy your Conversation username, password, and workspace ID, and paste them into the corresponding fields in the deployment form.

1.  Click **Create** to start the deployment.

1.  On the deployment page, click the Delivery Pipeline tile so you can monitor the deployment. You should see a **Build Stage** tile, followed by a **Deploy Stage** tile. Wait until the deploy stage finishes, indicated by a STAGE PASSED message (this might take several minutes).

1.  In the **Deploy Stage** tile, click **View logs and history**. Scroll to the bottom of the log and look for a line that begins `Your Request URL is:`. Copy the specified URL to the clipboard.

1.  In the Facebook app settings, go to the Messenger settings and scroll to the **Webhooks** section. Click **Setup Webhooks**.

1.  In the **New Page Subscription** window, paste the request URL from the clipboard into the **Callback URL** field. In the **Verify Token** field, specify the same Facebook verification token that you created earlier. Under **Subscription Fields**, select **messages** and **messaging_postbacks**. Then click **Verify and Save**.

1.  After the verification finishes, go back to the **Webhooks** section in the Messenger settings and click **Select a Page**. Select the same page you selected during token generation, and then click **Subscribe**.

    **Note:** Subscribe to only one page. Multiple-page subscriptions are not currently supported.

That's it. You're all set. You can now go to Facebook Messenger, search for your Facebook bot (or the Facebook page you subscribed to), and talk to it!

## Manual deployment

1.  Clone or download this GitHub repo to your local file system.

1.  If you have not done so already, install the following prerequisite software:

    - The [Cloud Foundry command-line interface](https://docs.cloudfoundry.org/cf-cli/install-go-cli.html)
    - The [Node.js runtime](https://nodejs.org/), including the npm package manager

1.  Log in using the following command:

    `cf login -a https://api.ng.bluemix.net/`

    Select the Bluemix organization and space where you want to deploy.

1.  Go to https://console.ng.bluemix.net/openwhisk/learn/cli and then do the following:

    1.  Click the account information in the upper right corner, and confirm that the organization and space shown are correct.

    1.  Scroll down and click on **Looking for the wsk CLI?**?

    1.  If you have not done so already, follow the instructions to install the wsk CLI.

    1.  Copy the command in step 2 ("Target a Region and Namespace"), and run it in your CLI.

1.  Go to [https://developers.facebook.com/apps/](https://developers.facebook.com/apps/). Log in with your Facebook credentials if necessary.

1.  Click **Add a New App**. Specify a display name for your app and your contact email address, and then click **Create App ID**.

    **Note:** If you have already created the app you want to use, select it from the **My Apps** menu.

1.  In the navigation pane, click **Dashboard**.

1.  Click **Show** and then copy the displayed app secret to the clipboard. Paste this value into the `providers.json` file.

1.  In the Facebook app page, click **+ Add Product**. Under **Select a product**, hover over **Messenger** and click **Set Up**. 

1.  In the Messenger settings, scroll down to **Token Generation**. Click **Select a Page** and choose the Facebook page you want to use for your app. 

    **Note:** If you don't already have a page for your app, click **Create a new page**. After you finish creating the page, return to the [Facebook apps page](https://developers.facebook.com/apps/) and navigate back to the Messenger settings for your app. You can then select the page you created.

1.  Copy the page access token and paste it into the `providers.json` file.

1.  In `providers.json`, add a value for `verfication_token`. This can be any string you want to use as a verification token. Make a record of this value, which you will need later. (Facebook will use this verification token to verify your webhook URL.)

1.  In `providers.json`, make sure the channel `name` is set to `facebook`, and also add an appropriate pipeline `name`. For more information about the pipeline, please refer to the [main repository README](../../README.md).

1.  In `providers.json`, edit the `conversation` object to specify the username, password, and workspace ID of the workspace you are deploying.

1.  Run `npm install`.

1.  `cd` to the root of the repository and run `./setup.sh -s`. This creates the Facebook Cloud Functions package, as well as all the other packages in your namespace.

1.  Copy the generated `${pipeline-name}_facebook/receive` endpoint URL. You can copy this directly from the terminal window after the script completes (look for a message that begins `Your Request URL is:`). If you need to find the endpoint URL later, follow these steps:

    1.  Go to https://console.ng.bluemix.net/openwhisk/editor.

    1.  Under **My Actions**,  click <pipeline_name>_facebook/receive>.

    1.  Click **View Action Details**.

    1.  Confirm that **Enable as Web Action** is selected. Copy the URL from the **Web Action URL** You will need to replace `.json` with `.text` when you paste this value into the Facebook webhook settings.

1.  In the Facebook app settings, go to the Messenger settings and scroll to the **Webhooks** section. Click **Setup Webhooks**.

1.  In the **New Page Subscription** window, paste the request URL from the clipboard into the **Callback URL** field. (If the URL you pasted ends with `.json`, change this to `.text`.) In the **Verify Token** field, specify the same Facebook verification token that you created earlier. Under **Subscription Fields**, select **messages** and **messaging_postbacks**. Then click **Verify and Save**.

1.  After the verification finishes, go back to the **Webhooks** section in the Messenger settings and click **Select a Page**. Select the same page you selected during token generation, and then click **Subscribe**.

    **Note:** Subscribe to only one page. Multiple-page subscriptions are not currently supported.

That's it. You're all set. You can now go to Facebook Messenger, search for your Facebook bot (or the Facebook page you subscribed to), and talk to it!