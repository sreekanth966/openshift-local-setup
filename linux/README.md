# 🐧 OpenShift Local on Linux

[⬅️ Back to main README](../README.md)

## Prerequisites

Recommended starting point:

| Resource | Recommendation |
|---|---:|
| CPU | 6 cores |
| RAM | 12 GB+ |
| Free disk | 40 GB+ |
| Architecture | 64-bit x86 where supported |
| Virtualization | KVM |

Use a Linux distribution/version supported by the current OpenShift Local release.

## 1. Verify KVM

```bash
lsmod | grep kvm
```

On Ubuntu/Debian, install the required virtualization packages:

```bash
sudo apt update
sudo apt install -y qemu-system-x86 libvirt-daemon libvirt-daemon-system libvirt-clients
```

Verify again:

```bash
sudo usermod -aG kvm,libvirt $USER
sudo systemctl enable --now libvirtd
virsh list --all
```

Make sure your user has the required KVM/libvirt permissions.

## 2. Download OpenShift Local

Download the Linux release:

https://console.redhat.com/openshift/create/local

Download the OpenShift pull secret.

## 3. Install CRC

Extract the archive and place `crc` in PATH:

```bash
sudo install -m 0755 crc /usr/local/bin/crc
```

Verify:

```bash
crc version
```

## 4. Run Setup

```bash
crc setup
```

## 5. Configure Resources

```bash
crc config set cpus 6
crc config set memory 12288
```

Check:

```bash
crc config view
```

## 6. Start OpenShift

```bash
crc start --pull-secret-file ~/Downloads/pull-secret.txt
```

## 7. Verify

```bash
crc status
```

## 8. Open Console

```bash
crc console
```

Or:

```bash
crc console --url
```

## 9. Configure `oc`

```bash
crc oc-env
```

Follow the command displayed by CRC.

Then:

```bash
oc version
oc get nodes
oc get clusteroperators
```

## Common Commands

Check CRC:

```bash
crc version
crc status
```

Start:

```bash
crc start --pull-secret-file <path-to-pull-secret>
```

Stop:

```bash
crc stop
```

Open the console:

```bash
crc console
```

Get the console URL:

```bash
crc console --url
```

Get credentials:

```bash
crc console --credentials
```

Configure the `oc` environment:

```bash
crc oc-env
```

Verify OpenShift:

```bash
oc version
oc get nodes
oc get clusteroperators
oc get projects
```

Create a project:

```bash
oc new-project dev
```

Deploy a test application:

```bash
oc new-app nginx
```

Expose it:

```bash
oc expose svc/nginx
```

Get the route:

```bash
oc get route
```

View pods:

```bash
oc get pods
```

View logs:

```bash
oc logs <pod-name>
```

Delete CRC:

```bash
crc delete
```

Clean up CRC host configuration:

```bash
crc cleanup
```
