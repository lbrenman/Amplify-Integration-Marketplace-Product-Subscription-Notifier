# Amplify Integration Axway Marketplace Product Subscription Notifier

An Amplify Integration project for notifying Axway Marketplace Product subscribers and approvers via Microsoft Teams and via email as shown below:

![Image](https://i.imgur.com/5HUCv3C.png)

Once setup and configured properly, the integration performs the following actions:
* Sends a message to MS Teams via an HTTP/S POST when a marketplace product subscription requested is created, approved or rejected. This is useful for a subscription approval team so they can be aware of requests
![Image](https://i.imgur.com/SQVCTgd.png)
![Image](https://i.imgur.com/zjBRQgf.png)
* Sends an email via SMTP to a team member with subscription approval role from the team that owns the product when a marketplace product subscription requested is created, approved or rejected.
![Image](https://i.imgur.com/nyC701A.png)
![Image](https://i.imgur.com/PPtTppG.png)
* Sends an email to the subscriber via SMTP when their request is approved or rejected
![Image](https://i.imgur.com/ujKQlic.png)

## Amplify Integration Setup

Follow these steps to setup the integration:

* Import the project, MPSRN.zip, from this repo from the Manager->Environment using the Import feature
![Image](https://i.imgur.com/JIRHHwY.png)
* Update the `Amplify Central API HTTPS Client` connection with an Amplify Service Account Client Id and Secret. The Service Account should have Amplify Central Admin rights.
![Image](https://i.imgur.com/SxoOkP2.png)
* Do the same for the `Amplify Platform API HTTPS Client` connection with the same Client Id and Secret
![Image](https://i.imgur.com/57h8TC8.png)
* Update the `MS Team - Product Subscription Approvals` HTTP/S Client connection and provide an Incoming Webhook Connector URL for the MS Teams channel you want to receive notifications on
![Image](https://i.imgur.com/xW06WzE.png)
* Update the `Send In Blue LBG` Email connection and provide your SMTP parameters for sending emails
![Image](https://i.imgur.com/KPqZuGf.png)
* Update the `Webhook Trigger HTTPS Server Basic` connection and provide any credentials you'd like for Basic authentication or set the Authentication to None
![Image](https://i.imgur.com/sOlSArk.png)
  * You will need the Basic Authentication credentials for when you configure the Marketplace webhook in the next section. For example, if you set the username/password to `abcd/1234`, then your Authorization header value will be `Basic YWJjZDoxMjM0`
* Open the Integration `Amplify MPSR Subscription Approver Notifier` and click on the second component in the integration. It is a Map component with label `Declare Vars and Parse webhook payload` and expand the bottom panel and expand the `Vars` variable in the Pipe-out
![Image](https://i.imgur.com/b2vGMxo.png)
* Right click on the following variables and make sure that they are set properly for your environment:
  * `AmplifyCentralBaseAddress` - base address of your Amplify Central environment (e.g. https://apicentral.axway.com)
  * `MarketplaceBaseURL` - base address of your Marketplace (e.g. https://lbrenman.marketplace.us.axway.com)
  * `EmailMessageSubject` - the email subject for the emails sent to the Subscription Approver (e.g. Marketplace product subscription status update)
  * `EmailMessageFromEmailAddress` - the email reply to email address for the emails sent to the Subscription Approver (e.g. no-reply@axway.com)
* Enable the integration and get the URL as you will need this in the next section
![Image](https://i.imgur.com/4lGyYXP.png)


## Marketplace Setup

Follow these steps to setup the Marketplace webhook that will trigger the integration above whenever a subscription request is made and whenever the request is approved or rejected:

* Download the `mpsrapprovalworflowintegration.yaml` YAML file from the repo and edit it:
  * Enter the URL of the Integration above in the Webhook section
  * Enter the Authorization header value in the Secrets section
* Use the CLI to create the resource using the Axway CLI command `axway central create -f mpsrapprovalworflowintegration.yaml`