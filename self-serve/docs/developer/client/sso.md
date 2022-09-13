# Google SSO Token
Integrating Google SSO with Blotout requires App registration on Google Console to get the Client ID and Secret Token.

OAuth Reference Link - https://developers.google.com/identity/protocols/oauth2

Follow the below steps to get the Client ID and Secret Token
1. Go to the URL https://console.developers.google.com/ 

2. First Create a new Project
    ![](assets/images/sso/add_project.png)
    ![](assets/images/sso/new_project.png)

3. Click on Oauth Consent Screen and select the type of users 
    ![](assets/images/sso/oauth_consent_1.png)

4. Fill below details and provide the whitelist domain details under Authorized domains section and save
    ![](assets/images/sso/oauth_consent_2.png)

5. Add scope for email,profile and openid      
    ![](assets/images/sso/scope.png)

6. Click on Create Credentials Menu and then choose OAuth Client ID (showing below)
    ![](assets/images/sso/credentials.png)

7. Choose the Web Application as below
    ![](assets/images/sso/choose_app.png)

8. Add the Application URL - http://<app-domain-name>/api/google/v1/callback in Authorized redirect URIs and click on Create to get the Client Id and Client Secret.