## About this project

This project is used to create the gce-cleaner snap, which is used to cleanup the google instances which are running more than the specified time. 

## Main description

The gce cleaner will run every 15 minutes, list all the running instances and delete the ones which are running more than the timeout specified. 

By default, all the instances will be deleted after 2 hours automatically. In case it is needed an instance more time than that default time, it is going to be required to add a label to the instance with the key: ‘halt-timeout’ and value: ‘Xh’ (replace X with the desired amount of hours). So far the time units supported are: m, h and d.

To setup the label there are 2 alternatives: either go the the google cloud instances web page and set the label manually, or by using the CLI as in the following example:

> gcloud compute instances add-labels [Instance-name] --zone=us-east1-b --labels=“halt-timeout=4h”

## Configuration

It is possible to configure some parameters for the snap by using snapctl:

* zone = the gce zone where the instances are allocated (default: us-east1-b)
* project = the gce project (default: computeengine)
* timeout = the timeout used to delete the instances measured in minutes (default: 120)
* haltkey = the key used to determine the timeout for the specific instance (default: halt-timeout)
* credentials = the path to the google credentials used to list/remove instances (default: $SNAP_COMMON/application_default_credentials.json)

## Deploy with Spread

There is a spread task to make the automatic deployment of the gce-cleaner into a google instance.

Before running the spread task, it is required to have in the project path a file called sa.json with the appropiated service account credentials. Those credentials will be used to connect the snap to the gce project.

Use the following command to run the spread task:

> spread -reuse google:ubuntu-16.04-64:tasks/deploy
