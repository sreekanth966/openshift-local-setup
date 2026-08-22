# OpenShift Local Installation Guide

<p align="center">
  <strong>OpenShift Local / CRC</strong><br>
  Install and run a local OpenShift cluster on your preferred operating system.
</p>

## Choose Your Operating System

<p align="center">
  <a href="./windows/README.md">
    <img src="https://img.shields.io/badge/Windows-0078D4?style=for-the-badge&logo=windows&logoColor=white" alt="Windows">
  </a>
  &nbsp;
  <a href="./linux/README.md">
    <img src="https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black" alt="Linux">
  </a>
  &nbsp;
  <a href="./macos/README.md">
    <img src="https://img.shields.io/badge/macOS-000000?style=for-the-badge&logo=apple&logoColor=white" alt="macOS">
  </a>
</p>

| Platform | Guide |
|---|---|
| 🪟 Windows | [Install OpenShift Local on Windows](./windows/README.md) |
| 🐧 Linux | [Install OpenShift Local on Linux](./linux/README.md) |
| 🍎 macOS | [Install OpenShift Local on macOS](./macos/README.md) |

## What You Will Learn

- Install OpenShift Local / CRC
- Configure CPU, memory and disk
- Configure virtualization
- Start and stop the local OpenShift cluster
- Access the OpenShift web console
- Configure the `oc` CLI
- Create projects
- Deploy applications
- Expose applications with Routes
- Troubleshoot CRC
- Prepare a local OpenShift DevSecOps lab

## Common Architecture

```text
                    Developer Workstation
                           |
                +----------+----------+
                |                     |
             Browser                oc CLI
                |                     |
                +----------+----------+
                           |
                    OpenShift Local
                         / CRC
                           |
          +----------------+----------------+
          |                |                |
       Projects        Operators         Router
          |                |                |
     Applications       Platform          Routes
```

## Repository Structure

```text
openshift-local/
│
├── README.md
│
├── windows/
│   └── README.md
│
├── linux/
│   └── README.md
│
└── macos/
    └── README.md
```

## Official Resources

- [OpenShift Local download](https://console.redhat.com/openshift/create/local)
- [Red Hat Documentation](https://docs.redhat.com/)
- [OpenShift Documentation](https://docs.redhat.com/en/documentation/openshift_container_platform/)

> OpenShift Local / CRC is intended for development and testing. It is not a production HA OpenShift cluster.
