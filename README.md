#   Run wireguard-go as systemd service

##  Why?

While WireGuard module has long been included in Linux kernel,
not every system has it.
My recent encounter is with [Niagahoster VPS](https://www.niagahoster.co.id/cloud-vps-hosting),
which actually is a system container,
seems to be OpenVZ,
and not an HVM.
Luckily there's an official [WireGuard implementaion in Go](https://github.com/WireGuard/wireguard-go).
And we only need to run it as a systemd service for manageability.

Other systems which may find this useful
are probably Linux LXD and SmartOS or OmniOS LX-branded zones.

##  Install in Debian 11

1.  Make sure `tun` device is available,

    ```bash
    lsmod | grep tun
    ls /dev/net/tun
    ```

    In Niagahoster VPS, turn on the **TUN/TAP\ Adapter** switch in VPS config page.

2.  Enable backports,

    ```bash
    echo 'deb http://deb.debian.org/debian bullseye-backports main' >> /etc/apt/sources.list
    apt update
    ```

3.  Install only the main packages,

    ```bash
    apt install --no-install-recommends wireguard-go wireguard-tools
    ```

    Then generate private and public keys, and create config file with `.conf` suffix,
    as you would with vanilla WireGuard.
    Except, the only wg-quick additional config implemented is `Address`.

4.  Copy the scripts into WireGuard directory, make sure they are executable,

    ```bash
    cp wg-if-* /etc/wireguard/
    chmod 500  /etc/wireguard/wg-if-*
    ```

5.  Copy the service file into place,

    ```bash
    cp wireguard-go.service /etc/systemd/system/
    ```

6.  Lastly, enable the service,

    ```bash
    systemctl enable --now wireguard-go.service
    ```

##  Limitation

Currently, the only wg-quick additional config implemented is `Address`.

