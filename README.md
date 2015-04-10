# Heroku App for Splunk

**Thanks for downloading the Heroku App for Splunk!**

The Splunk app for Heroku uses Heroku syslog drains to collect the logs generated by your apps into Splunk for powerful analysis.

To help you get started this app comes prebuilt with searches powering a number of dashboards and alerts for the ingested log data.

**Important:** This app will NOT work unless you follow the setup instructions below - they're super simple to setup, I promise.

**What's in the box?**

* 1 x Index (index=heroku)
* 1 x TCP Input (port:514)
* 4 x Dashboards (App Analytics, App Performance, App Errors)
* 3 x Prebuilt Alerts (Response Times, Errors)

## Setup

There are 4 steps you will need to get this app working:

1. Configure logging on Heroku
2. Enable Splunk input
3. Point Heroku syslog drain at Splunk
4. Define app names in lookup
5. Turn on alerts

**1) Configure logging on Heroku**

The searches used in this app require some [**Heroku labs features**](https://devcenter.heroku.com/categories/labs) to be enabled.

Assuming you have the [**Heroku toolbelt installed**](https://toolbelt.heroku.com/), you should run the following commands from the CLI.

Turn on debug logging:

```$ heroku config:add LOG_LEVEL=DEBUG --app <YOUR_APP_NAME>```

Turn on runtime logging:

```$ heroku labs:enable log-runtime-metrics --app <YOUR_APP_NAME>```

Restart your app to configure changes:

```$ heroku restart --app <YOUR_APP_NAME>```

**2) Enable Splunk input**

When installed this app will create a TCP input for port:514 (switched off by default), and also create a new index called "Heroku".

If you want to enable data collection through this port you can enable this input in the Splunk GUI by navigating to: "Settings" > "Data Inputs" > "TCP".

If you would like to use a different port you can add a new TCP input in the Splunk GUI by navigating to: "Settings" > "Data Inputs" > "TCP". Make sure you set the sourcetype="syslog", and index="Heroku".

**Sanity checkpoint:** It is essential the port you recieve data is open and traffic is not blocked.

**3) Point Heroku syslog drain at Splunk**

In the CLI you can do this by running the following commands:

```$ heroku drains:add syslog://<YOUR_INDEXER'S_IP>:<PORT_FOR_INPUT> --app <YOUR_APP_NAME>```

**4) Define app names in lookup**

Stop Splunk and open the following file:

```<SPLUNK_HOME>/etc/apps/splunk-heroku-app/lookups/app_name.csv```

In the host column you will need to specify the application identifier provided by Heroku. You can find this identifier in the "host" field of your Heroku event logs in Splunk.

You can then set a more user friendly name for searching in the "app_name" column. If desired, you can also specify a URL for the application to in the "app_url" column.

**5) Turn on alerts**

This app comes with 2 prebuilt alerts for response time and errors. y default they are disabled.

To view them in Splunk navigate to "Heroku" > "Alerts". You can click on each alert to view to amend the alert triggers, modify the search, or enable the alert.

## Getting Started

If everything has worked well, a simple search for "index=heroku" should return some results. You might need to restart Splunk first.

Should some of the dashboards be empty, run a search in Splunk to see if the fields used are in your logs. If you're still suspicious, check Heroku logging has been set correctly at step 1.

In some cases you may not have any logs that populate the search just yet - this is usually a good thing where errors are concerened.

## Where Can I Go For Help?

In the first instance any problems or issues with this app should be [**posted on Splunk Answers here**](http://answers.splunk.com/app/questions/1873.html). I follow all new threads and will aim to reply within 1 working day.

Should the problem need a more in-depth solution I will reach out directly via email for a solution from your thread.

If you have any feature requests for this app these can be directed to me [**via email**](mailto:support@splunkstart.com).

You can [**find and fork the latest codebase of this app on Github**](https://github.com/himynamesdave/splunk-heroku-app), you can fork the code here. I'll happily accept new pull requests.

# Current Feature Requests

* Support for drains over SSL (Heroku beta feature) - coming v2