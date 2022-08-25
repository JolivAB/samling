# Samling

Serverless (as in "client side only") SAML IDP for testing SAML integrations.

This repo is forked from [fujifish/samling](https://github.com/fujifish/samling), which in turn forked it from [DFE-Digital/samling](https://github.com/DFE-Digital/samling).

## How to Use

### SAMLRequest with AuthnRequest

Use `https://jolivab.github.io/samling/?SAMLRequest=<SAML_REQUEST>` to initiate a login request via samling.

Specifying `ForceAuthn="true"` in the request will force Samling to land on the properties page instead of auto submitting the SAML response in case the user is already logged-in.

### SAMLRequest with LogoutRequest

Use `https://jolivab.github.io/samling/?SAMLRequest=<SAML_REQUEST>` to initiate a logout request. If there is an active user session, the SAML Response will be automatically posted back.

Add `manual=1` query parameters to the url to logout manually instead of the response being automatically posted back.

### IdP Metadata

The IdP metadata is available if you go to https://jolivab.github.io/samling/ and click the "IdP Metadata" link in the left navbar.

In short:

* IdP Location and Issuer: https://jolivab.github.io/samling/
* IdP Certificate Thumbprint: 5566FE04C808D0298BB121498B842D0796F9CACF

### Manual Usage

1. Open up `https://jolivab.github.io/samling/`. You'll land on the **SAML Response Properties** section.
2. Fill in the required properties fields. Required fields are marked with an asterisks (*).
   * `Name Identifier` - the user name
   * `Assertion Consumer URL` - where to send the SAML response
3. Click on **Create Response**. You will be be taken the **SAML Response** section.
4. Review the SAML response and set the session duration, then click on **Post Response**. At this point a session cookie
   is created for the logged in user.
5. You can reload the samling page and go to the **User Details** page to verify the session cookie was created.

## What is SAMLING

SAMLING is a Serverless (as-in client side only) SAML IdP for the purpose of testing SAML integrations.

It provides control over the SAML response properties to send back to the Service Provider in response to a SAML request,
including simulating errors and specifying session cookie duration to track the logged-in user.

If there is a <strong>SAMLRequest</strong> query parameter present with an `AuthnRequest`,
SAMLING will auto populate some of the fields in the `SAML Response Properties` section in preparation for creating the SAML response.

If there is a <strong>SAMLRequest</strong> query parameter present with a `LogoutRequest`,
SAMLING will log out the currently logged-in user.

Generating a SAML Response requires the use of a private key and certificate for signing the SAML Assertion.
SAMLING comes bundled with default keys, but also enables to generate a random private/public key and to save them in the local storage so they are used in
subsequent SAML responses.

## Installation

```bash
git clone https://github.com/JolivAB/samling.git
cd samling
npm install
npm run build
```

You'll end up with a `public` directory with all the required assets for loading `samling.html`.

## Docker

Note: Docker 17.05 or higher is required.

```bash
git clone https://github.com/JolivAB/samling.git
cd samling
docker build -t fujifish/samling .
docker run -d -p 8080:80 fujifish/samling
```

You can now access samling at http://localhost:8080
