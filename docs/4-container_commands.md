# Working with Containers
## Exploring Containers
1. `podman run` - reaches out to registry, pulls image and stories it locally in home directory
`$ podman run -ti --rm registry.access.redhat.com/ubi8/httpd-24 bash`
    - bash prompt running within container
    - `-ti` flags tell Podman to hook up to terminal (allows interaction with container)
    - `-rm` delete container as soon as you exit (freeing up storage)
    - specify executable to run, in this case, `bash`
    - `cat /etc/os-release` useful for finding operating system information
    - `ls /usr/bin | wc -l` see number of commands available
    - `ps` view processes running inside of container

We can run the intended image like so: `podman run -d -p 8080:8080 --name myapp registry.access.redhat.com/ubi8/httpd-24`
- `-d` - launch container and then detach from it (running in background)
- `-p` - bind a port to container (use `podman port [container]` to see overview)
- close the container with `podman stop myapp`

Multiple web servers can be created as simple as this:
`podman run -d -p 8081:8080 --name myapp1 registry.access.redhat.com/ubi8/httpd-24`

`podman create` - can be used to pull and configure an image but not run it

### Key Podman Commands
- `--user USERNAME` Tells Podman to run the container as a specific user defined in the image. By default Podman will run the container as root, unless the container image specifies a default user.
- `--rm` automatically remove the container when it exits
- `--tty` -(t) allocates a pseudo-tty and attaches it to the standard input of the container.
- `--interactive` (`-i`) connects stdin to the primary process of the container. These options give you an interactive shell within the container.

#### Podman Stops
- `podman stop -t 0 myapp1` immediately kill container
    - `--timeout` (-t) sets the timeout, -t 0 sends the SIGKILL without waiting for the container to stop.
    - `--latest` (-l) is a useful option to allow you to stop the last created container rather than having to use the container name or container id. Most Podman commands that require you to specify a container name or id, also accept the --latest option. Only available on Linux machines.
    - `--all` tells Podman to stop all running containers. Similarly to --latest, Podman commands which require a container name or container id parameter, also take the --all option.

#### Podman Start
`podman start myapp`
- `--all` starts all of the stopped containers in container storage.
- `--attach` attaches your terminal to the output of the container.
- `--interactive` (-i) attaches the terminal input to the container.

#### List Containers
`podman ps` - running
`podman ps -all` - all

- `--quiet` tells Podman to only print the container ids
- -`-size` tells Podman to return the amount of disk space currently used for each container other than the images they are based on.

#### Inspecting Containers
`podman inspect myapp`
`podman inspect --format '{{ .Config.Cmd }}' myapp` - see initial command on container start
`podman inspect --format '{{ .Config.StopSignal }}' myapp`-  see stop signal

- `--latest` (-l) is handy in that it allows you to quickly inspect the last created container rather than specifying the container name or container id.
- `--format` is useful as you see above to extract particular fields out of the json.
- `--size` adds the amount of disk space the container is using. Gathering this information takes a long time, so it is not done by default.

#### Removing Containers
`podman rm myapp1`
- `--all` option is useful if you want to remove all of your containers
- `--force` option tells Podman to stop all of the running containers when removing.

#### Execute in Running Container
Examples:
```sh
podman exec -i myapp bash -c 'cat > /var/www/html/index.html' << _EOF
<html>
 <head>
 </head>
 <body>
  <h1>Hello World</h1>
 </body>
</html>
_EOF
```

`podman exec myapp cat /var/www/html/index.html`
- `--tty` connects a tty to the exec session
- `--interactive`,`-i` option tells Podman to run in interactive mode, meaning you can interact with an exec'd program like a shell.

#### Create Image from Container
> Must stop the running container before doing this.

- `podman commit myapp myimage` - create image from containar
- `podman run -d --name myapp1 -p 8080:8080 myimage` - create new container with created image

- `--pause` pauses a running container during the commit. Notice I stopped the container before doing the commit. I could have simply paused it. The podman pause and podman unpause commands allow you to pause and unpause containers directly.
- `--change` option allows you to commit instructions on how to use the image. The instructions are CMD, ENTRYPOINT, ENV, EXPOSE, LABEL, ONBUILD, STOPSIGNAL, USER, VOLUME, WORKDIR. These instructions match up with the directives in the Containerfile or Dockerfile.
