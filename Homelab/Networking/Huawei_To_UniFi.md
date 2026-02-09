# Network Migration: Huawei Bridge to UniFi Express

**Goal:** Configure Huawei router as a transparent bridge (ONT/VLAN 835) and UniFi Express as the primary gateway (PPPoE/NAT/DHCP).

---

## 1. Preparation (Pre-Downtime)

### Extract PPPoE Credentials
* Log into Huawei: `192.168.1.254`.
* * Download config XML and decrypt using ADSLGR tool.
* * Record PPPoE Username and Password.
* * **Note:** Do not modify VoIP (VLAN 837) settings.
### IP Strategy
* **Huawei LAN IP:** `192.168.1.254` (DHCP Disabled).
* * **UniFi LAN IP:** `192.168.1.1`.
* * **Subnet:** `255.255.255.0` (/24).
* * **UniFi DHCP Range:** `192.168.1.100 - 192.168.1.253` (Avoids collision with Huawei).
---

## 2. Huawei Configuration

1. **WAN Modification:** * Disable existing WAN connection.
2.     * Create New Bridge WAN.
3.     * Type: `Bridge`.
4.     * Protocol: `IPoE/Ethernet`.
5.     * VLAN: `Enabled` (ID: 835).
6.     * Binding: Bind to a specific port (e.g., `LAN3`).
7. 2. **Services:** Disable DHCP server and Wi-Fi.
8. 3. **Save:** Apply changes. Internet connectivity will be lost until UniFi is configured.
---

## 3. Physical Cabling

* **Fiber Line:** Connect to Huawei ONT port.
* * **Bridge Link:** Huawei `LAN3` -> UniFi Express `WAN` port.
* * **Management Link:** Huawei `LAN1` -> Ethernet Switch.
* * **LAN Link:** UniFi Express `LAN` port -> Ethernet Switch.
---

## 4. UniFi Express Configuration

Access the console via browser at `192.168.1.1`.

### Internet Settings
* **Connection Type:** `PPPoE`.
* * **Username/Password:** (The credentials extracted from Huawei).
* * **VLAN ID:** `Disabled` (Handled by the Huawei bridge).
* * **IPv4:** Enabled.
### Network Settings (LAN)
Configure the "Default" network:
* **Gateway IP:** `192.168.1.1`.
* * **DHCP Mode:** `Server`.
* * **DHCP Range:** `192.168.1.100` to `192.168.1.253`.
* * **DNS:** `1.1.1.1` (Optional).
---

## 5. Verification & Troubleshooting

### Verification Commands
```bash
# Check connectivity to UniFi Gateway
ping 192.168.1.1

# Check connectivity to Huawei Management
ping 192.168.1.254

# Verify External Routing
ping google.com
