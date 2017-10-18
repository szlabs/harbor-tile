# harbor-tile
Project to build the tile for Harbor based on Harbor bosh release.

**NOTES: Tile for Harbor is still under development. It's very unstable and may be not usable some time.**

## Package
[Tile](https://docs.pivotal.io/tiledev/tile-structure.html) in this project is built based on the [Harbor](https://github.com/vmware/harbor) [bosh](https://bosh.io) [release](https://github.com/steven-zou/harbor-bosh-release) created before.

```
packages:
- name: harbor
  type: bosh-release
  path: resources/harbor-bosh-release_0.1.3+dev.01.tgz
```

## Setup DEV environment
### Tile Generator
Pivotal recommends using [virtualenv](https://virtualenv.pypa.io/en/stable/) to setup the development environment. Install the **tile-generator** in the virtual environment created by virtualenv.
```
//create env
virtualenv -p /usr/local/python tile-generator-env

//activate env
source tile-generator-env/bin/activate

//install tile-generator
pip install tile-generator
```

### Pivotal Ops Manager
If you want to build you own ops manager environment to validate the tile you build, you can follow the document below to deploy Ops Manager on vSphere (For other IaaS platform, please refer the Povital document).

* [Deploying Operations Manager to vSphere](http://docs.pivotal.io/pivotalcf/1-12/customizing/deploying-vm.html)
* [Configuring Ops Manager Director for VMware vSphere](http://docs.pivotal.io/pivotalcf/1-12/customizing/vsphere-config.html)

## Build and Test
Use the following command to initialize tile project if not.
```
cd YOUR-PROD-DIRECTORY
tile init

```

Edit the generated **tile.yml** file to define your tile.
```
---
# The high-level description of your tile.
# Replace these properties with real values.
#
name: harbor-tile # By convention lowercase with dashes
icon_file: resources/harbor.png
label: Harbor
description: Project Harbor is an enterprise-class registry server that stores and distributes Docker images. Harbor extends the open source Docker Distribution by adding the functionalities usually required by an enterprise, such as security, identity and management.

packages:
- name: harbor
  type: bosh-release
  path: resources/harbor-bosh-release_0.1.3+dev.01.tgz
  jobs:
  - name: harbor-app
    templates:
    - name: harbor
      release: harbor-bosh-release
    cpu: 1
    memory: 4096
    ephemeral_disk: 10240
    persistent_disk: 20480
    instances: 1
    static_ip: 0
    dynamic_ip: 1
    default_internet_connected: true
    max_in_flight: 1
```

Build the tile.
```

tile build [version]

```

You can import the generated tile file which is located in the **product** folder into the ops manager to try the product deployment.
![import product](resources/ops-manager.png)

## Example
Here is a sample provided by Pivotal team for your reference.[sample](https://github.com/cf-platform-eng/tile-generator/tree/master/sample)