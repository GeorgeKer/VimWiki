# Immich Setup: RHEL and Rootless Podman

---

## 1. Configuration and Installation

* Config: Follow /myHomelab/Immich/README.md for pod creation.
* Install: Use /myHomelab/Immich/compose.yml for installation.

---

## 2. Troubleshooting: Hardware Transcoding

### Issue
`AssetEncodeVideo` jobs failed. Error found in DEBUG logs:
Error: `No /dev/dri devices found`

### Cause
1. Permissions: The rootless user does not have permission to access the GPU render nodes.
2. Mapping: The /dev/dri device is not being passed from the host into the Podman container.

---

## 3. Solution

### Step 1: Host Permissions
Ensure your user is part of the render group to access the Intel QuickSync hardware.

```bash
# Verify device owner (should be root:render)
ls -l /dev/dri/renderD128

# Add your current user to the render group
sudo usermod -aG render $USER

# Apply changes (Log out/in or use newgrp)
newgrp render
```

### Step 2: Compose Configuration
Add the devices mapping to both immich-server and immich-machine-learning in your compose.yml:

```yaml
devices:
  - /dev/dri:/dev/dri
```

### Step 3: Podman Refresh
If permissions are still denied after the group change, refresh the podman user namespace:

```bash
podman system migrate
```

---
