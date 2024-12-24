# Web Hosting Fundamentals

## HTTP/HTTPS Protocols

### HTTP (Hypertext Transfer Protocol)

- Request-response protocol between clients and servers
- Common HTTP methods:
  - GET: Retrieve data
  - POST: Submit data
  - PUT: Update existing resource
  - DELETE: Remove resource
- Status codes:
  - 2xx: Success (200 OK, 201 Created)
  - 3xx: Redirection
  - 4xx: Client errors (404 Not Found)
  - 5xx: Server errors

### HTTPS (HTTP Secure)

- Encrypted version of HTTP
- Uses TLS/SSL for security
- Protects against:
  - Man-in-the-middle attacks
  - Data tampering
  - Information theft

## DNS (Domain Name System)

### How DNS Works

1. User enters domain name (example.com)
2. DNS resolver queries root nameservers
3. Root servers direct to TLD (.com) servers
4. TLD servers direct to authoritative nameservers
5. Authoritative nameservers return IP address

### DNS Record Types

- A: Maps domain to IPv4 address
- AAAA: Maps domain to IPv6 address
- CNAME: Creates domain alias
- MX: Mail server records
- TXT: Text records (SPF, verification)
- NS: Nameserver records

## IP Addresses and Networking

### IP Addressing

- IPv4: 32-bit address (e.g., 192.168.1.1)
- IPv6: 128-bit address (e.g., 2001:0db8:85a3:0000:0000:8a2e:0370:7334)
- Public vs Private IP ranges
- Subnet masks and CIDR notation

### Ports

- Well-known ports (0-1023)
  - 80: HTTP
  - 443: HTTPS
  - 22: SSH
  - 21: FTP
- Registered ports (1024-49151)
- Dynamic ports (49152-65535)

## Web Servers

### Apache

- Configuration files location
- Virtual hosts setup
- Module system
- .htaccess files
- Common directives

### Nginx

- Configuration structure
- Server blocks (virtual hosts)
- Reverse proxy setup
- Load balancing
- Static file serving

### Configuration Examples

#### Basic Nginx Server Block

```nginx
server {
    listen 80;
    server_name example.com www.example.com;
    root /var/www/example.com;

    location / {
        try_files $uri $uri/ /index.html;
    }
}
```

#### Basic Apache Virtual Host

```apache
<VirtualHost *:80>
    ServerName example.com
    ServerAlias www.example.com
    DocumentRoot /var/www/example.com
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

## Static vs Dynamic Content

### Static Content

- HTML, CSS, JavaScript files
- Images, videos, documents
- Served directly by web server
- Faster delivery
- Easier to cache

### Dynamic Content

- Generated on-the-fly
- Requires server-side processing
- Database interactions
- User-specific content
- More resource-intensive

## Hands-on Practice Guide

### Setting Up Your First Web Server

1. Install Nginx:

   ```bash
   sudo apt update
   sudo apt install nginx
   ```

2. Basic configuration:

   ```bash
   sudo nano /etc/nginx/sites-available/default
   ```

3. Test configuration:

   ```bash
   sudo nginx -t
   ```

4. Restart service:
   ```bash
   sudo systemctl restart nginx
   ```

### Monitoring and Logs

- Access logs: `/var/log/nginx/access.log`
- Error logs: `/var/log/nginx/error.log`
- Monitor real-time logs:
  ```bash
  sudo tail -f /var/log/nginx/access.log
  ```

## Best Practices

1. Security

   - Keep software updated
   - Use HTTPS
   - Implement proper file permissions
   - Configure security headers

2. Performance

   - Enable compression
   - Configure caching
   - Optimize static file delivery
   - Use CDN for large files

3. Maintenance
   - Regular backups
   - Log rotation
   - Resource monitoring
   - Documentation

## Troubleshooting Common Issues

1. 502 Bad Gateway

   - Check if backend service is running
   - Verify proxy settings
   - Check logs for errors

2. 403 Forbidden

   - Check file permissions
   - Verify user/group ownership
   - Check SELinux/AppArmor settings

3. Performance Issues
   - Monitor resource usage
   - Check error logs
   - Analyze access patterns
   - Review configuration
