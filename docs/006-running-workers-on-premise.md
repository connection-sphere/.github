# Running Workers On-Premise

While **master** and **slave** nodes require high uptime, worker nodes can be offline for a few hours without affecting overall service deliverability.

Because of that, running workers on-premise is a good way to reduce infrastructure costs.

The challenge is that home/office machines are usually behind a router and are not publicly reachable.

This guide shows how to expose SSH securely through Cloudflare Tunnel, so you can manage your worker from anywhere.

For more information refer to [this article](https://github.com/leandrosardi/blackops/blob/main/doc/cloudflared.md).

## Prerequisites

1. A worker machine already installed and running (Ubuntu recommended).
2. A Cloudflare account with your domain configured.
3. Permission to create resources in Cloudflare Zero Trust.
4. SSH enabled on the worker (`sshd` listening on port `22`).
5. Your project repository available locally with the `saas` CLI.

## Setting Up Cloudflare Tunnel

1. Open Cloudflare Zero Trust.
2. Go to **Networks** -> **Tunnels**.
3. Create a new tunnel (Cloudflared connector).
4. Add a **Public Hostname** such as `w00a.connectionsphere.com`.
5. Set service type to **SSH** and destination to `localhost:22`.
6. Copy the tunnel connector token (`eyJ...`).

You will use that token in the next step to register your worker as a tunnel connector.

## Installing Tunnel in Your Worker Server

Use the existing BlackOps operation:

```bash
export OPSLIB=~/code1/secret/production && \
export CLOUDFLARE_TOKEN='eyJ...' && \
cd ~/code1/blackops/ops && \
saas source ./cloudflared.install.op \
  --node=w00a \
  --cloudflared_tunnel_token=$CLOUDFLARE_TOKEN
```

Notes:

- Replace `w00a` with your worker node name from `BlackOpsFile`.
- The token must be the connector token from Zero Trust Tunnel setup.
- If your tunnel route is configured correctly, the server will appear as connected in Cloudflare.

## Validating Tunnel Status

On the worker server:

```bash
sudo systemctl status cloudflared
sudo journalctl -u cloudflared -n 80 --no-pager
```

Expected result: service is `active (running)` and no authentication/token errors are shown.

## Connecting to Your Server From Anywhere

Install `cloudflared` on your local laptop/workstation (the client machine), then connect through ProxyCommand.

Ubuntu/Debian client example:

```bash
wget https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64.deb
sudo dpkg -i cloudflared-linux-amd64.deb
sudo apt-get install -f -y
```

SSH through Cloudflare:

```bash
ssh -o ProxyCommand="cloudflared access ssh --hostname %h" blackstack@w00a.connectionsphere.com
```

## Troubleshooting

1. `cloudflared` service is not running:
	- Check logs with `journalctl -u cloudflared -n 120 --no-pager`.
2. Hostname does not connect:
	- Verify tunnel Public Hostname points to `localhost:22` and type is SSH.
3. Authentication issues from local machine:
	- Ensure your Cloudflare Access policy allows your user identity.
