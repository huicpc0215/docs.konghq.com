---
title: Network & Firewall
---

# Network & Firewall

In this section you will find a summary about the recommended network and firewall settings for Kong.

## Ports

Kong listens on several ports that must allow external traffic and are by default:

* `8000` for proxying. This is where Kong listens for HTTP traffic. Be sure to change it to `80` once you go to production. See [proxy_listen].
* `8443` for proxying HTTPS traffic. Be sure to change it to `443` once you go to production. See [proxy_ssl_listen].
* `7946` which Kong uses for inter-nodes communication. Both UDP and TCP traffic should be allowed on it. See [cluster_listen].

Additionally, those ports are used internally and should be firewalled in  production usage:

* `8001` provides Kong's **Admin API** that you can use to operate Kong. See [admin_api_listen].
* `7373` used by Kong to communicate with the local clustering agent. See [cluster_listen_rpc].

## Firewall

Below are the recommended firewall settings:

* The upstream APIs behind Kong will be available on [proxy_listen][proxy_listen] and [proxy_listen_ssl][proxy_listen_ssl]. Configure these ports accordingly to the access level you wish to grant to the upstream APIs.
* **Protect** [admin_api_listen][admin_api_listen], and only allow trusted sources that can access the Admin API.
* Allow traffic on [cluster_listen][cluster_listen] port **only** between other Kong nodes. This port is used for infra-cluster communications.

## Network

Kong assumes a flat network topology in multi-datacenter setups. If you have a multi-datacenter setup, Kong nodes between the datacentes should communicate over a VPN connection.

Kong will try to auto-detect the node's first, non-loopback, IPv4 address and advertise this address to other Kong nodes. Sometimes this is not enough and the IP address needs to be manually set, you can do that by changing the `advertise` property in the [cluster][cluster] configuration.

[proxy_listen]: /{{page.kong_version}}/configuration/#proxy_listen
[proxy_listen_ssl]: /{{page.kong_version}}/configuration/#proxy_listen_ssl
[admin_api_listen]: /{{page.kong_version}}/configuration/#admin_api_listen
[cluster_listen]: /{{page.kong_version}}/configuration/#cluster_listen
[cluster_listen_rpc]: /{{page.kong_version}}/configuration/#cluster_listen_rpc
[cluster]: /{{page.kong_version}}/configuration/#cluster
