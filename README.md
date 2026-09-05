# AbletonOS Dev Container

This is a reverse engineered devcontainer targeting C/C++ programs and kernel modules for `AbletonOS` / `Push 3 Standalone`. Newer OS updates may change things and break compatibility, make sure to use the appropriate release tag for this project to ensure compatibility. If the latest tag doesn't match the latest AbletonOS version, please check [issues]() and if one does not exist for the target version, please create a new issue.

## Goals

- Reduce friction when starting new projects that target AbletonOS

## Terms

| Term          | Definition                                                                                                                                             |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| AbletonOS     | The operating system running on Abletons' [Push 3 Standalone](https://www.ableton.com/en/push/) device.                                                                                |
| Dev Container | A docker container designed to standardize the development envrionment for a project. See [Dev Container docs](https://containers.dev/) to learn more. |

## Compatibility

```bash
root@push:~# cat /proc/version
Linux version 5.15.48-intel-pk-preempt-rt (ci@abletonos-linux-noble-00) (x86_64-oe-linux-gcc (GCC) 11.5.0, GNU ld (GNU Binutils) 2.38.20220708) #1 SMP Tue Jun 21 16:59:08 UTC 2022
root@push:~# cat /proc/sys/kernel/osrelease
5.15.48-intel-pk-preempt-rt
root@push:~# uname -a
Linux push 5.15.48-intel-pk-preempt-rt #1 SMP Tue Jun 21 16:59:08 UTC 2022 x86_64 x86_64 x86_64 GNU/Linux
```

## Examples

These are two projects that demonstrate how to use this dev container. See the below template repos for more details;

- [push-dev](https://github.com/JGuzak/push-dev) is a project that builds a handful of linux kernel modules not shipped with the stock AbletonOS image for development/debugging purposes.
- Bare hello-world[userspace app example]() for Push 3 Standalone

## How to Use

This is a repo that can be included in your project as a submodule. It defines a boilerplate docker image to cross compile C and C++ programs/kernel modules for AbletonOS. It does not provide all neccessary components to develop or test things on AbletonOS, simply provide a consistent starting point for AbletonOS projects.

### Include In A Project

Add this repo as a submodule to your project:

```bash
git submodule add https://github.com/JGuzak/push-dev ./external/push-dev
```

**Minimal project structure:**

```bash
.
|-- .devcontainer/
|   -- compose.yaml
|   -- devcontainer.json
|   -- Dockerfile
|-- build/
|-- external/
|   -- ableton-os-devcontainer/
|-- src/
```

Use the following outlines for minimal expected .devcontainer file contents.

**compose.yaml:**

```yaml
services:
  your-project-name-devcontainer:
    build:
      context: .
      dockerfile: Dockerfile
      pull: false
    pull_policy: never
    container_name: your-project-name-devcontainer
    init: true
    restart: unless-stopped
    working_dir: /workspace
    volumes:
      - ..:/workspace
      - ../src:/src:ro
      - ../build:/out
```

**devcontainer.json:**

```json
{
  "name": "Your Project Name devcontainer",
  "dockerComposeFile": "compose.yaml",
  "service": "your-project-name-devcontainer",
  "workspaceFolder": "/workspace",
  "initializeCommand": "docker-compose.exe -f external/ableton-os-devcontainer/compose.yaml build ableton-os-devcontainer",
  "customizations": {
    "vscode": {
      "extensions": []
    }
  }
}

```

**Dockerfile:**

```yaml
#syntax=docker/dockerfile:1
FROM ableton-os-devcontainer:latest
```

### Start The Devcontainer

In `VS Code`, use the [dev container extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) and command pallette `Dev Containers: Open folder in Container...` to spin up the dev container from the project folder.

Or run Docker Compose from the consuming project root:

```bash
cd .devcontainer
docker compose up -d
```

The Docker image builds a minimal prefixed kernel toolchain from GNU sources
and validates it against the Push kernel tool versions:

```text
x86_64-oe-linux-gcc (GCC) 11.5.0
GNU ld (GNU Binutils) 2.38.20220708
```
