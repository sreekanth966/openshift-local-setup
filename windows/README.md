# 🪟 OpenShift Local on Windows

[⬅️ Back to main README](../README.md)

## Prerequisites

Recommended starting point for this setup:

| Resource       |           Recommendation |
| -------------- | -----------------------: |
| CPU            |                  6 cores |
| RAM            |                   12 GB+ |
| Free disk      |                   40 GB+ |
| OS             | Supported 64-bit Windows |
| Virtualization |                  Hyper-V |

You also need:

* Administrator privileges when Windows requests elevation
* A Red Hat account
* An OpenShift pull secret
* Hardware virtualization enabled in BIOS/UEFI
* Internet access during installation
* A Windows edition that supports Hyper-V

---

## 1. Enable Hyper-V

Open **PowerShell as Administrator**.

Enable Hyper-V:

```powershell
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All
```

Restart Windows:

```powershell
Restart-Computer
```

After Windows restarts, open PowerShell and verify Hyper-V:

```powershell
Get-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V
```

The state should be:

```text
State : Enabled
```

---

## 2. Download OpenShift Local

Download the current OpenShift Local release:

[Download OpenShift Local](https://console.redhat.com/openshift/create/local)

Download the OpenShift pull secret from the Red Hat console.

Keep the pull secret somewhere accessible, for example:

```text
C:\Users\Sreekanth\Downloads\pull-secret.txt
```

Replace `Sreekanth` with your actual Windows username.

---

## 3. Install CRC

Extract the downloaded ZIP file, for example:

```text
C:\crc
```

Make sure the CRC executable exists:

```text
C:\crc\crc.exe
```

Add `C:\crc` to the Windows `PATH`.

Open a **new PowerShell window** and verify:

```powershell
crc version
```

---

## 4. Configure Resources

For a 16 GB RAM workstation, a recommended starting configuration is:

```powershell
crc config set cpus 6
crc config set memory 12288
```

Check the configuration:

```powershell
crc config view
```

Expected:

```text
- cpus   : 6
- memory : 12288
```

`memory 12288` allocates approximately 12 GB of RAM to the CRC VM.

---

## 5. Run CRC Setup

Run:

```powershell
crc setup
```

CRC will configure the Windows host and prepare the environment required for OpenShift Local.

Wait for the setup to complete successfully before continuing.

---

## 6. Start OpenShift Local

Start CRC using your pull secret:

```powershell
crc start --pull-secret-file C:\Users\Sreekanth\Downloads\pull-secret.txt
```

Use the actual path to your pull secret.

Alternatively:

```powershell
crc start --pull-secret-file <path-to-pull-secret>
```

CRC will create and start the OpenShift Local VM.

---

## 7. Check Status

Check the CRC status:

```powershell
crc status
```

The output should indicate that the CRC VM and OpenShift cluster are running.

---

## 8. Open the OpenShift Console

Open the console:

```powershell
crc console
```

To display the console URL:

```powershell
crc console --url
```

The URL normally resembles:

```text
https://console-openshift-console.apps-crc.testing
```

The exact URL can vary by CRC configuration and release.

---

## 9. Get Credentials

Display the console credentials:

```powershell
crc console --credentials
```

Use the credentials displayed by CRC to log in to the OpenShift console when required.

---

## 10. Configure `oc`

Configure the OpenShift CLI environment:

```powershell
crc oc-env
```

Follow the command displayed by CRC.

Then verify the OpenShift CLI:

```powershell
oc version
```

---

## 11. Verify the Cluster

Check the nodes:

```powershell
oc get nodes
```

Check all pods:

```powershell
oc get pods -A
```

Check cluster operators:

```powershell
oc get clusteroperators
```

Check projects:

```powershell
oc get projects
```

---

## 12. Test Application

Create a test project:

```powershell
oc new-project dev
```

Deploy NGINX:

```powershell
oc new-app nginx
```

Check the pods:

```powershell
oc get pods
```

Expose the service:

```powershell
oc expose svc/nginx
```

Get the route:

```powershell
oc get route
```

Open the route shown by the command in a browser.

---

# Windows Troubleshooting

## Check Hyper-V

Run PowerShell as Administrator:

```powershell
Get-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V
```

The state should be:

```text
State : Enabled
```

## Check CRC Status

```powershell
crc status
```

## Check CRC Version

```powershell
crc version
```

## Rebuild CRC

If CRC becomes corrupted or needs to be recreated:

```powershell
crc stop
crc delete
crc cleanup
```

Restart Windows if required.

Then run:

```powershell
crc setup
```

Start CRC again:

```powershell
crc start --pull-secret-file C:\path\pull-secret.txt
```

## VPN Problems

Corporate VPNs can interfere with CRC networking and DNS.

If startup or networking fails, test CRC with the VPN disconnected where permitted by your organization's policies.

---

# Common Commands

## Check CRC

```powershell
crc version
crc status
```

## Start CRC

```powershell
crc start --pull-secret-file <path-to-pull-secret>
```

## Stop CRC

```powershell
crc stop
```

## Open the Console

```powershell
crc console
```

## Get the Console URL

```powershell
crc console --url
```

## Get Credentials

```powershell
crc console --credentials
```

## Configure the `oc` Environment

```powershell
crc oc-env
```

Follow the command displayed by CRC.

## Verify OpenShift

```powershell
oc version
oc get nodes
oc get clusteroperators
oc get projects
```

## Create a Project

```powershell
oc new-project dev
```

## Deploy a Test Application

```powershell
oc new-app nginx
```

## Expose the Application

```powershell
oc expose svc/nginx
```

## Get the Route

```powershell
oc get route
```

## View Pods

```powershell
oc get pods
```

## View Logs

```powershell
oc logs <pod-name>
```

## Delete CRC

```powershell
crc delete
```

## Clean Up CRC Host Configuration

```powershell
crc cleanup
```

---

# Installation Flow

The complete Windows installation flow is:

```text
Windows
   │
   ├── BIOS/UEFI Virtualization
   │
   ├── Hyper-V
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

* Hyper-V must be enabled before starting OpenShift Local.
* CPU and memory settings are applied when the CRC instance starts.
* The OpenShift pull secret is required when starting OpenShift Local.
* Keep enough free RAM available for both Windows and the CRC VM.
* Corporate VPNs may interfere with CRC networking or DNS.
* Use a Windows version supported by the OpenShift Local release you are installing.
* The exact console URL and command output can vary between OpenShift Local releases.
