---
title: "GPS Localization"
date: 2022-05-09T14:24:56+05:30
weight: 10
---

{{% pageinfo color="warning" %}}
The content of this page has been marked **work in progress** by the core development team.

If you would like to contribute and help keeping this documentation up to date,
please fork [https://github.com/eclipse-leda/leda](https://github.com/eclipse-leda/leda/fork)
and create a pull request.

Check the Eclipse [Get Involved!](https://www.eclipse.org/contribute/) FAQ for pre-requisites
and the [Eclipse Leda Contribution Guidelines](https://eclipse-leda.github.io/leda/docs/project-info/contribution-guidelines/)
for general information.
{{% /pageinfo %}}

The **GPS Demo** use case shows how to integrate an existing source for location-based services into the *Vehicle Signal* tree.

## Setup

- Install `gpsd` container to provide API for vehicle applications to access GPS services
- Install `fakegps` container to simulate the positions usually provided by a GPS hardware device
- Install `gps2val` container for the integration into VSS databroker

### Installation of gpsd

`gpsd` requires access to a physical device (serial) to retrieve the actual positioning information.

There seem to be a couple of outdated Docker containers with gpsd

- https://hub.docker.com/r/forcedinductionz/docker-gpsd - Outdated (6 years)
- https://github.com/ianblenke/docker-gpsd - no pre-built image
- https://hub.docker.com/r/knowhowlab/gpsd-nmea-simulator/ - gpsd and NMEA1083. Outdated (6 years)
- https://hub.docker.com/r/gpsdevteam/bathurst-backend - no ARM64 images
- https://hub.docker.com/r/swwright/gpsd-mqtt - no description, no AMD64 images
- https://hub.docker.com/r/lotherk/gpsd - no description, but recent updated and AMD64+ARM64 images (half a year old)

1. Run leda, login with `root` (no password)

    ```shell
    docker run -it ghcr.io/eclipse-leda/leda-distro/leda-quickstart-x86
    ```

2. Deploy a gpsd container

    ```shell
    # Propagation modes: rprivate, private, rshared, shared, rslave, slave
    kanto-cm create --name gpsd --devices /dev/ttyUSB0 --mp="/var/run/gpsd.sock:/var/run/gpsd.sock:rprivate" --ports="2947:2947" docker.io/lotherk/gpsd:latest
    kanto-cm create --name gpsd --devices="/dev/ttyS0:/dev/ttyS0" --mp="/var/run/gpsd.sock:/var/run/gpsd.sock:rprivate" --ports="2947:2947" docker.io/lotherk/gpsd:latest
    ```

## References

- Eclipse Kuksa.VAL Feeder - gps2val: <https://github.com/eclipse/kuksa.val.feeders/tree/main/gps2val>