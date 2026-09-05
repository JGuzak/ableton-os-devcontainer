# AbletonOS Dev Container

## Goals

- Reduce friction when starting new projects that target AbletonOS

## Terms

| Term          | Definition                                                                                                                                             |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| AbletonOS     | The operating system running on Abletons' [Push 3 Standalone](https://www.ableton.com/en/push/) device.                                                                                |
| Dev Container | A docker container designed to standardize the development envrionment for a project. See [Dev Container docs](https://containers.dev/) to learn more. |

## Use

This is a repo that can be included in your project as a submodule. It defines a boilerplate docker image to cross compile C and C++ programs/kernel modules for AbletonOS. It does not provide all neccessary components to develop or test things on AbletonOS, simply provide a consistent starting point for AbletonOS projects.

### Examples

There are two template projects that demonstrate how this repo can be used. See the below template repos for more details;

- push-dev is a [Kernel module example](https://github.com/JGuzak/push-dev) project that builds a handful of Linux kernel modules not shipped with the stock AbletonOS image for development/debugging purposes.
- [Hello-world userspace app example]()
