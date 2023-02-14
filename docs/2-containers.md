# Containers
*Groups of processes running on a Linux system, which are isolated from each other.*
- ensures that one group of processes does not interfere with other processes on the system
- rogue processes won't dominate system resources preventing other processes from performing their task
- hostile containers can't attack other containers, stealing data or causing denial of service attacks
- allow applications to be installed with their own versions of shared libraries that do not conflict with applications requiring different versions of the same libraries
- allow applications to live in a virtualized environment, with a feel that they own the entire system.

## Resource Constraints
Containers are primarily isolated via resource constraints (**cgroups**): https://man7.org/linux/man-pages/man7/cgroups.7.html

> Control groups, usually referred to as cgroups, are a Linux kernel feature which allow processes to be organized into hierarchical groups whose usage of various types of resources can then be limited and monitored.

Resources such as:
- memory that a group of processes can use
- CPU that a group of processes can use
- network resources that a process can use

> The basic idea of cgroups is to control one group of processes from dominating certain system resources in such a way that another group of processes can’t make progress on the system.

## Security Constraints
Containers can be isolated from each other using many security tools available in the kernel.

> The idea is to block privilege escalation, and prevent a rogue group of processes from hostile acts against the system.

## Virtualization Technologies (namespaces)
Namespaces create virtualized environments where one set of processes sees one set of resources while another set of processes sees a different set of resources.

> Essentially, this gives processes the feel of a virtual machine, without the overhead.

# Container Image Format
Three components:
- dir (`rootfs`) tree containing all software required to run application
    - software layed out like it is root of system
- JSON file describing contents of `rootfs` (can be viewed with `podman inspect` command):
    - exe to be ran
    - working dir
    - env var used
    - maintainer of exe
    - other labels to identify content of the image
- manifest list (JSON file) that links multiple images together to support different architectures.
    - thus different architectures can be supported by the same image names (with the correct arch being served)
    - `Skopeo` command can be used to view more details and better understand manifest list files
        - `skopeo inspect --raw`

> Container registries store the images on web servers (docker.io, quay.io e.t.c.)

> Container engines (Podman) can copy these images to host and unpack them into file system.

1. unpack images into file system
2. engine merges image's JSON file with engine's built-in defaults + user input; creating new container JSON file (OCI runtime specification)
3. engine launches container runetime program
4. container runetime reads the container JSON, launching the primary process of the container

# Container Standards
