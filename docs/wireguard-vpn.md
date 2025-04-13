# WireGuard - Fast, Modern, Secure VPN Tunnel

WireGuard is extremely fast yet simple and modern VPN that make use of state-of-the-art cryptography. It is aimed to be faster, simpler, leaner and more useful that IPsec and other VPN protocols while avoiding the massive overhead. It is as easy as SSH to configure. A VPN connection can be made by simply just exchaning the public keys. The main reason behind it's popularity is first, it is open-source and second, it has a soruce code of around 3000 lines.

## Installation

To install wireguard on client, please visit the [official webpage](https://www.wireguard.com/install/) and download the installer from there. Please review the usage section once you complete installation part. It is always recomended to keep your installation of wireguard up-to-date as it will contains patch to newly discovered vulnerability.

## Usage

Here in this section, I will take you through steps on easily setting up your wireguard VPN and will mention commonly made mistakes. I will also try to include images when and where possible. Please Note: The following steps are for linux, however, they have been generalized upto such extent that it would be possible to use them on other operating systems.

### Server Configuration

#### Step 1 - Installation & Verification

Please install wireguard on your server system and verify it's installation using the following command.

```bash
wg version
```

If the above command throws an error, please check your installation again.

#### Step 2 - Generating Public and Private Keys

Now locate the wireguard installation folder on your system. Generally it can be found at `/etc/wireguard`. Once you locate that folder, navigate to that folder and then generate the publc private key pair using the following commands.

```bash
umask 077
wg genkey > privateKey
cat privateKey | wg pubkey > publicKey
```

The first command limits the file permission to 600 or less permissive. The second command generates a random private key and stores it to file. The thrid command takes in newly generated private key and creates a public key corresponding to that private key. The private key must be kept a secret at all times and never to be shared or transmitted over the internet. The public key is shared with the clients along with port and assigned address.

The `PresharedKey` is optional however, it significantly increases the security of your tunnel. Thus I recommend everyone to use it. It should be unique to each <server, client> pair. Use the following command to make generate preshared key.

```bash
wg genpsk > presharedKey
```

The above command generates a random private key and stores it to file.

#### Step 3 - Creating Configuration File

Create a file named `wg0.conf`, you can name it anything while keeping the extension intact. The wireguard configuration contains 2 sections, first section contains a single `Interface` which contains data about server, second section contains multiple `Peers` which represents client nodes or even other interfaces in some case. The `Interface` section contains private key, listening port and address. The following code shows the Interface section of the wireguard configuration file.

```bash
[Interface]
Address = 10.100.10.1/24            # Local private address
ListenPort = 51234                  # Listening port
PrivateKey = <private_key>          # Actual Private Key not file path
```

For each `Peer`, we will append the following piece of code to the configuration file.

```bash
[Peer]
PublicKey = <client-public-key>         # Public Key of client
PresharedKey = <preshared-key>          # Preshared key between server and client
AllowedIPs = 10.100.10.2/32             # Unique private IP range for each client
```

#### Step 4 - Enabling/Disabling Server

Once the config file has been created, you can you the following commands to enable your server.

```bash
wg-quick up wg0
```

If you wish to stop the wireguard server, you can simple use the following command.

```bash
wg-quick down wg0
```

#### Step 5 - Monitoring

To moniter or view the statistics of your WireGuard server, you can use the following command to view your peers, their last active connection and other metrics.

```bash
wg show
```

If you are running multiple wiregurad configurations, then to view the metrics indicidually, you can pass the name of the interface. For example, to view the metrics of interface `wg0`, we use the following command.

```bash
wg show wg0
```

### Client Configuration

For this specific example, we will consider, that each client will connect to a single server. If you configure for more than one server, it will still work, however, only the server with highest speed will be selected and all the traffic will be routed through that server.

#### Step 1 - Installation

Please follow all the steps and install WireGuard VPN on your system. Please refer to [official website](https://www.wireguard.com/install/)

#### Step 2 - Generating Client Public and Private Keys

Please note that if you are using an UI version then both public and private key pair will be generated for you. If not please continue reading. Locate the wireguard installation folder on your system. Generally it can be found at `/etc/wireguard`. Once you locate that folder, navigate to that folder and then generate the publc private key pair using the following commands.

```bash
umask 077
wg genkey > privateKey
cat privateKey | wg pubkey > publicKey
```

The first command limits the file permission to 600 or less permissive. The second command generates a random private key and stores it to file. The thrid command takes in newly generated private key and creates a public key corresponding to that private key. The private key must be kept a secret at all times and never to be shared or transmitted over the internet. The public key is shared with the clients along with port and assigned address.

The `PresharedKey` is optional however, it significantly increases the security of your tunnel. Thus I recommend everyone to use it. It should be unique to each <server, client> pair. Use the following command to make generate preshared key.

```bash
wg genpsk > presharedKey
```

The above command generates a random private key and stores it to file.

#### Step 3 - Adding Server Configurations

Create a file named `wg0.conf`, you can name it anything while keeping the extension intact. In the `Interface` section you will add details about the client and in the `Peer` section you will need to add details about the server.

```bash
[Interface]
Address = 10.100.10.1/24            # IP Assigned on the server configfile
PrivateKey = <private_key>          # Actual Private Key not file path
DNS = <dns-ip>                      # Specify DNS IP address to be used
```

For each `Peer`, we will append the following piece of code to the configuration file.

```bash
[Peer]
PublicKey = <server-public-key>         # Public Key of server
Endpoint = <server-ip-address>          # Public IP or domin name with port of server
PresharedKey = <preshared-key>          # Preshared key between server and client
AllowedIPs = 0.0.0.0/0, ::/0            # This ensures all the traffic is routed through tunnel
```

#### Step 4 - Connecting to / Disconnecting from Server

Once the config file has been created, you can you the following command to connect to the server.

```bash
wg-quick up wg0
```

If you wish to disconnet from the server you can you the following command.

```bash
wg-quick down wg0
```
