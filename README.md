# ChirpStack Ansible Playbook

This repository provides an [Ansible](https://www.ansible.com) playbook to
setup the [ChirpStack](https://www.chirpstack.io/) open-source LoRaWAN Network Server (v4)
on a single VM (e.g. on DigitalOcean). 

It will:

* Setup firewall rules (iptables)
* Setup Mosquitto (MQTT broker) + client and server-certificate configuration
* Setup Redis
* Setup PostgreSQL + creation of role and database
* Setup [ChirpStack Gateway Bridge](https://www.chirpstack.io/docs/chirpstack-gateway-bridge/) for UDP
* Setup [ChirpStack Gateway Bridge](https://www.chirpstack.io/docs/chirpstack-gateway-bridge/) for Basics Station
* Setup [ChirpStack](https://www.chirpstack.io/docs/chirpstack/)
* Request a HTTPS certificate from [Let's Encrypt](https://letsencrypt.org)

## Deploying ChirpStack

This playbook has been tested on 
[DigitalOcean.com](https://m.do.co/c/6cd86e9f1cb8) but should also work on
bare-metal, AWS and other cloud-providers.

Don't have a DigitalOcean account yet? Use
[this](https://m.do.co/c/6cd86e9f1cb8) link and get $10 in credits for free :-)

### Ports

* `443`: ChirpStack UI and gRPC API (with TLS, e.g. https://subdomain.example.com/)
* `1700`: ChirpStack Gateway Bridge UDP listener (configured for EU868 region by default)
* `3001`: ChirpStack Gateway Bridge Basics Station listener (configured for EU868 region by default, with TLS, client-certificate files can be generated in the ChirpStack UI)
* `8883`: Mosquitto MQTT (with TLS, client-certificate files can be generated in the ChirpStack UI)

### Requirements

On the machine from where you will execute this Ansible playbook (e.g. your own
computer), make sure you have a recent Ansible version installed. You can install Ansible with
pip (`pip install ansible`), using Homebrew (OS X) (`brew install ansible`) or using
a package manager (e.g. `apt install ansible`). Refer to the [Ansible installation guide](http://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)
for more installation instructions.

The Ansible playbook has been tested on the following images:

* Debian 13
* Ubuntu 26.04 (LTS)

### Configuration

1. Create a new Debian or Ubuntu instance and make sure that from your own machine
   on which Ansible is installed, you can ssh to this machine using public-key
   authentication (e.g. `ssh user@ip`).

2. Configure a DNS record for your target instance and wait until this record
   resolves to your IP address.

3. Copy the `inventory.example` inside this repository to `inventory` and
   replace `example.com` with the hostname created in step 2.

4. Copy the `group_vars/chirpstack.example.yml` inside this repository to
   `group_vars/chirpstack.yml` and change the settings where needed.

For more information, see also:

* https://www.chirpstack.io/docs/

### Provisioning

Run the following command from your machine to deploy ChirpStack to your
target instance, to upgrade to the latest versions or to update the
configuration:

```bash
ansible-playbook -i inventory deploy.yml
```

After the playbook has been completed, ChirpStack should be accessible from
the domain you configured as fqdn in the `group_vars/chirpstack.yml`.
