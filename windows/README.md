# 🪟 OpenShift Local on Windows

[⬅️ Back to main README](../README.md)

## Prerequisites

Recommended starting point:

| Resource | Recommendation |
|---|---:|
| CPU | 6 cores |
| RAM | 12 GB+ |
| Free disk | 40 GB+ |
| OS | Supported 64-bit Windows |
| Virtualization | Hyper-V |

You also need:

- Administrator privileges when Windows requests elevation
- A Red Hat account
- OpenShift pull secret
- Hardware virtualization enabled in BIOS/UEFI
- Internet access during installation

## 1. Enable Hyper-V

Open **PowerShell as Administrator**:

```powershell
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All
```

Restart:

```powershell
Restart-Computer
```

Verify:

```powershell
Get-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V
```

The state should be:

```text
State : Enabled
```

## 2. Download OpenShift Local

Download the Windows release:

https://console.redhat.com/openshift/create/local

Download the OpenShift pull secret from the Red Hat console.

## 3. Install CRC

Extract the ZIP, for example:

```text
C:\crc
```

Make sure:

```text
C:\crc\crc.exe
```

exists.

Add `C:\crc` to your Windows PATH.

Open a new PowerShell window:

```powershell
crc version
```

## 4. Run Setup

```powershell
crc setup
```

## 5. Configure Resources

For a 16 GB RAM workstation:

```powershell
crc config set cpus 6
crc config set memory 12288
```

Check:

```powershell
crc config view
```

## 6. Start OpenShift

Example:

```powershell
crc start --pull-secret-file C:\Users\Sreekanth\Downloads\pull-secret.txt
```

Use your actual Windows username/path.

## 7. Check Status

```powershell
crc status
```

Expected:

```text
CRC VM:          Running
OpenShift:       Running
```

## 8. Open the Console

```powershell
crc console
```

Or:

```powershell
crc console --url
```

The URL normally resembles:

```text
https://console-openshift-console.apps-crc.testing
```

## 9. Get Credentials

```powershell
crc console --credentials
```

Use the displayed `kubeadmin` credentials.

## 10. Configure `oc`

```powershell
crc oc-env
```

Follow the command printed by CRC.

Then:

```powershell
oc version
```

## 11. Verify the Cluster

```powershell
oc get nodes
oc get pods -A
oc get clusteroperators
oc get projects
```

## 12. Test Application

```powershell
oc new-project dev
oc new-app nginx
oc expose svc/nginx
oc get route
```

Open the route shown by:

```powershell
oc get route
```

## Windows Troubleshooting

### Hyper-V check

```powershell
Get-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V
```

### CRC status

```powershell
crc status
```

### Rebuild CRC

```powershell
crc stop
crc delete
crc cleanup
```

Restart Windows, then:

```powershell
crc setup
crc start --pull-secret-file C:\path\pull-secret.txt
```

### VPN problems

Corporate VPNs can interfere with CRC networking and DNS. If startup/networking fails, test CRC with the VPN disconnected where your organization's policy permits.

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
