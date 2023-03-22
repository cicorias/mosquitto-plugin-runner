# DPS Lite E2E Tests - Creating Test Certificates
Create chain and client certificates to run E2E tests. Note that the scripts here are modified versions of the ones provided in the guides below for IoT Hub testing:

- [Tutorial: Use Microsoft-supplied scripts to create test certificates](https://learn.microsoft.com/en-us/azure/iot-hub/tutorial-x509-scripts)
- The guides above will make use of [these **demo** scripts from Microsoft](https://github.com/Azure/azure-iot-sdk-c/tree/main/tools/CACertificates)

## Prerequisites
Make sure you fulfill all the prerequisites in [the main README.md file](../README.md#prerequisites).

## Creating Certificates

Run the following commands to generate test certificates:
```sh
chmod 700 certGen.sh
./certGen.sh create_root_and_intermediate
```

The commands above generate root and intermediate certificates and keys in the same chain. You can also use Microsoft demo scripts to generate leaf (device) certificates and keys. Simply run the same script with the `create_device_certificate_from_intermediate` subcommand and **one argument that provides the device ID**. That part isn't bolded by accident: The device ID must match the Common Name (CN) in the device certificate. The device ID will be used when registering the device to an MQTT Broker using DPS.

Let's create 3 certificates from the intermediate cert for our tests:
```sh
./certGen.sh create_device_certificate_from_intermediate testdevice
./certGen.sh create_device_certificate_from_intermediate testdevice2
./certGen.sh create_device_certificate_from_intermediate testdevice3
```

## Using Task
If you're up to speed on the concepts here, your life will be much easier by simply doing this:

Create chain (Root CA and Intermediate) certs
```sh
task chain
```

Create device certs
```sh
task device # uses default device name: testdevice
task device DEVICE=testdevice2
task device DEVICE=testdevice3
```

Note that simply running `task` will run the default task, which creates the chain and a default `testdevice` client cert.

### Cleanup
This destroys all generated files (they're gitignored anyway, but just in case you want clean slate)
```sh
task clean
```
