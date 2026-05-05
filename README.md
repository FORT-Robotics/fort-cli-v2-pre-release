# FORT CLI V2

The **FORT CLI V2** is a command-line interface tool designed to help you manage, configure, and update your FORT Robotics devices. This tool allows you to easily discover connected hardware, ensure your device configurations are up to date, and manage firmware versions directly from your terminal.  Device actions are synced back to FORT Manager.


## 1. Logging In

Option A: **API Key**

You can create an API get in FORT Manager if you have an active subscription.


```bash
fortcli login --api-key zpka_my_api_key
```

Option B: **Interactive Login**

This will provide a URL and a unique code to authorize your device via a web browser.

```bash
fortcli login --interactive
```

Example Output:
```bash
 To authorize this device, open the following URL and follow the instructions:
 https://auth.fortrobotics.com/activate?user_code=USER_CODE
 If prompted, enter the user code: USER_CODE
 Authorization URL: https://auth.fortrobotics.com/activate
```

**Logging Out**

To clear your credentials and end the session:
```bash
fortcli logout
```


## 2. Discovering Devices

Before performing updates, verify that your FORT device is connected and recognized by the system.

Run the following command to list all attached devices:

```bash
./fortcli device list
```

**Example Output:**

```text
+--------------+------------+-----------+------------+
| Conn ID      | Model Name | Interface | Serial     |
+--------------+------------+-----------+------------+
| /dev/ttyACM0 | NSCP 1003  | USB       | WBJFU8CNE6 |
+--------------+------------+-----------+------------+
```

The device lise can also be outputted as a JSON object.

```base
./fortcli device list --json
```

```test
[
  {
    "connId": "/dev/ttyACM0",
    "modelName": "NSCP 1003",
    "interface": "USB",
    "serialNumber": "WBJFU8CNE6"
  }
]
```

---

## 3. Managing Configuration

You can update your device configuration automatically or manually download and apply files.

### Option A: Automatic Update

The `update` command downloads the latest configuration from FORT Manager and applies it to the device in one step.

```bash
./fortcli device configuration update <SERIAL_NUMBER> 
```

### Option B: Manual Download & Apply

If you need to save the configuration file locally first or apply a specific file:

**Step 1: Download the configuration**
This saves the configuration file (e.g., `FORT_WBJFU8CNE6.cbor`) to your current directory (`-o .`).

```bash
./fortcli device configuration download <SERIAL_NUMBER> -o .
```

**Step 2: Apply the configuration**
Apply the downloaded file to the device.

```bash
./fortcli device configuration apply <SERIAL_NUMBER> -f <FILENAME.cbor>
```

---

## 4. Managing Firmware

Keep your device running efficiently by ensuring it is on the latest firmware.

### Option A: Automatic Update (Recommended)

The `update` command fetches the latest firmware bundle and installs it on your device immediately.

```bash
./fortcli device firmware update <SERIAL_NUMBER> 
```

### Option B: Manual Download & Apply

Use this method if you need to download a specific firmware version for later use or offline installation.

**Step 1: Download the firmware**
This downloads the firmware bundle (e.g., `4.0.1-NSC.zip`) to your current directory.

```bash
./fortcli device firmware download -s <SERIAL_NUMBER> -o .
```

**Step 2: Apply the firmware**
Install the specific firmware bundle onto the device.

```bash
./fortcli device firmware apply <SERIAL_NUMBER> -f <FILENAME.zip>
```

> **Note:** The device will automatically reboot after a successful configuration or firmware update.