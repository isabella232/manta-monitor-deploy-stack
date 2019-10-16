## Introduction

The purpose of this tool is to allow the users of [manta-monitor](https://github.com/joyent/manta-monitor)
to quickly deploy the application along with [prometheus](https://prometheus.io/docs/introduction/overview/)
and [grafana](https://grafana.com/docs/guides/getting_started/) in a containerized environment
that makes it easy for dev and functional testing of the manta object store.
The goal is to make it easy for the manta engineers to observe latency metrics
exposed by manta-monitor, in grafana, and quickly test the improvements.

## Pre-requisites
In order to be able to use this tool make sure you have:
* [docker-compose](https://docs.docker.com/compose/install/) installed.
* [Honeybadger](https://github.com/joyent/manta-monitor/blob/master/doc/manta-monitor-deployment.md#honeybadger) api key.
* The tool expects that the MANTA_USER's public and private keys are located at
```
$HOME/.ssh/id_rsa.pub and $HOME/.ssh/id_rsa
```
If this is not the path to the keys then first edit the docker-compose.yml file and
under the *volumes* section of the *manta_monitor* service and change the following lines:
```
- <Complete path of the MANTA_USER's public key>:/opt/manta-monitor/.ssh/id_rsa.pub
- <Complete path of the MANTA_USER's private key>:/opt/manta-monitor/.ssh/id_rsa
```
## Usage

* From the project directory, locate the docker-compose.yml file. Open the file
in the editor of your choice and edit the following fields in the *environment* section
of the *manta_monitor* service:
```
HONEYBADGER_API_KEY=<Use the key provided to you>
MANTA_USER=<The manta user authorized to make request to the manta_url>
MANTA_URL=<The manta endpoint>
```
The environment variable CONFIG_FILE, under the manta_monitor service is used by
the application to configure the workload for manta. By default, it is configured
to test 'buckets', however, changing the 'test_type' key in the ```manta-monitor-config.json```
to 'dir' will configure the application to test manta directories.
* The service *prometheus* is configured with a default configuration file ```prometheus/prometheus.yaml```.
It is configured to scrape the manta-monitor metrics.
* The service *grafana* is configured to pre-install a dashboard that contains
some graphs to help look at some latency metrics right out of the box.

## Start the containers
```
$ docker-compose up -d
```
## Inspect the output
Open grafana UI in the browser http://localhost:3000.
The default userid and password is admin/admin.
![](images/Manta-Monitor-Grafana.png?raw=true)
