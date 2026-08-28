# 🍎 OpenShift Local on macOS

[⬅️ Back to main README](../README.md)

## Prerequisites

Recommended starting point for this setup:

| Resource       |                                                                    Recommendation |
| -------------- | --------------------------------------------------------------------------------: |
| CPU            |                                                                           6 cores |
| RAM            |                                                                            12 GB+ |
| Free disk      |                                                                            40 GB+ |
| Architecture   | Supported 64-bit Intel or Apple Silicon, depending on the OpenShift Local release |
| Virtualization |                                            Supported macOS virtualization backend |

Before installing, verify that your macOS version, CPU architecture, and OpenShift Local release are compatible.

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

> **Important:** Do not assume that every OpenShift Local release supports both Intel (`x86_64`) and Apple Silicon (`arm64`). Check the supported platforms for the specific OpenShift Local release you are installing.

---

## 1. Download OpenShift Local

Download the current OpenShift Local release:

[Download OpenShift Local](https://console.redhat.com/openshift/create/local)

Download the OpenShift pull secret from the Red Hat console.

Keep the pull secret somewhere accessible, for example:

```text
~/Downloads/pull-secret.txt
```

---

## 2. Install CRC

Extract the downloaded macOS archive.

Install the `crc` executable into your PATH:

```bash
sudo install -m 0755 crc /usr/local/bin/crc
```

Verify the installation:

```bash
crc version
```

---

## 3. Configure Resources

Configure CPU and memory before starting CRC.

Recommended starting configuration:

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

`memory 12288` allocates approximately 12 GB of RAM to the CRC VM.

---

## 4. Run CRC Setup

Run:

```bash
crc setup
```

CRC will configure the macOS host and prepare the environment required for OpenShift Local.

Wait for the setup to complete successfully before continuing.

---

## 5. Start OpenShift Local

Start CRC using your pull secret:

```bash
crc start --pull-secret-file ~/Downloads/pull-secret.txt
```

If the pull secret is stored elsewhere:

```bash
crc start --pull-secret-file <path-to-pull-secret>
```

CRC will create and start the OpenShift Local VM.

---

## 6. Check Status

Check the CRC status:

```bash
crc status
```

The output should indicate that the CRC VM and OpenShift cluster are running.

---

## 7. Open the OpenShift Console

Open the console:

```bash
crc console
```

To display the console URL:

```bash
crc console --url
```

The exact console URL can vary by CRC release and configuration.

---

## 8. Get Credentials

Display the console credentials:

```bash
crc console --credentials
```

Use the credentials displayed by CRC when required.

---

## 9. Configure `oc`

Configure the OpenShift CLI environment:

```bash
crc oc-env
```

Follow the command displayed by CRC.

Then verify:

```bash
oc version
```

---

## 10. Verify the Cluster

Check the nodes:

```bash
oc get nodes
```

Check all pods:

```bash
oc get pods -A
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

## 11. Test Application

Create a test project:

```bash
oc new-project dev
```

Deploy NGINX:

```bash
oc new-app nginx
```

Check the pods:

```bash
oc get pods
```

Expose the service:

```bash
oc expose svc/nginx
```

Get the route:

```bash
oc get route
```

Open the route shown by the command in a browser.

---

# Common Commands

## Check CRC

```bash
crc version
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

## Open the Console

```bash
crc console
```

## Get the Console URL

```bash
crc console --url
```

## Get Credentials

```bash
crc console --credentials
```

## Configure the `oc` Environment

```bash
crc oc-env
```

Follow the command displayed by CRC.

## Verify OpenShift

```bash
oc version
oc get nodes
oc get clusteroperators
oc get projects
```

## Create a Project

```bash
oc new-project dev
```

## Deploy a Test Application

```bash
oc new-app nginx
```

## Expose the Application

```bash
oc expose svc/nginx
```

## Get the Route

```bash
oc get route
```

## View Pods

```bash
oc get pods
```

## View Logs

```bash
oc logs <pod-name>
```

## Delete CRC

```bash
crc delete
```

## Clean Up CRC Host Configuration

```bash
crc cleanup
```

---

# Installation Flow

The complete macOS installation flow is:

```text
macOS
  │
  ├── CPU Architecture Check
  │
  ├── Supported Virtualization
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

* Check the supported macOS versions and CPU architectures for the specific OpenShift Local release before installation.
* Apple Silicon (`arm64`) support depends on the OpenShift Local release.
* Intel (`x86_64`) support also depends on the OpenShift Local release.
* CPU and memory settings are applied when the CRC instance starts.
* The OpenShift pull secret is required when starting OpenShift Local.
* Keep enough free RAM available for both macOS and the CRC VM.
* The exact virtualization backend and supported platforms can vary between OpenShift Local releases.
* The exact console URL and command output can vary between OpenShift Local releases.
