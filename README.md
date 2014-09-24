Splunk App for Heroku
=================

Welcome to the Splunk app for Heroku
-------------

**Thanks for downloading the Splunk app for Heroku** it's great to see you here. Introductions over, lets get down to business.

**Important:** this app will NOT work unless you follow the setup instructions below - they're super simple to setup, I promise.

__For your convenience, these instructions can also be found in the Splunk GUI once the app has been installed by visiting "help!" > "setup".__

I'm 12, what is this?
-------------

The Splunk app for Heroku helps guide you through improving the logging functionality of your Heroku app, and how to get these logs into Splunk for analysis. To help you get started this app comes prebuilt with searches powering a number of dashboards and alerts for the ingested log data.

What's in the box?
-------------

*1 x New Index (index=heroku)
*1 x TCP input (port:514)
*4 x Prebuilt Dashboards (application statistics, heroku errors, load average, memory usage)
*3 x Prebuilt Alerts (app response time)
*1 x Important Setup Instructions (you are here)

Setup
-------------

**0) Overview**

There are 4 steps you will need to get this app working.

*Configure logging on Heroku
*Set up Splunk input
*Point Heroku syslog drain at Splunk
*Profit

**1) Configure logging on Heroku**

The searches used in this app require some [**Heroku labs features**](https://devcenter.heroku.com/categories/labs) to be enabled.

Assuming you have the [**Heroku toolbelt installed**](https://toolbelt.heroku.com/), you should run the following commands from the CLI.

Turn on debug logging:

$ heroku config:add LOG_LEVEL=DEBUG --app "YOUR_APP_NAME"</code>

Turn on runtime logging:

$ heroku labs:enable log-runtime-metrics --app "YOUR_APP_NAME"

Restart your app to configure changes:

heroku restart --app "YOUR_APP_NAME"</code>

**2) Set up Splunk input**

When installed this app will do two things: 1) Create a TCP input for port:514 (switched off by default), and 2) Create a new index called "Heroku".

You can enable this input in the Splunk GUI by navigating to: "Settings" > "Data Inputs" > "TCP"

__Important: It is essential the port you choose is open and traffic is not blocked.__

**3) Point Heroku syslog drain at Splunk**

Now we need to direct the newly enabled Heroku logs at this source. In the CLI you can do this by running the following commands:

$ heroku drains:add syslog://"YOUR_INDEXER'S_IP":"PORT_FOR_INPUT" --app "YOUR_APP_NAME"</code>

**4) Profit**

If everything has worked well, a simple search for "index=heroku" should return some results. You might need to restart Splunk first.

Should some of the dashboards be empty, run a search in Splunk to see if the fields used are in your logs. If you're still suspicious, check Heroku logging has been set correctly at step 1. __Sometimes you may not have any logs that populate the search - this is usually a good thing where errors are concerened.__

[END]

Enjoy!

[**@himynamesdave**](https://twitter.com/himynamesdave)