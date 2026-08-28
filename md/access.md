# Getting access

Part of the ePost API documentation. Canonical page: https://developer.klara.ch/epost-preview/#access

## Getting access

### Test environment

Self-service, no approval needed. The API lives on `https://api-test.klara-epost.tech`, the web interface on `test.klara-epost.tech`.

1. Open `test.klara-epost.tech` in a desktop browser and register. **The email address and password you set here are the credentials you will use in the API token flow.** You may use different ones on production.

2. Register as a business tenant, accept the terms and enter your company details.

3. Open the widgets and activate **eLetter: Versand**, free of charge.

> **Warning: Which endpoints you may call depends on what you activated**
>
> This is the most common reason for an unexpected `403`. The **eLetter: Versand** subscription unlocks Sending, Preview Services, Monitoring and Standard Identity Matching. The **Digital Letterbox** has to be activated separately and unlocks the letterbox endpoints. Detailed identity matching is restricted and chargeable on top.

#### Receiving as well as sending

If you also want to receive, activate the digital letterbox on the same tenant or register a second one. Either way you get a company tenant. The step that surprises people: you have to verify the company with a code, and **on test no letter is ever posted**. Request the code by email from [enterprise support](mailto:support.enterprise@epostservice.ch), stating company name, company address and the email address you registered with.

#### What you can actually test

| Channel | On the test environment |
|---|---|
| ePost digital letterbox | **Self-service.** Works as soon as you are onboarded |
| eBill | Possible, but not self-service. Ask [enterprise support](mailto:support.enterprise@epostservice.ch) |
| SMS | Possible, but not self-service. Ask [enterprise support](mailto:support.enterprise@epostservice.ch) |
| Email | Possible, but only with a special test domain. Ask [enterprise support](mailto:support.enterprise@epostservice.ch) |
| Physical letter | Possible, but only with close support from ePost. Ask [enterprise support](mailto:support.enterprise@epostservice.ch) |

### Production environment

Not self-service. Onboarding happens together with an ePost administrator. Contact [enterprise support](mailto:support.enterprise@epostservice.ch) to start it.

### Accessing the letterbox with a private account

Private accounts are onboarded through the ePost mobile app and sign in with SwissID, which the API does not accept. To use the letterbox endpoints with a private tenant you first have to set a password:

1. Sign in at `app.epost.ch` with SwissID.

2. In the same browser, open the account page at `login.epost.ch`, go to authentication and set a password.

3. Authenticate against the API with that email and password.
