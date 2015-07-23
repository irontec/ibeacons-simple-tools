# iBeacons-simple-tools
Using easily your BLE as an iBeacon or use them as a LE Scanner to find out any iBeacon near you.

### Requisties

The folowing binaries must be accesible from your $PATH
 * hciconfig
 * hcidump
 * hcitool

On Debian based Distributions:
```bash
apt-get install -y bluez bluez-hcidump
```


## launch-ibeacon
The script will allow you to convert your little BLE dongle into a fully configurable iBeacon.

**Usage:**
```bash
./launch-ibeacon -i hciN -u UUID -M MAYOR -m MINOR -p dBm
```
Default Values:
 * -i hci0
   * HCI Device (as listed with _hciconfig -a_)
 * -u "0"
   * UUID will be left-padded with "0", and truncated to 32 chars. Illegal Characters will be ignored.
 * -M 0
   * Mayor value, specified in decimal. Never bigger than 65535 (0xFFFF)
 * -m 0
   * Minor value, specified in decimal. Never bigger than 65535 (0xFFFF)
 * -p "-59"
   * Measured Power in dBm (the script will transform the values as a two complement)

**Example:**
```bash
# ./launch-ibeacon -i hci1 -u 01 -M 666 -M 333 -p "-59"
 · Setting up hci1 [leadv, noscan] ✓
 · Setting up beacon payload ✓
     UUID: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 01
     MAYOR: 01 4D (333)
     MINOR: 00 00 (0)
     POWER: C5 (-59)
 · Activating Beacon ✓
     hcitool -i hci1 cmd 0x08 0x0008 1E 02 01 1A 1A FF 4C 00 02 15 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 01 01 4D 00 00 C5 00
     ```


## ibeacon-scan
This little script will allow you to listen to any ibeacon near you.

The script is **heavily** based on iBeacon Scan by Radius Networks.

Usage:
```bash
# ./ibeacon-scan [-i hciX] [-b (bare output, easely parseable)]
````

Default Values:
 * -i _hci0_


The command will enable "lescan" mode on hciX, and will be generating the following output perpetually:
```text
UUID: 00000000-0000-0000-C000-000000000028 MAJOR: 1 MINOR: 1 POWER: -59
UUID: 00000000-0000-0000-C000-000000000028 MAJOR: 1 MINOR: 1 POWER: -59
UUID: 23A01AF0-232A-4518-9C0E-323FB773F5EF MAJOR: 35469 MINOR: 29443 POWER: -77
UUID: 23A01AF0-232A-4518-9C0E-323FB773F5EF MAJOR: 52606 MINOR: 13223 POWER: -77
UUID: 00000000-0000-0000-C000-000000000028 MAJOR: 1 MINOR: 1 POWER: -59
UUID: 23A01AF0-232A-4518-9C0E-323FB773F5EF MAJOR: 4322 MINOR: 34831 POWER: -77
UUID: 00000000-0000-0000-0000-000000000000 MAJOR: 0 MINOR: 0 POWER: -59
UUID: 00000000-0000-0000-C000-000000000028 MAJOR: 1 MINOR: 1 POWER: -59
UUID: 23A01AF0-232A-4518-9C0E-323FB773F5EF MAJOR: 35469 MINOR: 29443 POWER: -77
UUID: 23A01AF0-232A-4518-9C0E-323FB773F5EF MAJOR: 52606 MINOR: 13223 POWER: -77
UUID: 23A01AF0-232A-4518-9C0E-323FB773F5EF MAJOR: 52606 MINOR: 13223 POWER: -77
UUID: 23A01AF0-232A-4518-9C0E-323FB773F5EF MAJOR: 4322 MINOR: 34831 POWER: -77
UUID: 23A01AF0-232A-4518-9C0E-323FB773F5EF MAJOR: 35469 MINOR: 29443 POWER: -77
UUID: 23A01AF0-232A-4518-9C0E-323FB773F5EF MAJOR: 52606 MINOR: 13223 POWER: -77
UUID: 23A01AF0-232A-4518-9C0E-323FB773F5EF MAJOR: 52606 MINOR: 13223 POWER: -77
```

If "-b" option is passed:
```text
00000000-0000-0000-C000-000000000028 1 1 -59
00000000-0000-0000-0000-000000000000 0 0 -59
00000000-0000-0000-C000-000000000028 1 1 -59
23A01AF0-232A-4518-9C0E-323FB773F5EF 35469 29443 -77
23A01AF0-232A-4518-9C0E-323FB773F5EF 52606 13223 -77
00000000-0000-0000-C000-000000000028 1 1 -59
00000000-0000-0000-C000-000000000028 1 1 -59
23A01AF0-232A-4518-9C0E-323FB773F5EF 4322 34831 -77
00000000-0000-0000-0000-000000000000 0 0 -59
```


