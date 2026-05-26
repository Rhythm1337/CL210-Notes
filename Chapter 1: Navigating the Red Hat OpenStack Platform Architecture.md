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




