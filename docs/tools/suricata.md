# SURICATA - IDS/IPS

**Suricata** is a high performance Network Intrusion Detection System ([NIDS](https://www.paloaltonetworks.com/cyberpedia/what-is-an-intrusion-detection-system-ids)), Intrusion Prevention System and Network Security Monitering Engine which is open-source in nature.

**Category:** [IDS/IPS](tools/index?id=idp/ips)

## Installation

It is assumed that you are using the most recent version of Ubuntu or any linux kernal before proceeding. To installl suricata use the following command.

```bash
($) sudo apt install software-properties-common
($) sudo apt update
($) sudo apt install suricata jq
```

**jq** tool is recommended to instalkl with suricata as it will enablke viewing EVE JSON output in the terminal.

To check if suricata is installed use one of the following commands to see version or build information.

```bash
($) suricata -V
($) suricata --build-info
```

To uninstall suricata, simple use the following command.

```bash
($) sudo apt uninstall suricata jq
```

## Usage

Before doing anything else, please note that the configuration files for suricata are located at `/etc/suricata` and logs for the same are stored at `/var/log/suricata`.

The folder structure of suricata is as follows,

```text
/etc/suricata/
    classification.config       # Maps "rule classification" to short description and priority
    reference.config            # Maps "external reference systems" to URL.
    rules/*                     # IDS and IPS rules
    suricata.yml                # Main configuration file
    threshold.config            # Put limits on alerts.
```

Now to begin we need to load the latest rules from trusted sources for detecting network threats. To do this we use the following command and then restart the suricata.

```bash
($) sudo suricata-update
($) sudo systemctl restart suricata
```

To meddel with the sources, you can use one for the commads.

```bash
($) sudo suricata-update list-sources                               # List all sources
($) sudo suricata-update enable-source example/rule                 # Enable source
($) sudo suricata-update disable-source example/rule                # Disable source
($) sudo suricata-update add-source myruleset https://example.com   # Add new source
```

Please Note that suricata running as root is vulnerabile as it processes untrusted network data. Thus consider reviewing this link ([Security Consideration](https://docs.suricata.io/en/latest/security.html))

Now to start suricata, you must run it as a Daemon. Running it in console will keep the console occupied. Thus use the following command.

```bash
($) suricata -D
```

## Writing Custom Rules

Now lets move to writing your own rules. It is recommended and also safe to write your own rules in a seperate file and not modidy any rules files to prevent them being overwritten by suricata while update. Follow the given steps to achieve the same.

- Navigate to `/etc/suricata/rules`

```bash
($) cd /etc/suricata/rules
```

- Create your own rules file. Follow a general format to quickly identify them later. Here I will use `local_XXXX.rules`.

```bash
($) touch local_test.rules
($) touch local_test1.rules
($) ...
```

- Write your rules in to the newly created file in the given format. (More on Rules found [here](#rules))

```text
<action> <protocol> <src_ip> <src_port> -> <dst_ip> <dst_port> (options)
```

- Include your rules file in the `suricata.yaml` file in the `rule-files` section. It should look something like this,

```text
rule-files:
  - suricata.rules
  - /path/to/your/local_test.rules
  - /path/to/your/local_test1.rules
```

- To check if you have include the files or not you can use the following commad.

```bash
($) grep -A 10 "rule-files" suricata.yaml
```

- Now validate your configs before restarting using the following command.

```bash
($) sudo suricata -T -c /etc/suricata/suricata.yaml -v
```

- Final step is to restart the suricata to apply the rules.

## Rules

Finally we will learn about writing rules in suricata. It is to be noted that the format of rules acceptable by suricata is as follow.

```text
<action> <protocol> <src_ip> <src_port> -> <dst_ip> <dst_port> (options)
```

While this is only a brief notes about rules, you can view the whole documentation on rules [here](https://docs.suricata.io/en/latest/rules/index.html)

- **action:** determins what heppends on rule match. Possible values are as follows,
  - `alert` - Generate an alert and allow.
  - `pass` - Stop further inspection.
  - `drop` - Drop packet and generate alert.
  - `reject` - Send ICMP unreachable error to sender. Drop the packet.
  - `rejectsrc` - Same as above. Drop the packet.
  - `rejectdst` - Send ICMP error to receiver. Drop the packet.
  - `rejectboth` - Send ICMP error to both sender and receiver. Drop the packet.
- **protocol:** defines the type of packet it will affect. Possible values but are not limited to the following,
  - `tcp` - For any TCP traffic
  - `udp` - For UDP packets
  - `icmp` - For ICMP errors
  - `ip` - Any packet in general (stands for "all" or "any")
  - `http`, `ftp`, `ssh`, `dns`, `smtp`, etc.
- **(src, dst) ip and port:** defines the flow of traffic that should be inspected (inward or outward).
  - `src_ip` - Origin IP can be "192.0.0.1" or $ or $HOME_NET or "$EXTERNAL_NET" or "any".
  - `src_port` - Origin Port. It can be 80, 443, "any".
  - `dst_ip` - Destination IP.
  - `dst_port` - Destination Port.
- **option:** visit the official documentation as it depends on case by case basis. [Click here](https://docs.suricata.io/en/latest/rules/index.html) for official rules document.

## References

- [Website - Suricata](https://suricata.io/)
- [Documenetation - Suricata](https://docs.suricata.io/en/latest/index.html)
