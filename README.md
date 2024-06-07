# Network-management-in-Linux
Network management in Linux involves configuring and managing network interfaces, routes, firewalls, and other network-related settings. Here’s an overview of some common tasks and tools used in Linux for network management:


Network management in Linux involves configuring and managing network interfaces, routes, firewalls, and other network-related settings. Here’s an overview of some common tasks and tools used in Linux for network management:

### 1. Checking Network Configuration
You can use various commands to check the current network configuration.

- **ifconfig**: Displays the configuration of network interfaces.
    ```sh
    ifconfig
    ```
  
- **ip**: A more modern and versatile tool than ifconfig.
    ```sh
    ip addr show
    ip route show
    ```

### 2. Configuring Network Interfaces
Network interfaces can be configured manually using `ifconfig` or `ip`.

- **ifconfig**:
    ```sh
    sudo ifconfig eth0 192.168.1.100 netmask 255.255.255.0 up
    ```

- **ip**:
    ```sh
    sudo ip addr add 192.168.1.100/24 dev eth0
    sudo ip link set eth0 up
    ```

### 3. Persistent Network Configuration
To make network configurations persistent across reboots, you need to edit configuration files.

- **Debian/Ubuntu**: Edit `/etc/network/interfaces` or use `netplan`.
    ```sh
    # /etc/network/interfaces
    auto eth0
    iface eth0 inet static
        address 192.168.1.100
        netmask 255.255.255.0
        gateway 192.168.1.1
    ```

    ```yaml
    # /etc/netplan/01-netcfg.yaml
    network:
      version: 2
      ethernets:
        eth0:
          dhcp4: no
          addresses: [192.168.1.100/24]
          gateway4: 192.168.1.1
          nameservers:
              addresses: [8.8.8.8, 8.8.4.4]
    ```

- **Red Hat/CentOS**: Edit files in `/etc/sysconfig/network-scripts/`.
    ```sh
    # /etc/sysconfig/network-scripts/ifcfg-eth0
    DEVICE=eth0
    BOOTPROTO=static
    IPADDR=192.168.1.100
    NETMASK=255.255.255.0
    GATEWAY=192.168.1.1
    ONBOOT=yes
    ```

### 4. Managing Routes
Routes can be added, deleted, and viewed using the `ip` command.

- **Adding a route**:
    ```sh
    sudo ip route add 192.168.2.0/24 via 192.168.1.1
    ```

- **Deleting a route**:
    ```sh
    sudo ip route del 192.168.2.0/24
    ```

### 5. DNS Configuration
DNS settings are typically configured in `/etc/resolv.conf` or via network manager tools.

```sh
# /etc/resolv.conf
nameserver 8.8.8.8
nameserver 8.8.4.4
```

### 6. Network Manager
Network Manager is a powerful tool that provides a graphical and command-line interface for managing network connections.

- **nmcli**: Command-line tool for Network Manager.
    ```sh
    nmcli device status
    nmcli connection show
    nmcli connection add type ethernet ifname eth0 con-name my-eth0
    nmcli connection up my-eth0
    ```

### 7. Firewall Configuration
Managing firewalls is crucial for network security. `iptables` and `firewalld` are commonly used tools.

- **iptables**:
    ```sh
    sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
    sudo iptables -A INPUT -j DROP
    sudo iptables-save > /etc/iptables/rules.v4
    ```

- **firewalld**:
    ```sh
    sudo firewall-cmd --zone=public --add-port=22/tcp --permanent
    sudo firewall-cmd --reload
    ```

### 8. Monitoring Network Traffic
Tools like `netstat`, `ss`, and `iftop` can be used to monitor network traffic.

- **netstat**:
    ```sh
    netstat -tuln
    ```

- **ss**:
    ```sh
    ss -tuln
    ```

- **iftop**:
    ```sh
    sudo iftop
    ```

### 9. Troubleshooting Network Issues
Use tools like `ping`, `traceroute`, and `tcpdump` to troubleshoot network issues.

- **ping**:
    ```sh
    ping google.com
    ```

- **traceroute**:
    ```sh
    traceroute google.com
    ```

- **tcpdump**:
    ```sh
    sudo tcpdump -i eth0
    ```

These tools and commands provide a robust set of capabilities for managing and troubleshooting networks on Linux systems.
