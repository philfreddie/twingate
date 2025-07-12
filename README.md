# Twingate Private Access Setup

## Why Use Twingate?

Twingate provides secure remote access to private resources without the need for a traditional VPN.  
It’s fast, zero-trust, easy to set up, and ideal for internal services like self-hosted apps, admin panels, and dashboards.

## Where I Used Twingate

I deployed Twingate on my Raspberry Pi 3 alongside other services like Pi-hole and Tailscale.  
This allows me to access private services (e.g., internal dashboards, NAS, local web apps) from anywhere securely using my iPhone or laptop.

## Key Features I Use

- **Zero Trust Access** — Services are only accessible after device authentication and policy validation.  
- **Split Tunneling** — Only private traffic goes through Twingate; public traffic stays local.  
- **Mobile & Desktop Support** — I use the iPhone and macOS clients to connect.  
- **No Port Forwarding Needed** — My Pi remains hidden from the public internet.

## How I Set It Up

1. **Create a Free Twingate Account**  
   - Go to [https://twingate.com](https://twingate.com) and sign up.

2. **Define Your Network**  
   - Add resources (e.g., `192.168.1.1`, `pi.local`, etc.)  
   - Create remote access groups and configure access policies.

3. **Install Twingate Connector on Raspberry Pi**  
   Use Docker to deploy the connector:

   ```bash
   docker run -d \\
     --restart unless-stopped \\
     --name twingate-connector \\
     --network host \\
     -e TWINGATE_NETWORK=\"your-network\" \\
     -e TWINGATE_ACCESS_TOKEN=\"your-access-token\" \\
     -e TWINGATE_REFRESH_TOKEN=\"your-refresh-token\" \\
     -e TWINGATE_LABEL=\"raspberrypi\" \\
     twingate/connector:latest
   ```

4. **Install the Client on Your Devices**  
   - Download the Twingate client for iOS, Android, Windows, or macOS  
   - Log in using your organisation’s setup link

5. **Test Access**  
   - Verify access to internal IPs or services like `http://192.168.1.100:8000`

## Bonus: Running Alongside Pi-hole & Tailscale

- I run **Pi-hole** for DNS, **Tailscale** for VPN-like mesh, and **Twingate** for fine-grained access control.
- Twingate complements Tailscale well — I use one for full mesh access, the other for secure app-level access.
