# Project-Compose-RustDesk-Server

This repository contains a Docker Compose configuration for deploying a **RustDesk self-hosted server** (`hbbs` and `hbbr`) alongside a **Tailscale container** for secure remote access.

It is designed for **public-facing access** by clients (via public IP and ports), while maintaining **private admin access** through the Tailscale network — ideal for both cloud (e.g., AWS EC2) and local setups.

---

## 📦 Services

- **`rustdesk-hbbs`** – RustDesk relay server (TCP/UDP)  
- **`rustdesk-hbbr`** – RustDesk rendezvous server (TCP)  
- **`tailscale`** – Tailscale container node for private admin access  

---

## 🧾 Prerequisites

- Docker and Docker Compose installed
- A Tailscale account with an [auth key](https://tailscale.com/kb/1085/auth-keys)
- Optional: a RustDesk server key (for secure client registration)
- A public IP address if you're hosting for external clients

---

## 🔧 Setup

### 1. Clone the Repository

```bash
git clone https://github.com/yourusername/Project-Compose-RustDesk-Server.git
cd Project-Compose-RustDesk-Server
````

### 2. Create the `.env` File

```bash
cp .env.example .env
```

Then edit `.env` to include your configuration:

```env
TAILSCALE_AUTHKEY=tskey-client-xxxxxxxxxxxxxxxxx
TAILSCALE_HOSTNAME=rustdesk-ts
RUSTDESK_KEY=_    # Use '_' if no key is required
```

### 3. Deploy the Stack

```bash
docker compose up -d
```

---

## 🌐 Access

### 🚀 Client Access (via Public IP)

RustDesk clients connect to your server through your **public IP**:

* `PublicIP:21115` (relay)
* `PublicIP:21116` (rendezvous TCP/UDP)
* `PublicIP:21117` (control)

➡️ Ensure these ports are **open in your firewall or router** (or cloud security group).

### 🔐 Admin Access (via Tailscale)

You can access the host system or manage containers using Tailscale via:

* The container’s Tailscale IP address
* Or MagicDNS: `rustdesk-ts.tailnet-YOURNAME.ts.net`

This is useful for:

* SSHing into the host
* Debugging without relying on exposed public ports
* Secure emergency recovery access

---

## 🔁 Redeploy or Update

To update RustDesk or Tailscale images:

```bash
docker compose pull
docker compose down
docker compose up -d
```

---

## 📁 Project Structure

```
Project-Compose-RustDesk-Server/
├── docker-compose.yml        # Main stack definition
├── .env.example              # Sample environment file
├── .env                      # Your actual config (not committed)
└── tailscale-state/          # Auto-created container state (do not delete)
```

---

## 🔐 Security Tips

* The stack exposes **public ports** — only expose those needed and secure your firewall
* Use a **restricted Tailscale auth key** with a tag (`tag:rustdesk`) for security
* Keep your `.env` file private and out of version control
* Restart the stack occasionally to refresh auth sessions and apply updates

---

## 🧠 License & Attribution

This project is maintained by [@jbledua](https://github.com/jbledua).
RustDesk is an open-source project: [https://rustdesk.com](https://rustdesk.com)
Get Started with tailscale at [https://tailscale.com/kb/1017/install](https://tailscale.com/kb/1017/install)

---

