---
Title: Google Workspace SAML integration guide
linkTitle: Google workspace integration
description: This integration guide shows how to configure Google Workspace as a SAML single sign on provider for your Redis Cloud account.
weight: 52
alwaysopen: false
categories: ["RC"]
---

This guide shows how to configure [Google Workspace](https://workspace.google.com/) as a SAML single sign-on identity provider (IdP) for your Redis Cloud account.

To learn more about Redis Cloud support for SAML, see [SAML single sign-on]({{<relref "/rc/security/single-sign-on/saml-sso">}}).

## Step 1: Setup your identity provider (IdP)

### Create the Google Workspace SAML application

1. Sign in to your [Google Workspace admin account](https://admin.google.com/).

2. From the main menu, select **Apps** then **Web and mobile apps**.

   {{<image filename="images/rc/saml/google_workspace_saml_0.png" alt="" >}}{{</image>}}

3. Once in Web and mobile apps, select **Add custom SAML app** from the dropdown list.

   {{<image filename="images/rc/saml/google_workspace_saml_1.png" alt="" >}}{{</image>}}

4. To begin, change the **App name** and **Description** to **Redis Cloud**. You can also choose an **App icon** for the application. We suggest you upload a Redis icon that you can find on the Internet. Once complete, select **Continue**.

   {{<image filename="images/rc/saml/google_workspace_saml_2.png" alt="" >}}{{</image>}}

5. In the next screen, you will find all of the information needed to configure SAML in Redis Cloud. Select the **copy** button for the three following sections of information:

* SSO URL
* Entity ID
* Certificate

   {{<image filename="images/rc/saml/google_workspace_saml_3.png" alt="" >}}{{</image>}}

Once complete, select **Continue**.

## Step 2: Configure SAML support in Redis Cloud

Now that you have your Google Workspace IdP server information, configure support for SAML in Redis Cloud.

### Log in to your Redis Cloud account

Log in to your account at [Redis admin console](https://app.redislabs.com/#/login)

### Activate SAML in Access Management

To activate SAML, you must have a local user (or social sign-on user) with the `owner` role. If you have the correct permissions, you will see the **Single Sign-On** tab.

{{<image filename="images/rc/saml/aws_iam_identity_center_saml_7.png" alt="" >}}{{</image>}}

1. Add the information you saved previously in the **Google identity provider details** screen. This includes:

    * **Issuer (IdP Entity ID)**: Entity ID.
    * **IdP server URL**: SSO URL.
    * **Assertion signing certificate**: Certificate.

   Also add:

    * **Email domain binding** - The domain used in your company's email addresses.

      {{<image filename="images/rc/saml/google_workspace_saml_4.png" alt="" >}}{{</image>}}

   Select **Enable** and wait a few seconds for the status to change.

2. Select **Download** to get the service provider (SP) metadata. Save the file to your local hard disk.

   {{<image filename="images/rc/saml/google_workspace_saml_5.png" alt="" >}}{{</image>}}

3. Open the file in any text editor. Save the following text from the metadata:

    * **EntityID** - The unique name of the service provider (SP).

      {{<image filename="images/rc/saml/sm_saml_4.png" alt="" >}}{{</image>}}

   * **Location** : The location of the assertion consumer service.

  {{<image filename="images/rc/saml/sm_saml_5.png" alt="" >}}{{</image>}}

## Step 3: Finish SAML configuration in Google Workspace's Redis Cloud Application

1. Return to the **Service provider details** screen in Google Workspace, and add the following information:

   * **ACS URL**: The EntityID from the downloaded file from Redis Cloud
   * **Entity Id**: The Location from the downloaded file from Redis Cloud

   {{<image filename="images/rc/saml/google_workspace_saml_6.png" alt="" >}}{{</image>}}

Leave the **Name ID** default information as it is. Once complete, select **Continue**.

2. If you would like to also configure an IdP initiated workflow, fill in the **relay state** field in the **Application properties** section. Use this URL: `https://app.redislabs.com/#/login/?idpId=XXXXXX`. Take the ID from the location URL in step 3 (the content after the last forward slash "/") and append to the URL.

   {{<image filename="images/rc/saml/aws_iam_identity_center_saml_11.png" alt="" >}}{{</image>}}

1. Select **Submit** to finish creating the application.

   {{<image filename="images/rc/saml/aws_iam_identity_center_saml_12.png" alt="" >}}{{</image>}}

1. Configure the **Redis Cloud** application's attribute mappings. Select **Actions > Edit Attribute Mappings**.

   {{<image filename="images/rc/saml/aws_iam_identity_center_saml_13.png" alt="" >}}{{</image>}}

   In the next screen, add these attributes:

    * **Subject**: `${user:email}`, `unspecified`
    * **Email**: `${user:email}`, `unspecified`
    * **FirstName**: `${user:givenName}`, `unspecified`
    * **LastName**: `${user:familyName}`, `unspecified`
    * **redisAccountMapping**: `XXXXXXX=owner`, `unspecified`

The `redisAccountMapping` key-value pair consists of the lowercase role name (owner, member, manager, or viewer) and your Redis Cloud Account ID found in the [account settings]({{<relref "rc/accounts/account-settings">}}).

{{<image filename="images/rc/saml/aws_iam_identity_center_saml_14.png" alt="" >}}{{</image>}}

## Step 4: Ensure that the Cloud account user has an IAM Identity Center user account

To complete SAML setup, ensure that the user who began SAML configuration in Redis Cloud admin console has a user defined in the AWS IAM identity center. This user account is required to complete the SAML setup.

Also, make sure that the user has been assigned to the **Redis Cloud** Application.

## Step 5: Activate SAML integration

The final step in our SAML integration with AWS IAM identity Center is to activate the SAML integration.

1. In the Single Sign-On screen, select **Activate**.

   {{<image filename="images/rc/saml/aws_iam_identity_center_saml_15.png" alt="" >}}{{</image>}}

A logout notification screen displays, letting you know that you are redirected to AWS IAM Identity Center's login screen.

{{<image filename="images/rc/saml/aws_iam_identity_center_saml_16.png" alt="" >}}{{</image>}}

1. Enter you AWS IAM Identity Center credentials.

   {{<image filename="images/rc/saml/aws_iam_identity_center_saml_18.png" alt="" >}}{{</image>}}

1. If everything is configured correctly, you should get a **SAML activation succeeded** message. From this point forward, users need to click **SSO** to log in to the Redis Cloud admin console.

   {{<image filename="images/rc/saml/aws_iam_identity_center_saml_19.png" alt="" >}}{{</image>}}

A message displays, stating that your local user is now converted to a SAML user. Select **Confirm**.

You have successfully configured AWS IAM Identity Center as an identification provider.

{{<image filename="images/rc/saml/aws_iam_identity_center_saml_22.png" alt="" >}}{{</image>}}