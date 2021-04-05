Red Hat Process Automation Manager / Decision Manager Workshop Module 2
===
This is `module 2` of a full-day hands-on Red Hat Process Automation Manager and Red Hat Decision Manager workshop. It provides developers and/or business analysts an introduction to Rules- and Process Driven applications and microservices in the context of modern, cloud-native, architectures.

Agenda
===
* Use Case Overview
* Business Object Model
* Decisions Authoring
* Decisions as a Service
* Testing a Decision Service
* Executing a Decision Service

Give a look at: https://red.ht/pam-dm-workshop-module2


Run locally
=== 

```
$ git clone https://github.com/RedHat-Middleware-Workshops/rhpam-rhdm-workshop-v1m2-guides.git
$ cd rhpam-rhdm-workshop-v1m2-guides
$ docker run -it --rm -p 8080:8080 -v $(pwd):/app-data -e CONTENT_URL_PREFIX="file:///app-data" -e WORKSHOPS_URLS="file:///app-data/_rhpam-rhdm-workshop-module2.yml" -e LOG_TO_STDOUT=true quay.io/osevg/workshopper 
```

Lab Instructions on OpenShift
===

Note that if you have installed the lab infra via APB, the lab instructions are already deployed.

*Option 1:*
Login to your OCP Cluster and run:

````
oc create -f support/ocp-provisioning.yaml
````

*Option 2:* 

Here is an example Ansible playbook to deploy the lab instruction to your OpenShift cluster manually.
```
- name: Create Guides Module 2
  hosts: localhost
  tasks:
  - import_role:
      name: siamaksade.openshift_workshopper
    vars:
      project_name: "guide-m2"
      workshopper_name: "RHPAM / RHDM Workshop V1 Module-2"
      project_suffix: "-XX"
      workshopper_content_url_prefix: https://raw.githubusercontent.com/RedHat-Middleware-Workshops/rhpam-rhdm-workshop-v1m2-guides/master
      workshopper_workshop_urls: https://raw.githubusercontent.com/RedHat-Middleware-Workshops/rhpam-rhdm-workshop-v1m2-guides/master/_rhpam-rhdm-workshop-module2.yml
      workshopper_env_vars:
        PROJECT_SUFFIX: "-XX"
        COOLSTORE_PROJECT: coolstore{{ project_suffix }}
        OPENSHIFT_CONSOLE_URL: "https://master.seoul-2922.openshiftworkshop.com"
        ECLIPSE_CHE_URL: "http://che-labs-infra.apps.seoul-2922.openshiftworkshop.com"
        GIT_URL: "http://gogs-labs-infra.apps.seoul-2922.openshiftworkshop.com"
        NEXUS_URL: "http://nexus-labs-infra.apps.seoul-2922.openshiftworkshop.com"
        LABS_DOWNLOAD_URL: "http://gogs-labs-infra.apps.seoul-2922.openshiftworkshop.com"

      openshift_cli: "/Users/doh/cloud-native-app-dev/oc --server https://master.seoul-2922.openshiftworkshop.com"
```
