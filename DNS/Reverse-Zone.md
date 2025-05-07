# Reverse DNS Zone Configuration

- Edit the zone configuration file:

    ```bash
    vim /etc/named.rfc1912.zones
    ```


- Add Reverse Zone Entry:

    ```bash
    zone "112.168.192.in-addr.arpa" IN {
        type master;
        file "reverse.armour.local";
        allow-update { none; };
    };
    ```


Here’s how the updated `/etc/named.rfc1912.zones` should look:

```bash
zone "localhost.localdomain" IN {
    type master;
    file "named.localhost";
    allow-update { none; };
};

zone "localhost" IN {
    type master;
    file "named.localhost";
    allow-update { none; };
};

zone "1.0.0.127.in-addr.arpa" IN {
    type master;
    file "named.loopback";
    allow-update { none; };
};

zone "0.in-addr.arpa" IN {
    type master;
    file "named.empty";
    allow-update { none; };
};

zone "armour.local" IN {
    type master;
    file "forward.armour.local";
    allow-update { none; };
};

zone "112.168.192.in-addr.arpa" IN {
    type master;
    file "reverse.armour.local";
    allow-update { none; };
};
```



## Create the Reverse Zone File

- Copy an Existing Zone File (Optional)

    ```bash
    cp -v /var/named/forward.armour.local /var/named/reverse.armour.local
    ```

- Edit the Reverse Zone File

    ```bash
    vim /var/named/reverse.armour.local
    ```

- Sample Configuration:

    ```bash
    $TTL 1D
    @       IN SOA  ns1.armour.local. root.armour.local. (
                                20250324    ; serial
                                3600        ; refresh
                                1800        ; retry
                                604800      ; expire
                                86400 )     ; minimum

    @       IN      NS      ns1.armour.local.

    145     IN      PTR     ns1.armour.local.
    145     IN      PTR     www.armour.local.
    200     IN      PTR     emp1.armour.local.
    201     IN      PTR     emp2.armour.local.
    205     IN      PTR     test.armour.local.
    210     IN      PTR     mail.armour.local.
    215     IN      PTR     db.armour.local.
    220     IN      PTR     vpn.armour.local.
    ```

> Ensure that IPs like `192.168.112.145` correspond to entries like `145 IN PTR ...`.



- Set Proper File Permissions

    ```bash
    chgrp named /var/named/reverse.armour.local
    chmod 640 /var/named/reverse.armour.local
    ```


- Restart DNS Service

    ```bash
    systemctl restart named
    ```


- Check BIND Config Files:

    ```bash
    named-checkconf
    named-checkconf /etc/named.conf
    named-checkconf /etc/named.rfc1912.zones
    ```

- Verify the Reverse Zone File:

    ```bash
    named-checkzone 112.168.192.in-addr.arpa /var/named/reverse.armour.local
    ```

- Expected Output:

    ```bash
    zone 112.168.192.in-addr.arpa/IN: loaded serial 20250324
    OK
    ```



- Test Reverse DNS Resolution

    ```bash
    dig -x 192.168.112.145
    dig -x 192.168.112.200
    dig -x 192.168.112.210
    ```