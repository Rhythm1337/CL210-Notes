**Date:** 26 May 2026

## OpenStack Personas
**Cloud Persona:** It is the person who is the admin of the control plane, day to day operations include troubleshooting nova, neutron, cinder etc. Manages global resources, eliminates repetitive tasks, interacts with Ansible and **OpenStack Director**.
They also support the following personas
* Domain Operator
* Automation Engineer / Devops Engineer
* Infra Architect

### In Labs
* **DNS Zone** set in `/etc/hosts`
* **IdM:** Red Hat has their own IdM instead of an LDAP server.
* **Virtual Power Management:** In labs we use `fake power`.
* **ssh @ power**
* `cat /etc/bmc/vms`
* In real world, there is a separate setting on motherboard connected to diff network.

---

## OpenStack Roles / Architecture Diagram Transcriptions

### Concentric Layers (From Outer to Inner)
1. **Application** (Application Developer / Application Architect / Dev Ops / Security Operator)
2. **Project Owner**
3. **Domain Operator** (CL110)
4. **Cloud Operator** (CL210 / Automation Engineer)
5. **Design Architect** (Undercloud / Overcloud / Cloud Admin / OpenStack Cloud Infra and Infra Services Admin)
6. **Hardware Center Operator** (Interacts with hardware, server provisioning, monitoring)


## Containerized Services

OpenStack Platform runs most of the major services as containers. These services are isolated.
Container images are pulled from **Red Hat Container catalog** or **local satellite server** or **Tripleo deployment** (recommended) which can be created locally on the under cloud node.

## Container Commands

```
podman ps

# To get service info 
podman ps -a --format="table {{.Names}} {{.Status}}" | grep heat

# To filter
podmam ps --filter status=running --format="table {{.ID}} {{.Names}} {{.Status}}"

# To display status
podman status octavia_worker

# To display images
podman images

# To inspect
podman inspect octavia_worker
podman inspect octavia_worker | jk .[0].xxx
podman inspect --format"{{.HostPath}}" 547u9783234e

# To gather Logs
podman logs octavia_worker
podman logs --since 3h --tail 4 octavia_worker

# To access container
podman exec -it octavia_worker /bin/bash [This brings you inside the container with bash shell]
podman exec octavia_worker hostname [This only executes commands inside the container and sends the output directly]
```

## Systemd Services

```
systemctl status service_name
less /etc/systemd/system/service_name

systemctl stop service_name
systemctl start service_name
systemctl restart service_name
systemctl status service_name
```

There is no container monitoring status so systemd restarts containers when:
* exit signal
* unclear exit system signal like if a container crashes after starting
* timeout reached when a container takes more than 1 minute 30 seconds to start

```
systemctl list-timers | grep tripleo
```

## Logs

Logs are stored in /var/log/containers/
* stdout
* stderr

## Config Files

These are configuration files for the containers, they live in **/var/lib/config-data/puppet-generated/**
Containarized services are named with a tripleo_ prefix because they are installed by it.
```
ls /etc/systemd/system/tripleo_*.service
/etc/systemd/system/tripleo_aodh_api_healthcheck.service
/etc/systemd/system/tripleo_aodh_evaluator_healthcheck.service**
```

# Undercloud

To run overcloud you need undercloud. This is a mini self contained openstack on just 1 server.
Undercloud uses a toolset called TripleO which is basically **Openstack on Openstack** lmao.

1. **Keystone: (Identity Service)** Identity service used for auth for undercloud's openstacks services
2. **Glance: (Image Service)** Image service that stores initial images that can be deployed to bare metal. They contain RHEL, KVM hypervison and container runtimes
3. **Ironic: (Bare Metal Service)** This provisions physical machines
4. **Nova: (Compute Service)** This works with Bare Metal Service's to provision nodes by taking the inventory of all the avaliable systems and functional nodes where we can deploy a machine
5. **Heat: (Orchestration Service)** This provides set of yaml templates and roles to define config and instructions to provision overcloud deployments.
6. **Object Service: (Swift)** This holds images, deployment logs and all
7. **Neutron: (Networking Service)** This is the service that configures interfaces for external (public access) and provisioning (DHCP and PXE boot functions) networks.
