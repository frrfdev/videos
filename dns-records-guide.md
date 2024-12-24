# DNS Record Types: A Comprehensive Guide

## A Record (Address Record)

**Purpose**: Maps a domain name to an IPv4 address.

**Examples**:

```dns
example.com.     IN  A     192.0.2.1
blog.example.com IN  A     192.0.2.2
mail.example.com IN  A     192.0.2.3
```

**Use Cases**:

- Main website hosting
- Subdomains for different services
- Multiple A records for load balancing:
  ```dns
  example.com.   IN  A   192.0.2.1
  example.com.   IN  A   192.0.2.2
  ```

## AAAA Record (IPv6 Address Record)

**Purpose**: Maps a domain name to an IPv6 address.

**Examples**:

```dns
example.com.     IN  AAAA  2001:0db8:85a3:0000:0000:8a2e:0370:7334
ipv6.example.com IN  AAAA  2001:0db8:85a3:0000:0000:8a2e:0370:7335
```

**Use Cases**:

- IPv6-enabled websites
- Dual-stack hosting (both IPv4 and IPv6)
- Modern cloud services

## CNAME Record (Canonical Name)

**Purpose**: Creates an alias pointing to another domain name.

**Examples**:

```dns
www.example.com.    IN  CNAME  example.com.
blog.example.com.   IN  CNAME  example.github.io.
shop.example.com.   IN  CNAME  myshop.shopify.com.
```

**Use Cases**:

- WWW subdomain
- Third-party service integration
- CDN implementation
- Cannot be used on domain apex (root domain)

## MX Record (Mail Exchange)

**Purpose**: Specifies mail servers for receiving email.

**Examples**:

```dns
example.com.    IN  MX  10  primary-mail.example.com.
example.com.    IN  MX  20  backup-mail.example.com.
example.com.    IN  MX  30  last-resort-mail.example.com.
```

**Use Cases**:

- Email server configuration
- Google Workspace setup:
  ```dns
  example.com.    IN  MX  1   aspmx.l.google.com.
  example.com.    IN  MX  5   alt1.aspmx.l.google.com.
  example.com.    IN  MX  10  alt2.aspmx.l.google.com.
  ```
- Microsoft 365 setup
- Multiple servers with priorities (lower number = higher priority)

## TXT Record (Text)

**Purpose**: Stores text-based information for various purposes.

**Examples**:

```dns
example.com.    IN  TXT  "v=spf1 include:_spf.google.com ~all"
_dmarc          IN  TXT  "v=DMARC1; p=reject; rua=mailto:admin@example.com"
google._domainkey IN TXT "v=DKIM1; k=rsa; p=MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA..."
```

**Use Cases**:

- SPF records for email authentication
- DKIM verification
- Domain ownership verification
- DMARC policy specification
- Site verification for services

## NS Record (Name Server)

**Purpose**: Delegates a DNS zone to use the given authoritative nameservers.

**Examples**:

```dns
example.com.    IN  NS  ns1.nameserver.com.
example.com.    IN  NS  ns2.nameserver.com.
```

**Use Cases**:

- Domain nameserver specification
- DNS hosting setup
- Subdomain delegation:
  ```dns
  sub.example.com.   IN  NS  ns1.otherprovider.com.
  sub.example.com.   IN  NS  ns2.otherprovider.com.
  ```

## PTR Record (Pointer)

**Purpose**: Maps an IP address to a domain name (reverse DNS lookup).

**Examples**:

```dns
1.2.0.192.in-addr.arpa.    IN  PTR  example.com.
```

**Use Cases**:

- Email server verification
- Server identification
- Network troubleshooting

## SRV Record (Service)

**Purpose**: Specifies location of services.

**Examples**:

```dns
_sip._tcp.example.com.    IN  SRV  10  60  5060  sip1.example.com.
_xmpp._tcp.example.com.   IN  SRV  10  30  5222  chat.example.com.
```

Format: `_service._protocol.name. TTL class SRV priority weight port target`

**Use Cases**:

- VoIP services
- XMPP servers
- Microsoft Active Directory
- Minecraft servers

## CAA Record (Certification Authority Authorization)

**Purpose**: Specifies which Certificate Authorities (CAs) are allowed to issue certificates.

**Examples**:

```dns
example.com.    IN  CAA  0 issue "letsencrypt.org"
example.com.    IN  CAA  0 issuewild ";"
```

**Use Cases**:

- SSL/TLS certificate issuance control
- Security policy enforcement
- Preventing unauthorized certificate issuance

## Practical Examples

### Setting Up Google Workspace

```dns
# Mail handling
example.com.    IN  MX   1   aspmx.l.google.com.
example.com.    IN  MX   5   alt1.aspmx.l.google.com.

# SPF record
example.com.    IN  TXT  "v=spf1 include:_spf.google.com ~all"

# Domain verification
example.com.    IN  TXT  "google-site-verification=randomstring"
```

### Setting Up a Website with CDN

```dns
# Main website
example.com.        IN  A      192.0.2.1
www.example.com.    IN  CNAME  example.com.

# CDN configuration
cdn.example.com.    IN  CNAME  example.cdn-provider.com.

# Security
example.com.        IN  CAA    0 issue "letsencrypt.org"
```

### Setting Up Email Security

```dns
# SPF
example.com.    IN  TXT  "v=spf1 ip4:192.0.2.0/24 include:_spf.google.com -all"

# DKIM
mail._domainkey IN  TXT  "v=DKIM1; k=rsa; p=MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA..."

# DMARC
_dmarc          IN  TXT  "v=DMARC1; p=reject; rua=mailto:admin@example.com"
```

## Best Practices

1. **TTL Management**:

   - Use lower TTLs (300-900 seconds) when planning changes
   - Use higher TTLs (3600+ seconds) for stable records

2. **Security**:

   - Always implement SPF, DKIM, and DMARC
   - Use CAA records to control SSL issuance
   - Regularly audit DNS records

3. **Redundancy**:

   - Use multiple NS records
   - Configure backup MX servers
   - Consider secondary DNS providers

4. **Maintenance**:
   - Document all DNS records
   - Regular review and cleanup
   - Monitor DNS propagation after changes

## Practice Exercises

### Exercise 1: Basic Website Setup

You're setting up a new website "mycompany.com" with the following requirements:

- Main website IP: 203.0.113.10
- WWW subdomain should point to the main domain
- Email should be handled by Google Workspace
- The domain should only allow Let's Encrypt to issue SSL certificates

Try to write all necessary DNS records before checking the solution.

<details>
<summary>Click to see solution</summary>

```dns
# Main A record
mycompany.com.        IN  A      203.0.113.10

# WWW subdomain
www.mycompany.com.    IN  CNAME  mycompany.com.

# Google Workspace MX records
mycompany.com.        IN  MX     1  aspmx.l.google.com.
mycompany.com.        IN  MX     5  alt1.aspmx.l.google.com.
mycompany.com.        IN  MX     10 alt2.aspmx.l.google.com.

# SSL Certificate restriction
mycompany.com.        IN  CAA    0 issue "letsencrypt.org"
```

</details>

### Exercise 2: Email Security

Your domain "securemail.com" needs proper email security configuration. Set up:

- SPF to allow sending from your IP (198.51.100.5) and Google Workspace
- DKIM record for key "k=rsa; p=MIGfMA0GCSqGSIb3DQEBAQUAA4"
- DMARC policy to reject failed messages and send reports to admin@securemail.com

<details>
<summary>Click to see solution</summary>

```dns
# SPF Record
securemail.com.       IN  TXT    "v=spf1 ip4:198.51.100.5 include:_spf.google.com -all"

# DKIM Record
default._domainkey    IN  TXT    "v=DKIM1; k=rsa; p=MIGfMA0GCSqGSIb3DQEBAQUAA4"

# DMARC Record
_dmarc.securemail.com. IN  TXT    "v=DMARC1; p=reject; rua=mailto:admin@securemail.com"
```

</details>

### Exercise 3: Load Balanced Setup

You need to set up "loadbalanced.com" with:

- Three servers for load balancing (198.51.100.10, 198.51.100.11, 198.51.100.12)
- A development subdomain pointing to a separate server (198.51.100.20)
- Cloudflare CDN for the main domain
- Microsoft 365 for email

<details>
<summary>Click to see solution</summary>

```dns
# Load balanced A records
loadbalanced.com.     IN  A      198.51.100.10
loadbalanced.com.     IN  A      198.51.100.11
loadbalanced.com.     IN  A      198.51.100.12

# Development subdomain
dev.loadbalanced.com. IN  A      198.51.100.20

# Microsoft 365 MX records
loadbalanced.com.     IN  MX     0  loadbalanced-com.mail.protection.outlook.com.

# Cloudflare CNAME setup
www.loadbalanced.com. IN  CNAME  loadbalanced.com.cloudflare.net.
```

</details>

### Exercise 4: Service Configuration

Set up DNS for "gameserver.com" that needs:

- Minecraft server running on port 25565
- TeamSpeak server running on port 9987
- Website hosted on 203.0.113.20
- Wildcard subdomain for testing purposes

<details>
<summary>Click to see solution</summary>

```dns
# Main website
gameserver.com.       IN  A      203.0.113.20

# Minecraft SRV record
_minecraft._tcp.gameserver.com. IN  SRV  10  5  25565  gameserver.com.

# TeamSpeak SRV record
_ts3._udp.gameserver.com.      IN  SRV  10  5  9987   gameserver.com.

# Wildcard subdomain
*.gameserver.com.     IN  A      203.0.113.20
```

</details>

### Exercise 5: Troubleshooting

For each scenario, identify what's wrong and how to fix it:

1. Emails from your domain are being marked as spam
2. Your SSL certificate renewal keeps failing
3. Users can't connect to your Minecraft server
4. Your website is accessible via IP but not via domain name

<details>
<summary>Click to see solutions</summary>

1. Missing or incorrect email authentication records. Add/fix:

```dns
# Add proper SPF
domain.com.          IN  TXT    "v=spf1 ip4:YOUR_IP include:_spf.google.com -all"
# Add DKIM
default._domainkey   IN  TXT    "v=DKIM1; k=rsa; p=YOUR_KEY"
# Add DMARC
_dmarc               IN  TXT    "v=DMARC1; p=quarantine; rua=mailto:admin@domain.com"
```

2. Missing or incorrect CAA record. Add:

```dns
domain.com.          IN  CAA    0 issue "letsencrypt.org"
```

3. Incorrect or missing SRV record. Should be:

```dns
_minecraft._tcp.domain.com. IN  SRV  10  5  25565  domain.com.
```

4. Incorrect or missing A record. Should be:

```dns
domain.com.          IN  A      YOUR_SERVER_IP
www.domain.com.      IN  CNAME  domain.com.
```

</details>

## Challenge Exercise

Create a complete DNS configuration for a company that needs:

- Main website with www subdomain
- Blog hosted on Ghost Pro
- Email through Google Workspace
- Development and staging environments
- Full email security
- CDN integration
- SSL security restrictions

<details>
<summary>Click to see solution</summary>

```dns
# Main website
company.com.         IN  A      203.0.113.50
www.company.com.     IN  CNAME  company.com.

# Blog subdomain (Ghost Pro)
blog.company.com.    IN  CNAME  ghost.io.

# Development and staging
dev.company.com.     IN  A      203.0.113.51
staging.company.com. IN  A      203.0.113.52

# Google Workspace
company.com.         IN  MX     1   aspmx.l.google.com.
company.com.         IN  MX     5   alt1.aspmx.l.google.com.
company.com.         IN  MX     10  alt2.aspmx.l.google.com.

# Email security
company.com.         IN  TXT    "v=spf1 include:_spf.google.com -all"
google._domainkey    IN  TXT    "v=DKIM1; k=rsa; p=YOUR_DKIM_KEY"
_dmarc               IN  TXT    "v=DMARC1; p=reject; rua=mailto:admin@company.com"

# CDN
cdn.company.com.     IN  CNAME  company.cdn-provider.com.

# SSL security
company.com.         IN  CAA    0 issue "letsencrypt.org"
company.com.         IN  CAA    0 issuewild ";"

# DNS servers
company.com.         IN  NS     ns1.nameserver.com.
company.com.         IN  NS     ns2.nameserver.com.
```

</details>
