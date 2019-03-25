# mfa_test_harness
A lightweight test harness that allows quick evaluation of Okta's sign-on policies

## Components
**1. src/policy_demo_api.html**
  This is the main test harness file.  It is a very basic vue.js app.  It is configured as a trusted application within Okta so that location/device information may be passed in at login time.
   
**2. container_app folder**
  To host the policy_demo_api.html file, a small container with a webserver is spun up inside a docker container.  This folder contains the configuration for that container.
   
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
   1. OKTA_ORG (set to your Okta org name https://subdomain.okta.com for example)
   2. API_KEY (obtain an API_KEY for your org and specify here)
   
   *Note- in src/environment.js, there are settings for 2 Okta orgs.  The intent is to show the differences in feature-sets between MFA and Adaptive MFA in Okta.  There is a checkbox on the login form to select which set of variables to actually use.*

**3. Run docker-compose build**

**You're done!**

## How To Run
In your same working directory (that was created when you unzipped the repository in step 1 of the build process) run:
`docker-compose up`

That command will start the container, and the web app will be hosted locally.

Visit http://localhost:8080 to see your creation!  Note that the application will not work yet until your Okta org is configured.

## How to Configure Okta
While this guide assumes a working understanding of how to configure Okta, there are a few requirements when setting up the new Okta application that you should be aware of.
