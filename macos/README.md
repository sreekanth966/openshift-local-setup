# 🍎 OpenShift Local on macOS

[⬅️ Back to main README](../README.md)

## Prerequisites

Recommended starting point:

| Resource | Recommendation |
|---|---:|
| CPU | 6 cores |
| RAM | 12 GB+ |
| Free disk | 40 GB+ |
| Architecture | Intel or Apple Silicon, subject to current release support |
| Virtualization | Supported macOS virtualization |

Check your CPU architecture:

```bash
uname -m
```

Typical results:

```text
x86_64
```

or:

```text
arm64
```

Before installing, verify that your OpenShift Local release supports your macOS version and architecture.

## 1. Download OpenShift Local

Download the current macOS release:

https://console.redhat.com/openshift/create/local

Download the OpenShift pull secret.

## 2. Install CRC

Extract the archive and put `crc` in PATH.

For example:

```bash
sudo install -m 0755 crc /usr/local/bin/crc
```

Verify:

```bash
crc version
```

## 3. Run Setup

```bash
crc setup
```

## 4. Configure Resources

```bash
crc config set cpus 6
crc config set memory 12288
```

Check:

```bash
crc config view
```

## 5. Start OpenShift

```bash
crc start --pull-secret-file ~/Downloads/pull-secret.txt
```

## 6. Check Status

```bash
crc status
```

## 7. Open Console

```bash
crc console
```

Or:

```bash
crc console --url
```

## 8. Configure `oc`

```bash
crc oc-env
```

Follow the command displayed by CRC.

Verify:

```bash
oc version
oc get nodes
oc get clusteroperators
oc get projects
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
