# hue-artnet: python Art-Net to Hue Entertainment converter

## Disclaimer

This is a personal project, just for fun. It is neither clean, nor efficient, nor guaranteed to work without tinkering.
Please be careful when using this to create flashing lights, as this could trigger epileptic symptoms.

## Prerequisites

- Python 3 (tested with 3.7.2)
- https://github.com/Mbed-TLS/mbedtls (not the older one from apt!)
- python-mbedtls (from pip)
- Hue bridge v2 with at least api version 1.22
- Up to 10 Hue Entertainment-compatible lights in one group

## Usage

Copy and adapt the config.json.
To get the required login credentials for your Hue bridge, you can use the login.py script.
Press the link button before running the script, and enter the bridge's IP to fetch a username/key pair.

Each Hue light can be configured onto any DMX channel block of one DMX universe. The mapping can be configured in the
`mapping` list. Each entry should consist of:

```json
{"start": 1, "light": 38, "fine":True}
```

- `start` denotes the lowest DMX channel this light will use
- `light` is the light ID in the Hue system
- `fine` will use two DMX channels per color (RRGGBB), coarse/fine. Optional, defaults to `False`

Hue lights will not support the full 16 bits, in my tests using only one channel per color was enough.

In normal mode, each light will use three DMX channels for the R,G,B, starting with R at the channel given by `start`.
DMX channels are numbered starting from 1.

To facilitate matching the light IDs, run the script without a mapping defined in the config (but valid credentials and
Entertainment group name). It will then cycle through the lights in the given group, and blink the corresponding light.

The script will wait for Art-Net packets to arrive on the configured address. If the stream stops after the first
packet was received, the script will shutdown and exit. You can also abort the script with Ctrl+C and it will try to
shutdown the Hue connection before exiting.

## Restrictions / Caveats

- Supports one Entertainment group on one bridge
- Supports one Art-Net Universe
