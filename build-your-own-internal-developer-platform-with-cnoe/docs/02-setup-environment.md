# CONE Installation 

## Idpbuilder: Simplifying Internal Developer Platforms with CNOE
Welcome to `idpbuilder`, a tool designed to simplify the setup and management of internal developer platforms using the power of CNOE. The project provides to deployment methods by using your [local machine with a container runtime (Podman or Docker)](https://cnoe.io/docs/reference-implementation/installations/idpbuilder) or to [AWS](https://cnoe.io/docs/reference-implementation/installations/app-idp). This guide will walk you through setting up idpbuilder on your local machine using your selected container runtime.

## Prerequisites
Before installing idpbuilder, ensure you have the following:

* A container engine is needed locally, such as:
- [Docker desktop](https://www.docker.com/get-started/) (recommended)
- [Podman desktop](https://podman-desktop.io/) (idpbuilder can create a cluster using podman rootful)
- [Finch](https://runfinch.com/)

`Note:` Set the DOCKER_HOST env var property using podman to let idpbuilder to talk with the engine (e.g export DOCKER_HOST="unix:///var/run/docker.sock")

For this guideline, we opted for [podman container engine](https://podman.io/docs/installation) on MacOS (M2 ARM64). 

After installing, you need to create and start your first Podman machine:

```bash
podman machine init --cpus 2 --memory 8192 # You need at least 8GB of memory to fully deploy the CNOE stack
podman machine set --rootful
podman machine start
```
You can the verify the machine creation using:

```bash
podman machine ls
NAME                    VM TYPE     CREATED       LAST UP            CPUS        MEMORY      DISK SIZE
podman-machine-default  applehv     26 hours ago  Currently running  2           8GiB        100GiB
```
## Idpbuilder Installation
You can execute the following bash script to get started with a running version of the idpBuilder (inspect the script first if you have concerns):

```bash
curl -fsSL https://raw.githubusercontent.com/cnoe-io/idpbuilder/main/hack/install.sh | bash
```
Verify a successful installation by running the following command and inspecting the output for the right version:

```bash
idpbuilder version
idpbuilder 0.7.0 go1.21.3 darwin/arm64
```

Alternatively, you can run the following commands for a manual installation:

```bash
version=$(curl -Ls -o /dev/null -w %{url_effective} https://github.com/cnoe-io/idpbuilder/releases/latest)
version=${version##*/}
curl -L -o ./idpbuilder.tar.gz "https://github.com/cnoe-io/idpbuilder/releases/download/${version}/idpbuilder-$(uname | awk '{print tolower($0)}')-$(uname -m | sed 's/x86_64/amd64/').tar.gz"
tar xzf idpbuilder.tar.gz

./idpbuilder version
./idpbuilder 0.7.0 go1.21.3 darwin/arm64

./idpbuilder --help
Manage reference IDPs

Usage:
  idpbuilder [command]

Available Commands:
  completion  Generate the autocompletion script for the specified shell
  create      (Re)Create an IDP cluster
  delete      Delete an IDP cluster
  get         get information from the cluster
  help        Help about any command
  version     Print idpbuilder version and environment info

Flags:
  -h, --help               help for idpbuilder
  -l, --log-level string   Set the log verbosity. Supported values are: debug, info, warn, and error. (default "info")

Use "idpbuilder [command] --help" for more information about a command.
```

For installation alternatives, please refer to the [official documentation](https://cnoe.io/docs/reference-implementation/installations/idpbuilder/quick-start#running-in-codespaces).
In the [next section](), we will take quick look at how idpbuilder works. 
