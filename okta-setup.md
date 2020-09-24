## OKTA SETUP (10 minutes)

* ***SIGNUP*** Go to https://developer.okta.com/signup & provide the signup details
  * On the next page, scroll down and select "Do it Later" for customizations
  * Click on Verify your email address on the next screen
  * Upon signup, you will receive an email to verify your email address for account activation
  * Open the verification email received (note., it also contains a temporary password)
  * Once verified, you will be redirected to your OKTA developer account page
  * Note the URL in the browser bar, e.g. dev-xxxxxx.okta.com
  * Login using the email address that was used to setup the account along with the temporary password received in the email
  * Set up your new password and other security details and continue
  * You will be land up in the admin console of your OKTA developer account
  * You will be asked to customize your profile
  * You can select the appropriate role for your profile and select "Custom App" along with the languages that your prefer as a developer
  * From hereon, you can straight jump to setting up the OIDC setup for your application
  * You can ignore the other GETTING STARTED actions

----
***OIDC Setup***

* Click on the top bar "Users" > "People" in the admin page
  * Click on "Add Persons" and add a test user with a valid email address (for the password, select your preference, set by user or admin)
  * Once done, your added user will receive a verification email
  * Next, click on "Applications" on the top admin bar
  * Then click on "Add Application" and select "Web" as the type and "next"
  * You will arrive at a page to setup your Application
  * Give it a suitable name
  * Base URIs: This url must be the same as the one that you will use to access the fronend NGINX reverse proxy
    * *If deployed on docker-compose* <https://ip-address-of-your-docker-host-running-nginx-proxy>
    * *If deployed on Tanzu App Services* <https://unique-string-excalidraw-frontend.your-cf-apps-domain>
    * *If deployed on Kubernetes* <https://Mapped-FQDN-or-load-balancer-dns-name-or-nodePort-IP-and-port-number>
  * Login redirect URIs: Same as above but suffixed with redirect_uri path
    * *If deployed on docker-compose* <https://ip-address-of-your-docker-host-running-nginx-proxy/redirect_url>
    * *If deployed on Tanzu App Services* <https://unique-string-excalidraw-frontend.your-cf-apps-domain/redirect_url>
    * *If deployed on Kubernetes* <https://Mapped-FQDN-or-load-balancer-dns-name-or-nodePort-IP-and-port-number/redirect_url>
 
  * Leave everything else to defaults and click done
  * On the next page, you will see a "Client ID" and "Client secret" value. Copy those for further use
  * Next, click on the "Assignments" tab for your Application
  * Click on "Assign" > "Assign to People" and select users that should be able to access this application
  * Next, click on "API" on the top admin bar and then "Trusted Origins"
  * Click on "Add Origins" and give a suitable name and in the origin URL, type the same value as in BASE URL and also select both the options in TYPE
    * *If deployed on docker-compose* <https://ip-address-of-your-docker-host-running-nginx-proxy>
    * *If deployed on Tanzu App Services* <https://unique-string-excalidraw-frontend.your-cf-apps-domain>
    * *If deployed on Kubernetes* <https://Mapped-FQDN-or-load-balancer-dns-name-or-nodePort-IP-and-port-number>

----
***Retrieve values for env variables use during app deployment***

  * Now you are done with the OKTA setup. At the end of this exercise, you will need the below values to proceed further
    * Your OKTA URL dev-xxxxxx.okta.com
    * CLIENT ID, CLIENT SECRET and the REDIRECT URL. These values are gathered during the above steps.
  * We will need these values as variables while deploying our application
