# Prometheus + Grafana = Awesome Monitoring
### Talk presentend at DevOps Days Florianopolis - 18/02/2017

Check out the presentation slides here: [Prometheus + Grafana = Awesome Monitoring - Slide Share](https://www.slideshare.net/henriquedalssaso/prometheus-grafana-awesome-monitoring)

This is the repository that you'll find all the files used in the demo presented!

Dependencies:
* Vagrant >= 1.8
* Unix/Linux Machine
* vagrant-hostmanager (Vagrant Plugin)
* vagrant-vbguest (Vagrant Plugin)

Checkout this docs to install vagrant and vagrant plugins:
* [Vagrant Installation](https://www.vagrantup.com/docs/installation/)
* [Vagrant Plugins](https://www.vagrantup.com/docs/plugins/usage.html)

All the Prometheus and Grafana setup runs inside of the Vagrant so you can replicate easily on any machine.

Once you got everything needed you just need to clone the repository and enter into the directory
```bash
  git clone https://github.com/dalssaso/prometheus-grafana-devops-floripa.git
  cd prometheus-grafana-devops-floripa
```

After that you need to start vagrant vms

```bash
  vagrant up --provisioning
```

It will take a while to spin up, this will start the VMs and install everything you need for running the steps, including:
* Docker
* Docker Compose
* Share the files from the host machine and the vagrant machines
* Set the hostnames and communication between them

You can see all the steps in the [`Vagrantfile`](Vagrantfile)

## Prometheus Server Setup
After you get everything up and running you need to connect into the vagrant machine called `prom-server`
```bash
  vagrant ssh prom-server
```
Here comes the good part :)

All the commands that we'll need to run, we'll run inside of `/vagrant` directory

* Let's start the prometheus server

```bash
  docker-compose up -d prometheus
```

* After that we'll need to start the alertmanager

```bash
  docker-compose up -d alertmanager
```

* After that we'll start the grafana

```bash
  docker-compose up -d grafana
```

Now we're good to go to our browse and check the grafana and prometheus working

Open your browser and point to: `http://prom-server.example.com:3000` to check grafana

Open your browser and point to: `http://prom-server.example.com:9090` to check prometheus dashboard


Now to start getting some metrics we need to start the nodeexporter

```bash
  docker-compose up -d nodeexporter
```

Open a new terminal session and go to the repository folder and run:

```bash
  vagrant ssh node
  cd /vagrant
  docker-compose up -d nodeexporter
```

You'll start getting metrics from to servers!

## Grafana Dashboard Setup

### Prometheus DataSource

Open Grafana Dashboard on your browser (`http://prom-server.example.com:3000`)
> The default user and pass is: admin

Click in the `Grafana Icon at Left Top Corner -> Data Source -> Add Data Source`

| Name   | Value                 |
|--------|-----------------------|
| Name   | Prometheus            |
| Type   | Prometheus            |
| URL    | http://localhost:9090 |
| Access | proxy                 |

Fill the form with the values above and then click `Add`


### Node Explorer Dashboard

Click in the `Home -> Import -> Upload .json file` select the `node_grafana_template.json`

Select the Prometheus Datasource that we just added
Click and Add and you're good to go

You have all set! Now you just need to play around :D

# Checking the configuration files

* [`alert.rules`](alert.rules) - configuration file when we configure the rules to the alerts
* [`alertmanager.yml`](alertmanager.yml) - configuration file for the alertmanager service
* [`prometheus.yml`](prometheus.yml) - configuration file for the prometheus server service

For further learning of Prometheus check the its docs [here](https://prometheus.io/docs/introduction/overview/)

# Destroy the environment

```bash
  cd prometheus-grafana-devops-floripa
  vagrant destroy -f
```
