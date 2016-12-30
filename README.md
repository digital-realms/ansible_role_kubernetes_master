Kubernetes Master
=========
Master:

[![Build Status](https://travis-ci.org/digital-realms/ansible_role_kubernetes_master.svg?branch=master)](https://travis-ci.org/digital-realms/ansible_role_kubernetes_master)

This role installs a Kubernetes Master node (standalone) using the new `kubeadm` utility that Kubernetes provides.

Networking: Currently this module installs weave, however, further network modules will be added. If there is one that you need and is not listed, feel free to open an Issue and it will be looked into. If you're feeling kind, generous and contributing, feel free to fork, commit and PR :) .

This role also exposes as a fact the cluster token which in turn can be consumed by any other role which will be configuring the Kubernetes Minnions.. such as: <https://galaxy.ansible.com/digital-realms/kubernetes-minnion/> (bit of self advertising never hurt I guess :) )

Requirements
------------

The master node has the following Requirements:

1. Running CentOS 7 (or any other equivalent from the RHEL family should be just as fine)

2. A bare minimum of 1GB RAM, ideally a minimum of 1.5GB

3. Full network connectivity with any other box that will be acting as the minnion(s) of this master.

**NOTE:** Last but not least, please do remember to run this role with `become: yes` to grant privileged access during the installation process.

Role Variables
--------------

The master has the following variables which will are available to be consumed:

1. `k8s_network_selection:` - This variable has a default value to use `weave`, however, may be overwritten should you wish to use any other networking module. Currently its the **only** module supported. as mentioned above, if you need others, feel free to open an issue and contribute if you're feeling kind and generous :)

2. `k8s_init_xtrargs:` - This allows us to pass in any extra arguments that you might want to pass to the `init` phase of the installation. this is particularly useful should you wish to pass parameters such as:
  * `--api-advertise-addresses=<ip-address>`
  * `--pod-network-cidr=10.244.0.0/16`
  * `--api-external-dns-names=kubernetes.example.com,kube.example.com`.

  It's default is an empty string, which means that it will be bypassed.

3. `k8s_taint_mode` - By default, your cluster will not schedule pods on the master for security reasons. If you want to be able to schedule pods on the master, for example if you want a single-machine Kubernetes cluster for development set this variable to `true`.
                      You can find more information about it [here](http://kubernetes.io/docs/getting-started-guides/kubeadm/) under the `Initializing your master` section

4. `install_docker` - By default this role will install docker for you, if you would like to mange docker yourself, just set this variable to `false` and the role will skip the installation.


Dependencies
------------

The main dependency for this role pertains to the `become:yes` when invoking from a Playbook. This (as explained before, I know) will allow this role to gain privileged access during the installation phase.

Example Playbook
----------------

A line of code speaks a thousand words.

Sample basic profile, invoking the role and using `become: yes` for elevated privileges

    - hosts: k8s-master
      roles:
         - { role: digital-realms.kubernetes-master, become: yes }

Should you wish to be a little bit more adventurous and would like to pass in further parameters, something of the sort would work:

    - hosts: k8s-master
      roles:
         - { role: digital-realms.kubernetes-master, become: yes, k8s_init_xtrargs: '--api-advertise-addresses=10.0.0.1' }

License
-------

[MIT](LICENSE)

Author Information
------------------

This role has been written by Digital Realms. feel free to contact <info@digital-realms.net> or visit <http://www.digital-realms.net>. The authors who contributed to this role are (in no particular order):
* Marco Lenzo
* Steve Azzopardi
* Edward Midolo
