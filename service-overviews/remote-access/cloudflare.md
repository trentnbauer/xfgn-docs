# Cloudflare

Refer to the following documentation for creation and management of Cloudflare services

* [.](./ "mention")
* [external-access-to-systems.md](../../policies/external-access-to-systems.md "mention")
* [authentication-access-and-accounts.md](../../policies/authentication-access-and-accounts.md "mention")

## Zero Trust / Tunnels

[Link to App](https://www.cloudflare.com/en-au/products/tunnel/)

The Cloudflare tunnel is a port-forwarding-less reverse proxy. The traffic is tunneled through Cloudflare servers reducing the risk of DDOS  and other malicious attacks. Another advantage is that Cloudflare handles authentication, so its harder to brute force one of our internal services.



### Flowchart <a href="#bkmrk-flowchart" id="bkmrk-flowchart"></a>

<img src="../../.gitbook/assets/file.excalidraw (8).svg" alt="" class="gitbook-drawing">

### Tunnels

#### Internal

The Tunnels are hosted as Docker containers, on&#x20;

* Espresso
* Americano
* Fettuccine
* Lasagna

This is to allow for load balancing / fail over when required.

#### External

Cola tunnel is hosted on the [Vultr](../infrastructure/vultr.md) VPS 'Cola'

## WAF

<figure><img src="https://kb.xfgn.dev/uploads/images/gallery/2023-05/scaled-1680-/cloudflare.jpg" alt=""><figcaption></figcaption></figure>
