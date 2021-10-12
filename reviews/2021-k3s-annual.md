# K3s 2021 Annual Review

## Table of Contents

- [Background](#background)
- [Alignment with Cloud Native Computing Foundation](#alignment-with-cloud-native-computing-foundation)
- [Year in Review](#year-in-review)
- [Annual Review Contents](#annual-review-contents)
- [Project Links](#project-links)

## Background

K3s is a Lightweight Kubernetes distribution that ships all needed components and utilities,
sans images, embedded in a single binary that weighs in at under 60mb (with compressed air-gap images adding ~150mb).

K3s is a [fully conformant](https://github.com/cncf/k8s-conformance/pulls?q=is%3Apr+k3s) production-ready distribution with some [additional flavor](https://knowyourmeme.com/memes/salt-bae):

1. Packaged as a single binary.
2. Support for sqlite3 as the default storage backend. Etcd3, MySQL, and Postgres are also supported.
3. Wraps Kubernetes and other components in a single, simple launcher.
4. Secure by default with reasonable defaults for lightweight environments.
5. Minimal to no OS dependencies (just a sane kernel and cgroup mounts needed).
6. Eliminates the need to expose a port on Kubernetes worker nodes for the kubelet API by exposing this API to the Kubernetes control plane nodes over a websocket tunnel.
7. Internet installation via [https://get.k3s.io](https://get.k3s.io "Y.O.L.O.")

K3s bundles the following technologies together into a single cohesive distribution:

* [runc](https://github.com/opencontainers/runc) as the default OCI runtime
* [Containerd](https://containerd.io/) as the default CRI
* [Flannel](https://github.com/coreos/flannel) as the default CNI
* [CoreDNS](https://coredns.io/) as kube-dns
* [Metrics Server](https://github.com/kubernetes-sigs/metrics-server) exposing metrics
* [Traefik](https://containo.us/traefik/) for ingress
* [Klipper-lb](https://github.com/k3s-io/klipper-lb) as an embedded service loadbalancer provider
* [Kube-router](https://www.kube-router.io/) for network policy
* [Helm-controller](https://github.com/k3s-io/helm-controller) to allow for CRD-driven deployment of helm manifests
* [Kine](https://github.com/k3s-io/kine) as a datastore shim that allows etcd to be replaced with other databases
* [Local-path-provisioner](https://github.com/rancher/local-path-provisioner) for provisioning volumes using local storage
* [Host utilities](https://github.com/k3s-io/k3s-root) such as iptables/nftables, ebtables, ethtool, & socat

Additionally, K3s simplifies Kubernetes operations by maintaining functionality for:

* Managing the TLS certificates of Kubernetes componenents
* Managing the connection between worker and server nodes
* Drop-in manifest apply for all built-in and custom resources
* Managing an embedded etcd cluster

### Accepted into CNCF Sandbox

**K3s was accepted as a CNCF Sandbox project on August 19, 2020**

- Sandbox proposal [pr #447](https://github.com/cncf/toc/pull/447)
- Sandbox proposal [doc](https://github.com/cncf/toc/blob/main/proposals/sandbox/k3s.md)

## Alignment with Cloud Native Computing Foundation

K3s falls in the scope of [CNCF Runtime TAG](https://github.com/cncf/tag-runtime).

### K3s Focus

- Delivering a light-weight, easy-to-get-up-and-running Kubernetes installation and management experience.
- 

### K3s Ecosystem

#### Upstream Contributions
The K3s maintainers have contributed fixes and features to the following upstream projects:
* [container-selinux](https://github.com/containers/container-selinux)
* [containerd](https://github.com/containerd/containerd)
* [etcd](https://github.com/etcd-io/etcd)
* [flannel](https://github.com/flannel-io/flannel)

#### Collaborations and Integrations
In the following, we provide a list of **ecosystem projects** that have collaborated or integrated with K3s:
* [k3d](https://github.com/rancher/k3d)
* [k3s-ansible](https://github.com/k3s-io/k3s-ansible)
* [k3s-selinux](https://github.com/k3s-io/k3s-selinux)
* [k3sup](https://github.com/alexellis/k3sup)
* [kine](https://github.com/k3s-io/kine)
* [klipper-helm](https://github.com/k3s-io/klipper-helm)
* [klipper-lb](https://github.com/k3s-io/klipper-lb)

## Year in Review

**Key features added**:

- etcd snapshots
- etcd member management
- etcd backup and restore
- automatic discovery of nvidia container runtime
- containerd v1.5.x
- traefik v2.x
- selinux
- 

**K3s community and adoption growth** in the last year:

| Statistic | Sandbox | Current |
|-|-|-|
| [GitHub Stars](https://github.com/k3s-io/k3s/stargazers) | ~14k | ~18k |
| [Commits](https://github.com/k3s-io/k3s/commits/master) | ~75/month | ~65/month |
| [Merged PRs](https://github.com/k3s-io/k3s/pulls) | ~35/month | ~45/month |
| [Releases](https://github.com/k3s-io/k3s/releases) | 100 | 171 |

## Annual Review Contents

### Include a link to your project’s devstats page. We will be looking for signs of consistent or increasing contribution activity. Please feel free to add commentary to add colour to the numbers and graphs we will see on devstats.

- The main K3s project averages [~730 contributions from ~120 contributors](https://k3s.devstats.cncf.io/d/74/contributions-chart?orgId=1&from=1598943600000&to=1633071599000&var-period=m&var-metric=contributions&var-repogroup_name=K3s) on average per month for the last year.

### How many maintainers do you have, and which organizations are they from? (Feel free to link to an existing MAINTAINERS file if appropriate.)

Our maintainers list: https://github.com/k3s-io/k3s/blob/master/MAINTAINERS

Currently, the project has 11 maintainers, all with SUSE:
- [Brad Davidson](https://github.com/brandond "@brandond")
- [Brian Downs](https://github.com/briandowns "@briandowns")
- [Craig Jellick](https://github.com/cjellick "@cjellick")
- [Chris Wayne](https://github.com/cwayne18 "@cwayne18")
- [Derek Nola](https://github.com/dereknola "@dereknola")
- [Jacob Blain Christen](https://github.com/dweomer "@dweomer")
- [Hussein Galal](https://github.com/galal-hussein "@galal-hussein")
- [Darren Shepherd](https://github.com/ibuildthecloud "@ibuildthecloud")
- [Manuel Buil](https://github.com/manuelbuil "@manuelbuil")
- [Chris Kim](https://github.com/Oats87 "@Oats87")
- [Michal Rostecki](https://github.com/vadorovsky "@vadorovsky")

### What do you know about adoption, and how has this changed since your last review / since you joined Sandbox? If you can list companies that are end users of your project, please do so. (Feel free to link to an existing ADOPTERS file if appropriate.)

Some adoption highlights from the last year are:

1. [Civo Kubernetes](https://www.civo.com/kubernetes)
2. [Northflank](https://northflank.com/)

### How has the project performed against its goals since the last review? (We won't penalize you if your goals changed for good reasons.)

- [x] SELinux Policy
- [ ] Dual Stack Networking


### What are the current goals of the project? For example, are you working on major new features? Or are you concentrating on adoption or documentation?

- Delivering patch releases that lag mere days behind upstream (with CVEs ideally same day or day after).
- Delivering minor releases that lag at most 1-2 weeks behind upstream.
- Better engagement with the community via [Github Discussions](https://github.com/k3s-io/k3s/discussions)
- Better engagement with upstream(s) via contributions (PR and KEP)
- 

### How can the CNCF help you achieve your upcoming goals?

- Continued promotion via blogs and webinars
- License review(s)
- CI resource allocation (building and validating a k8s distribution is resource-intensive)
- 

### Do you think that your project meets the [criteria for incubation](https://github.com/cncf/toc/blob/master/process/graduation_criteria.adoc#incubating-stage)?

- Yes, proposal to follow.

## Project Links
 - [web://k3s.io](https://k3s.io)
 - [github://k3s-io/k3s](https://github.com/k3s-io/k3s)
 - [slack://rancher-users#k3s](https://rancher-users.slack.com/archives/CGGQEHPPW)
