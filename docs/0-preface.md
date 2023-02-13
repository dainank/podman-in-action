# Preface
## Context
- **containers** were originally called **sandboxes**
- **Docker** has some problems in its design:
    - running a centralized daemon as root
- better ways of running containers consist of taking advantage of more core concepts of the operating system

## Podman
- CLI and API had to match the Docker CLI and API
    - extend these technologies
- secure the containers with everything the operating system provided
    - the **CoreOS** takes advantage of all of the features of the operating system in order to isolate containerized applications from one another
        - this provides increased security
        - this creates resource constraints:
            - thus applications become convinced they are running on dedicated system
