# Samples Readme

# 1. Python example models console

This script is a sample dbus client for meshd under BlueZ covering some pre-implemented mesh client models.

---

## Requirements

* BlueZ with meshd version for Junction2018
* python 3.x (tested on 3.6.5)

## Initialization

1. Plug-in HCI device to machine (not used by any other process - in down state)

2. Start meshd
    * have right privilages to dbus, "mesh.conf" in /etc/dbus-1/system.d/ (Fedora example)
    * have at least one node pre-configured in bluetooth storage, "node.json" in "/var/lib/bluetooth/mesh/0001"
    * now meshd interfaces, objects should be registered on dbus

3. Set up **token** in python script (line 38)

    Most important thing to make it working with Junction2018 meshd version is setting a right **token** while attaching to net.

    Example: node.json "deviceKey":"00764772234fbd76e3b40519d1d66a75" -> token = 0x76bd4f2372477600.

    Token is a half value of deviceKey in opposite byte ordering "00764772234fbd76"

## Usage

Run: python3 python_samples/example-models-console

If everything in initialized succesfully "**[example-models-console]$**" should appear.

### Supported main commands

* help - show help message with main commands and example for usage mesh modes send
* exit - required to exit from script thread. To definetely exit ctr-c or another script kill would be needed (mainloop is working in main thread).
* model - sends message via model

### model command

Every model send command requires parameters: element_index model_index opcode dst_addr key_index

1. element index:

    Integer element index e.g. default script has only one element: 0

2. model index:

    * 0x0000 for vendor client model
    * 0x1000 for generic on off client model
    * 0x1102 for sensor client model

3. opcode:

    * vendor client model:
        * 0xfbf105 for hello message
        * 0xfdf105 to get name from vendor server model (may be not merged PR #11505)
    * generic on off client model:
        * 0x8201 for get state
        * 0x8202 for set state
        * 0x8203 for set state without acknowledgement
    * sensor client model:
        * 0x8231 for sensor get status

4. destination address: node/group address to which model msg send is addressed. On reel board you'll get it once mesh will be started (after name set via characteristic write and disconnect)

5. key index: node config files has defined keys for applications. Should choose key binded to application.

* zephyr:

```
    static const u8_t app_key[16] = {
		0xaa, 0xaa, 0xaa, 0xaa, 0xaa, 0xaa, 0xaa, 0xaa,
		0xaa, 0xaa, 0xaa, 0xaa, 0xaa, 0xaa, 0xaa, 0xaa,
	};

	/* Add Application Key */
	bt_mesh_cfg_app_key_add(NET_IDX, addr, NET_IDX, APP_IDX, app_key, NULL);
```

* node.json (configuration for node):

```
  "appKeys":[
    {
      "index":0,
      "boundNetKey":0,
      "key":"aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"
    },
    {
      "index":1,
      "boundNetKey":0,
      "key":"63964771734fbd76e3b40519d1d94a49"
    },
    {
      "index":2,
      "boundNetKey":0,
      "key":"efb2255e6422d330088e09bb015ed789"
    }
  ],
```

---

## Sample usage

Hello message with name "User1"
`model 0 0x0000 0xfbf105 0x0898 0 User1`

Get name from board
`model 0 0x0000 0xfdf105 0x0898 0`
returns for vendor model: "My name is:  sample1" 

Get state of "back green LED"
`model 0 0x1000 0x8201 0x0898 0`
returns for generic on off model: "Got state: 0"

Set "back green LED" state to 0 (lit) with response
`model 0 0x1000 0x8202 0x0898 0 0 0`
returns for generic on off model: "Got state: 0"

Set "back green LED" state to 0 (lit) without response
`model 0 0x1000 0x8203 0x0898 0 0 0`

Get temperature sensor value
`model 0 0x1102 0x8231 0x2671 0 0x2a1f`
returns for sensor model: "Temperature in celcius = 27C"

## Hacking

...