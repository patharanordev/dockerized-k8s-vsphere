# **Dockerized K8s vSphere**

To connect to VMWare Tanzu server for CI/CD.

## **Dockerized**

> Before start, don't forget connect to target VPN or `Zscaler` first. 

Firstly, you must download executable file from `https://${VSPHERE_IP}/wcp/plugin/darwin-amd64/vsphere-plugin.zip`

**note** : `VSPHERE_IP` is our target server.

Extract `vsphere-plugin.zip` then 

 - Copy every files in `vsphere-plugin/bin/` to the same directory with `Dockerfile`.
 - Set file permission to `755` (allowed public execute but not allow write if not owner) :

    ```sh
    -rwxr-xr-x@ 1 USER_NAME  SOMETHING  46424064 MMM DD  YYYY kubectl
    -rwxr-xr-x@ 1 USER_NAME  SOMETHING  11169175 MMM DD  YYYY kubectl-vsphere
    ```

 - Your `Dockerfile` should looks like this :

    ```docker
    # syntax=docker/dockerfile:1
    FROM ubuntu:18.04

    WORKDIR /app

    COPY . .

    RUN mkdir -p /usr/local/bin
    RUN mv /app/kubectl /usr/local/bin/kubectl && \
        mv /app/kubectl-vsphere /usr/local/bin/kubectl-vsphere

    CMD [ "kubectl", "vsphere", "--help" ]
    ```

## **Usage**

Build container image :

```sh
$ docker build -t k8svsphere:latest .
```

Run :

```sh
# access to container
$ docker run --rm -it --privileged k8svsphere:latest /bin/sh

# using command only
$ docker run --rm -it --privileged k8svsphere:latest kubectl vsphere --help
```

## **References**

> Installing Build Kit CLI for kubectl :
> 
> Ref. https://github.com/vmware-tanzu/buildkit-cli-for-kubectl/blob/main/docs/installing.md
