#   Run wireguard-go as systemd service

##  Why

While WireGuard module has long been included in Linux kernel,
not every system has it.
My recent encounter is with [Niagahoster VPS](https://www.niagahoster.co.id/cloud-vps-hosting),
which actually is a system container,
may be LXD,
and not HVM.
Other system may find this useful,
besides LXD,
is probably LX-branded zone in SmartOS or OmniOS.

##  How to install in Debian 11

1.  Enable backports,

    ```bash
    echo 'deb http://deb.debian.org/debian bullseye-backports main' >> /etc/apt/sources.list
    apt update
    ```

2.  Install dependencies,

    ```bash
    apt install --no-install-recommends wireguard-go wireguard-tools
    ```

3.  Copy scripts into WireGuard directory, and make sure they're executable,

    ```bash
    cp wg-if-* /etc/wireguard
    chmod 700  /etc/wireguard/wg-if-*
    ```

4.  Copy the systemd service file into place,

    ```bash
    cp wireguard-go.service /etc/systemd/system
    ```

5.  Lastly, enable the service,

    ```bash
    systemctl enable --now wireguard-go.service
    ```

##  Limitation

Currently, the only wg-quick additional config implemented is `Address`.

