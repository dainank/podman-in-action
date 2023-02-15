# Why Podman Over Docker
- allows running of `systemd` within container
- container is child of command
- supports running container in separate namespaces
- runs like traditional command-line tool, thus not needing multiple root running daemons
- supports running multiple containers within the same pod
- supports launching containers/pods based on Kubernetes yaml
- allows configuration of registries for short name expansion
- supports fully customizing all of its defaults
- not all containers need to be stopped on software upgrades

## Rootless Containers
*The ability to run in rootless mode.*
All processes running on the system are owned by the user, thus have no capabilities greater than a normal user.

## Fork/Exec Model
## Daemon-less
*Podman does not need multiple continuously running root daemons.*
## User-Friendly Command Line
*All commands are the same as Docker.*
## Support for REST API
*Remote clients can maange and launch Podman containers.*
## Integration with `systemd`
*Fundamental init system in operating systems.*
- the first process started by kernel on boot
- init can theremore monitor all processes (since it is the ancestor)
Containers essentially function as services in the systemd unit files
## Pods
*“A pod (as in a pod of seals, hence the logo, or pea pod) is a group of one or more containers, with shared storage/network resources, and a specification for how to run the containers.”*
- a single container (like Docker)
- manage a group of containers together (in a pod, unlike Docker)
`podman generate kube`: generates Kubernetes yaml files from running containers/pods
`podman generate play`: generates containers/pods from Kubernetes yaml files
## Customizable Registries
*Can pull images using short names from any registries.*
## Multiple Transports
*Podman supports many different container image sources and targets called transports.*
## Complete Customizability
`/usr/share/containers/containers.conf` - which is where a distribution can define the changes that the distribution likes to use.
`/etc/containers/containers.conf` - where they can set up their system overrides.
`$HOME/.config/containers/containers.conf` - which can be specified only in rootless mode.
## User Namespace Support
*Allows multiple rootless users running containers with multiple uids, all isolated from each other.*