# mfa_test_harness
A lightweight test harness that allows quick evaluation of Okta's sign-on policies

## Components
**1. src/policy_demo_api.html**
  This is the main test harness file.  It is a very basic vue.js app.  It is configured as a trusted application within Okta so that location/device information may be passed in at login time.

**2. container_app folder**
  To host the policy_demo_api.html file, a small container with a web server is spun up inside a docker container.  This folder contains the configuration for that container.

**3. src/environment.js.sample**
  This file is the configuration file that will need to be renamed to "environment.js", and edited to contain your specific Okta org values.

## Pre-Requisites
**1. Docker w/ docker-compose**
  For actually running the solution.

**2. At least 1 Okta org**
  For evaluating policies against.

## How To Build
**1. Download this repository**
    Either download as a zip file (https://github.com/dancinnamon-okta/mfa_test_harness/archive/master.zip) or by using git.

**2. Specify your settings in the src/environment.js file (an example is included- rename this file)**
   Only 2 things needed:
   1. mfa_okta_org (set to your Okta org name https://subdomain.okta.com for example)
   2. mfa_api_key (obtain an api key for your org and specify here)

   *Note- in src/environment.js, there are settings for 2 Okta orgs.  The intent is to show the differences in feature-sets between MFA and Adaptive MFA in Okta.  There is a checkbox on the login form to select which set of variables to actually use.*

**3. Run docker-compose build**
Running

`docker-compose build`

in your favorite shell will fetch the proper environment for running the demo- as shown:
![MFA Test Harness Docker Build](https://github.com/dancinnamon-okta/mfa_test_harness/blob/master/readme_images/Docker_Build.jpg "MFA Test Harness Docker Build")
**You're done!**

## How To Run
In your same working directory (that was created when you unzipped the repository in step 1 of the build process) run:
`docker-compose up`

That command will start the container, and the web app will be hosted locally.

Visit http://localhost:8080 to see your creation!  Note that the application will not work yet until your Okta org is configured.

## How to Configure Okta
While this guide assumes a working understanding of how to configure Okta, there are a few requirements when setting up the new Okta application that you should be aware of.

**1. Create an API key (if not done already)**
  In order for Okta to "trust" your test harness to pass through client location and device info, an API key is needed.  Generate one in the Security/API menu in Okta.  The key value must be provided in the environment.js file.

**2. Set your current IP address as a trusted gateway/proxy**
  In addition to requiring an API key to "trust" your test harness, your machine must also be setup as a trusted proxy in Okta.  In Okta, create a network zone called "MFA Test Harness", and add your current IP address, as shown.
  ![MFA Test Harness Network Zone](https://github.com/dancinnamon-okta/mfa_test_harness/blob/master/readme_images/Network_Zone.jpg "MFA Test Harness Network Zone")

  *Note- This is not a one-time operation.  This step should always be performed prior to use!*

**3. Add http://localhost:8080 as a trusted origin**
  This step is required to enable your browser to run local+Okta code concurrently- through the use of CORS policy.
  ![MFA Test Harness Trusted Origin](https://github.com/dancinnamon-okta/mfa_test_harness/blob/master/readme_images/Trusted_Origin.jpg "MFA Test Harness Trusted Origin")

**You're done!**

## How to verify it's working
Since setting authentication location/device information is a sensitive operation, there are some checks/balances in place to ensure this ability is not mis-used.
**1. Login as a user, using the test harness.  Select "Ireland" as the location.**
At this time, you'll either be authenticated into Okta, or challenged for MFA.  It doesn't matter at this point which occurs.

**2. Login to the normal Okta administrative interface, and view the system logs.**
Find your authentication attempt in the system log- as shown.  Expand the "client" information, and you should see "Ireland", as shown.
![MFA Test Harness Log Example](https://github.com/dancinnamon-okta/mfa_test_harness/blob/master/readme_images/Log_Example.jpg "MFA Test Harness Log Example")

*Note- you must see "Ireland" in the CLIENT section of the event.  Even if configured incorrectly, you may see "Ireland" somewhere in the log entry as a link in the proxy IP Chain.  It MUST be in the client section to be evaluated by Okta MFA policies properly!!*


## Restrictions/Caveats
**1. Only OTP MFA factors are supported by the test harness.**
If you're challenged by Okta for MFA according to policy, the test harness will select the first available OTP MFA factor and will use that.  The user must also be previously enrolled in an OTP factor.

**2. Run localhost only**
The behaviors shown here in this demo are highly sensitive, and requires a secret API key.  Putting this key in javascript is normally a very bad thing- so the way this application is designed is intended for local demo/exploration purposes only, and should NOT be hosted on any publicly accessible site.
