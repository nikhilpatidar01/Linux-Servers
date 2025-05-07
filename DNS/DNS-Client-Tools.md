# DNS Client Tools

### Install DNS Client Utilities

To use DNS tools like `dig`, `host`, and `nslookup` on RHEL-based systems, install the `bind-utils` package:

```bash
yum install bind-utils
```

To verify the contents of the installed package:

```bash
rpm -ql bind-utils
```

## `dig` – Domain Information Groper

`dig` is a flexible DNS lookup tool used to query information from DNS servers, such as A, MX, NS, and AAAA records.

* **Basic DNS lookup**

  ```bash
  dig example.com
  ```

* **Query a specific DNS server**

  ```bash
  dig example.com @8.8.8.8
  ```

* **Specify a DNS record type**

  ```bash
  dig example.com MX
  ```

* **Force use of IPv4**

  ```bash
  dig -4 example.com
  ```

* **Force use of IPv6**

  ```bash
  dig -6 example.com
  ```

* **Sort answer records**

  ```bash
  dig example.com +sort
  ```

* **Get a concise answer**

  ```bash
  dig example.com +short
  ```

* **Get AAAA (IPv6) record, suppress question section**

  ```bash
  dig example.com AAAA +noquestion
  ```

* **Get MX record only (short output)**

  ```bash
  dig example.com MX +short
  ```

* **Query for NS records using IPv4, suppress answer section**

  ```bash
  dig -4 example.com NS +noanswer
  ```

* **Explicitly specify record type with `-t` flag**

  ```bash
  dig -t A example.com +short
  dig -t AAAA example.com
  ```

* **Query all records**

  ```bash
  dig example.com ANY
  ```



## `host` – Simple DNS Lookup Utility

`host` is a straightforward command to perform DNS lookups.

* **Basic lookup**

  ```bash
  host example.com
  ```

* **Reverse lookup of an IP address**

  ```bash
  host 8.8.8.8
  ```

* **Query for a specific record type (e.g., A, MX, NS)**

  ```bash
  host -t A example.com
  host -t MX example.com
  ```

* **Display all available DNS records**

  ```bash
  host -a example.com
  ```

* **Force IPv4 resolution**

  ```bash
  host -4 example.com
  ```




## `nslookup` – Query Internet Name Servers

`nslookup` is another tool to query DNS records interactively or via command line.

* **Basic lookup**

  ```bash
  nslookup example.com
  ```

* **Query a specific DNS record type**

  ```bash
  nslookup -type=SOA example.com
  ```

* **Query using a specific DNS server**

  ```bash
  nslookup example.com 8.8.8.8
  ```