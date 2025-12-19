# Keelson

Keelson is designed to be a near drop in replacement for [Keel](https://keel.sh) but
with better docs, better logging, a more active community, a simpler implementation
that is more opinionated about what right looks like. 


## Project Structure

This repository is a [Branchout/branchout](https://github.com/Branchout/branchout) project.
The repos available are listed in `Branchoutprojects` and can be found on disk in a folder of
the same name as their prefix (before first hyphen). Its two purposes are to organise and
document and make working with all the repos easy and clean AND to put centralised docs in.


## Tech Stack

A simple shell and OCI image based stack - see the [Dependencies](#dependencies) section for more info.


## Development Setup

A unix environment (Linux, MacOS, WSL2, etc) with Git and Docker installed. All other tools (BATS, Shellcheck, yq, coreutils, etc) run inside containers.

### macOS (Homebrew)

Git is included with Xcode Command Line Tools (required for Homebrew). For Docker, install the CLI and one of the following runtimes:

```bash
brew install docker

# Pick ONE runtime:
brew install colima           # Lightweight, recommended
brew install rancher-desktop  # Includes Kubernetes
brew install --cask docker    # Docker Desktop (commercial license may apply)
```

### Linux

#### Debian/Ubuntu (apt)

```bash
sudo apt update
sudo apt install git docker.io
```

#### RHEL/Fedora (yum/dnf)

```bash
sudo dnf install git docker
```

#### Arch

```bash
sudo pacman -S git docker
```


## Build & Run

Build and run behaviour will be per sub project - this project is for humans and LLMs to consume/contribute to.


## Testing

<!-- TODO -->


## Code Style

- Always ensure text files have a trailing newline
- Repos are always named in the style group-name group-name-morename group-name-morename-evenmore etc
- With the exception of README.md and CLAUDE.md, all markdown docs should be NamedLikeThis.md


## Architecture

Keelson is designed to run in a cluster and manage either all resources for the entire
cluster or just a particular namespace as needed. The configuration and permissions that
are granted will determine this behaviour. It scans the cluster or namespace for workloads
that are annotated to be controlled by Keelson or come be changed to become controlled by
Keelson and subscribes to events for those resources. On a schedule any resources that are
currently configured for Keelson control have their image repository queried for tags and
if any are newer (greater than from a semantic version style point of view) they are updated
or not as determined by the behaviour configured in annotations eg minor patch or major/all.
When a workload resource is updated to a new tag it causes a deployment (in the style that
the workload configuration specifies, eg rolling) and a new version to run with the same old
configuration for that workload.


## Common Tasks

<!-- TODO -->


## Dependencies

Only common tools such as:

* Bash 3.2 up - shell runtime for branchout
* Branchout - bulk repo management and documentation
* Git - source control operations
* Docker - packaging OCI images, running containers for testing and builds

And inside containers others like:

* Bash 4.X - shell runtime for scripts
* kubectl - all kube API operations
* BATS - testing scripts
* Shellcheck - linting scripts
* yq 4 - manipulating YAML documents
* sed, awk, tail, head, etc etc - GNU versions of common unix utilities


## Notes

Be careful in shell scripts to not lose data from executing sub shells and trying to write to vars outside that context.
