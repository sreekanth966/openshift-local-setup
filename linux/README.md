# 🐧 OpenShift Local on Linux

[⬅️ Back to main README](../README.md)

## Prerequisites

Recommended starting point for this setup:

| Resource       |             Recommendation |
| -------------- | -------------------------: |
| CPU            |                    6 cores |
| RAM            |                     12 GB+ |
| Free disk      |                     40 GB+ |
| Architecture   | 64-bit x86 where supported |
| Virtualization |                        KVM |

Use a Linux distribution/version supported by the current OpenShift Local release.

---

## 1. Verify KVM

Check whether KVM is available:

```bash
lsmod | grep kvm
```

On Ubuntu/Debian, install the required virtualization packages:

```bash
sudo apt update
sudo apt install -y qemu-system-x86 libvirt-daemon libvirt-daemon-system libvirt-clients
```

Add your user to the required KVM and libvirt groups:

```bash
sudo usermod -aG kvm,libvirt $USER
```

Log out and log back in so the new group membership takes effect.

Enable and start libvirt:

```bash
sudo systemctl enable --now libvirtd
```

Verify KVM and libvirt:

```bash
lsmod | grep kvm
virsh list --all
```

`virsh list --all` may show no VMs at this stage. That is expected because CRC has not created its VM yet.

Make sure your user has the required KVM/libvirt permissions.

---

## 2. Download OpenShift Local

Download the current OpenShift Local release from:

[Download OpenShift Local](https://console.redhat.com/openshift/create/local)

Download the OpenShift pull secret from the same Red Hat account.

Keep the pull secret somewhere accessible, for example:

```text
~/Downloads/pull-secret.txt
```

---

## 3. Install CRC

Extract the downloaded CRC archive.

Enter the extracted directory:

```bash
cd crc-linux-<version>-amd64
```

Install the `crc` binary into your PATH:

```bash
sudo install -m 0755 crc /usr/local/bin/crc
```

Verify the installation:

```bash
crc version
```

Example:

```text
CRC version: 2.63.0
OpenShift version: 4.22.7
```

---

## 4. Configure Resources

Configure CPU and memory before starting CRC.

Recommended configuration for this setup:

```bash
crc config set cpus 6
crc config set memory 12288
```

Check the configuration:

```bash
crc config view
```

Expected:

```text
- cpus   : 6
- memory : 12288
```

`memory 12288` means approximately 12 GB of RAM is allocated to the CRC VM.

---

## 5. Run CRC Setup

Run the CRC host setup:

```bash
crc setup
```

CRC will configure the required host components and prepare the environment for the OpenShift Local VM.

Wait for `crc setup` to complete successfully before continuing.

---

## 6. Start OpenShift Local

Start the OpenShift Local cluster using your pull secret:

```bash
crc start --pull-secret-file ~/Downloads/pull-secret.txt
```

If your pull secret is stored somewhere else, replace the path with the actual file path:

```bash
crc start --pull-secret-file <path-to-pull-secret>
```

CRC will create and start the OpenShift Local VM.

---

## 7. Verify the Cluster

Check the CRC status:

```bash
crc status
```

You should eventually see the cluster in a running state.

---

## 8. Open the OpenShift Console

Open the OpenShift web console:

```bash
crc console
```

To print the console URL without opening a browser:

```bash
crc console --url
```

---

## 9. Configure `oc`

Configure the OpenShift CLI environment:

```bash
crc oc-env
```

CRC will display a command that needs to be executed.

Run the command displayed by CRC.

Then verify the CLI:

```bash
oc version
```

Check the OpenShift nodes:

```bash
oc get nodes
```

Check cluster operators:

```bash
oc get clusteroperators
```

---

# Common Commands

## Check CRC Version

```bash
crc version
```

## Check CRC Status

```bash
crc status
```

## Start CRC

```bash
crc start --pull-secret-file <path-to-pull-secret>
```

## Stop CRC

```bash
crc stop
```

## Open the OpenShift Console

```bash
crc console
```

## Get the Console URL

```bash
crc console --url
```

## Get Console Credentials

```bash
crc console --credentials
```

## Configure the `oc` Environment

```bash
crc oc-env
```

Follow the command displayed by CRC.

---

# OpenShift Verification

Check the OpenShift client:

```bash
oc version
```

Check nodes:

```bash
oc get nodes
```

Check cluster operators:

```bash
oc get clusteroperators
```

Check projects:

```bash
oc get projects
```

---

# Create a Project

Create a test project:

```bash
oc new-project dev
```

Verify:

```bash
oc get projects
```

---

# Deploy a Test Application

Deploy an NGINX application:

```bash
oc new-app nginx
```

Check the pods:

```bash
oc get pods
```

Check the service:

```bash
oc get svc
```

Expose the service:

```bash
oc expose svc/nginx
```

Get the route:

```bash
oc get route
```

---

# View Application Logs

List pods:

```bash
oc get pods
```

View logs:

```bash
oc logs <pod-name>
```

---

# CRC Lifecycle

Stop the OpenShift Local VM:

```bash
crc stop
```

Start it again:

```bash
crc start
```

Check its status:

```bash
crc status
```

Delete the CRC cluster:

```bash
crc delete
```

Clean up CRC host configuration:

```bash
crc cleanup
```

---

# Installation Flow

The complete installation flow is:

```text
Linux
  │
  ├── KVM / VT-x
  │
  ├── QEMU
  │
  ├── libvirt
  │
  ├── CRC
  │
  ├── Configure CPU + RAM
  │
  ├── crc setup
  │
  └── crc start
          │
          ▼
     CRC Virtual Machine
          │
          ▼
    OpenShift Local
          │
          ▼
       OpenShift
          │
          └── oc CLI
```

# Notes

* `qemu-system-x86` is used instead of `qemu-kvm` on Ubuntu/Debian systems where `qemu-kvm` is provided as a virtual package.
* CPU and memory settings are applied when the CRC instance starts.
* The pull secret is required when starting OpenShift Local.
* `virsh list --all` showing no VMs before `crc start` is normal.
* Ensure the host has enough available RAM before starting CRC.
* Use a Linux distribution/version supported by the OpenShift Local release you are installing.
