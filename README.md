# proxy
Reverse proxy for drawio-cernbox

Allows to change the production website pointed from CERNBox before an upgrade.

## Prerequisites

A proper role with "edit" permissions in the OpenShift project.

The OpenShift CLI [okd](https://openshift.cern.ch/console/command-line)

## Usage

### Modify the template with the target website

```yml
- apiVersion: v1
  data:
    app-nginx.conf: |
      server {
        listen       8080 default_server;
        server_name  drawio-cernbox.web.cern.ch;

        location / {
          proxy_pass <target website>;
        }
      }
```

### Login to your account
`oc login https://openshift.cern.ch --token=<token>`

### Select the project
`oc project drawio-cernbox`

### Upload the template
`oc create -f nginx.yml`

Note that if some of the objects created by the template already exists during the execution of this command it will fail for that specific object and will not be uploaded.

### Clean the project
To remove all the objects from the project the following command might be executed.

`oc delete all --all`

The ConfigMap should be manually removed. 
