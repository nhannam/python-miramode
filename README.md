# python-miramode

Python module for controlling Mira Mode digital showers via BLE.

Mira Mode is a line of digital showers and bathfills from Mira Showers. They
work great in my experience, but having only a Bluetooth Low Energy (BLE)
interface, they can only be controlled locally via smartphone and not via
Alexa, Google Home and the likes, which makes the whole experience
significantly less useful.

## Alexa, turn on the shower!

To overcome this limitation, here's a Python library that can be used to
control Mira Mode devices from a Raspberry PI or any other computer equipped
with BLE capabilities, allowing an easy integration with projects like
Home Assistant which can then expose the shower outlets as switches to
Alexa or Google Home.

This project contains a CLI utility to perform operations on the shower and
a Python library that can be used by other projects.

*Disclaimer: this projects contains only the results of personal experiments,
use at your own risk!*

## Requirements

1. Python 3.9 and above
2. A Bluetooth Low Energy (BLE) interface, already included in Raspberry Pi
devices, MacBooks and most laptops
3. On Linux, make sure the _bluez_ package is installed
4. Install this project:

```console
cd python-miramode

# Optionally use a Python virtual environment:
python3 -m venv venv
. venv/bin/activate

pip install .
```


## CLI Examples

First, obtain the address of your shower device. Look for devices with a name
starting with *Mira*:

```console
miramodecli devices-list
```

The device address has the "xx:xx:xx:xx:xx:xx" format on Linux and Windows,
and a UUID format on MacOS.

### Pair a new client

The _-c_ argument is used to pass the new client id. If not provided, a random
id will be generated. Take note of both the client id and the client slot
returned by the device, as those will be needed when sending other commands.
Do not forget to put the device in pairing mode by pressing the shower outlet
button for 5 seconds!

```console
miramodecli client-pair -a <address> -c <new_client_id>  -n <new_client_name>
```

### List existing clients:

The following command will list the existing clients, please note that it
requires a valid client id and its corresponding slot, obtained from a
previous pairing:

```console
client-list -a <address> -c <client_id> -s <client_slot>
```

### Unpair an existing client

```console
miramodecli client-unpair -a <address> -c <client_id> -s <client_slot> \
-u <client_id_to_unpair>
```

## Set debug logging level

For additional logging details, all commands support a _--debug_ argument, e.g:

```console
miramodecli client-pair -a xx:xx:xx:xx:xx:xx -c 100100  -n Foobar --debug
```

## Acknowledgements

Many thanks to Nigel Hannam for his excellent work in documenting the BLE
protocol: https://github.com/nhannam/shower-controller-documentation
