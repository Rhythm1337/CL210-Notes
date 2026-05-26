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
