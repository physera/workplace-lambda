# Integrate services with Facebook Workplace via AWS Lambda

## Overview

This documents contains pointers to other repositories which implement integrations between various services and [Facebook Workplace](https://workplace.facebook.com). The strategy employed by these integrations is to:

1. Set up callbacks for a particular service to hit an endpoint hosted on [AWS Lambda](https://aws.amazon.com/lambda/).
2. Set up the Workplace environment with a bot for the service.
3. Have the AWS Lambda function receive the callback payload, format it into a post, and call the Facebook API.

## Setup

To make this work you'll need to set things up on the service in question, Workplace, and AWS Lambda. The following describes the common set up steps for all of the integrations.

### Set up Workplace

* Create a group you want the posts to appear in.  Take note of the group ID (this is the numeric ID in the url for the group).
* As an admin, go to manage apps.  Create a new app and make sure it has permission to post to the group you just created.
* From there also request an API access token and take note of it.

### Set up AWS Lambda

* Go to the Management Console on Lambda, navigate to Functions, and follow the instructions to create a new Lambda function.
* Make sure to set the environment to Python 3.6.
* By default, it should have Cloudwatch and S3 under the access roles on the right. That's fine and quite helpful for debugging.
* Set up an API Gateway trigger on the left hand side.  You can create a new API trigger for this function.  There's some useful information [here](https://docs.aws.amazon.com/apigateway/latest/developerguide/getting-started.html) to guide you through the process if you're not familiar.  The endpoint you create can be arbitrarily named so long as it supports POST.
* In the code area, copy-paste the contents of `handler.py`.
* Under environment variables, you'll need to set the following values:
** `FB_API_TOKEN` - Set this to the api token you requested from Workplace in the previous steps.
** `FB_GROUP_ID` - Set this to the numeric ID of the group you want to post to.
** `HELPSCOUT_KEY` - Set this to a sufficiently long/complicated secret key you want to use to sign Helpscout requests. Take note of it
* Click through on the API Gateway trigger and expand the details to get the URL of the endpoint and take note of it.


## Services

Below are the currently implemented integrations:

* [Helpscout](https://helpscout.net) - https://github.com/physera/helpscout-workplace-lambda
