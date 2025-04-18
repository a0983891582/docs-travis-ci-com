---
title: Setup Travis CI Enterprise Worker Machine
layout: en_enterprise

---

The Travis CI Enterprise Worker machine manages build containers, and reports build
statuses to the platform. It must be installed on a separate machine
instance from the Platform. We recommend using **compute optimized** instance 
with 8vCPU and 16GB RAM running with Ubuntu 16.04 or later.

## Prerequisites 
1. [Enterprise 3.x](/user/enterprise/tcie-3.x-setting-up-travis-ci-enterprise/#1-setting-up-enterprise-platform) or [Enterprise 2.x](/user/enterprise/setting-up-travis-ci-enterprise/#1-setting-up-enterprise-platform-virtual-machine) Platform is set up
2. You need the *RabbitMQ password* and the *hostname* from the Platform Dashboard.

## Travis CI Worker Setup

1. *On your virtual machine management platform*, create a Travis CI Worker Security Group

    If you're setting up Worker image for the first time, you will need to create
    a Security Group or Firewall rules. From the management console, create an entry for
    each port in the table below:

    | Port | Service | Description |
    |:-----|:--------|:------------|
    | 22   | SSH     | Allow inbound SSH traffic in order to access the Worker Machine from your local machine. |

1. *On your new virtual machine*, download and run the following installation script:

    ```
    $ curl -sSL -o /tmp/installer.sh https://raw.githubusercontent.com/travis-ci/travis-enterprise-worker-installers/master/installer.sh
    $ sudo bash /tmp/installer.sh --travis_enterprise_host="<enterprise host>" --travis_enterprise_security_token="<rabbitmq password>"
    ```

### Install Workers behind a web proxy

If you are behind a web proxy and Docker fails to download the image(s), when you run the worker installation script, edit `/etc/default/docker` and set your proxy there.
Then, rerun the installation script.  

If you need Docker itself to use an HTTP proxy, export it before each docker command:

```
export http_proxy="http://proxy.mycompany.corp:8080/" docker <COMMAND>
```

### Older versions of Travis CI Enterprise

| Travis CI Enterprise Version | Default Worker Version                               | Alternative Worker Versions                          | Worker Status |
|:-----------------------------|:-----------------------------------------------------|:-----------------------------------------------------|:-------------:|
| Enterprise 2.2+              | [Trusty (14.04)](/user/enterprise/trusty/)           | [Precise (Legacy, 12.04)](/user/enterprise/precise/) | Deprecated    |
| Enterprise 2.1.9+            | [Precise (Legacy, 12.04)](/user/enterprise/precise/) | [Trusty (14.04)](/user/enterprise/trusty/)           | Deprecated    |
| Enterprise 2.0+              | [Precise (Legacy, 12.04)](/user/enterprise/precise/) | --                                                   | Deprecated    |

After setting up a new instance for the worker, please follow the respective guides for your Travis CI Enterprise version.

## LXD Worker Setup

1. *On your virtual machine management platform*, create a Travis CI Worker Security Group

    If you're setting up Worker image for the first time, you will need to create
    a Security Group or Firewall rules. From the management console, create an entry for
    each port in the table below:

    | Port | Service | Description |
    |:-----|:--------|:------------|
    | 22   | SSH     | Allow inbound SSH traffic in order to access the Worker Machine from your local machine. |
    
1. *On your new virtual machine*, download and run the following installation script:
 
    ```
    $ curl -sSL -o /tmp/lxd_install.sh https://raw.githubusercontent.com/travis-ci/travis-enterprise-worker-installers/master/lxd/lxd_install.sh
    $ sudo bash /tmp/lxd_install.sh --travis_enterprise_host="<enterprise host>" --travis_enterprise_security_token="<rabbitmq password>" --travis_build_images_arch=”<architecture>”
     ```
Focal images are installed by default; you can change this by providing a `--travis_build_images` parameter.
    
### Advanced Configuration

You can change the LXC storage for instances from the default by using a `--travis_storage_for_instances` flag.
You can change the LXC storage for data from the default by using a `--travis_storate_for_data` flag.

By default, the installer creates an IP4 network for LXC and assigns address 192.168.0.1 to it. The IP6 network is off by default. 
To alter these parameters, use the following installation flags:
 
 |`--travis_network_ipv4_network`|
 |`--travis_network_ipv4_address`|
 |`--travis_network_ipv6_address`|

