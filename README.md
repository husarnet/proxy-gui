# proxy-gui

**[Nginx Proxy Manager](https://nginxproxymanager.com/) + Husarnet + Ansible**

Easy way to share services hosted on your Husarnet devices over proxy server with nice UI. 

## Installation

1. Click **[Use this template](https://github.com/husarnet/proxy-gui/generate)** button and create your own public or private copy of this repo
2. Prepare a new server with public IP address with clear installation of **Ubuntu 20.04** 
3. Setup SSH key pair (with NO passphrase!!!):
  ```bash
  ssh-keygen -t ed25519 -C "user@example.com" -f ./ga-ssh-key
  ```
  That command will generate 2 files:
  | File | Target location | Procedure |
  | - | - | - |
  | `ga-ssh-key` | GitHub Secret | (in the GitHub repository) `Settings` > `Secrets` > `New repository secret` and create `SSH_PRIVATE_KEY` secret. Copy the `ga-ssh-key` file content here |
  | `ga-ssh-key.pub` | `/root/.ssh/authorized_keys` on your VPS | `cat ga-ssh-key.pub >> /root/.ssh/authorized_keys` |
4. Create `PUBLIC_IP` repository secret and place your server's public IPv4 address here
5. Get your **Husarnet Join Code** from app.husarnet.com, and create a new GitHub repository secret `HUSARNET_JOINCODE`.
6. Go to `Actions` tab and trigger the `Deploy to server` workflow

### GitHub repository secrets summary

| Repository secret | description |
| - | - |
| `PUBLIC_IP` | Public IP address of your VPS |
| `SSH_PRIVATE_KEY` | private SSH key (for root at your VPS) |
| `HUSARNET_JOINCODE` | Join Code from app.husarnet.com |

## Usage

### Adding a new revers proxy rule

Let's assume you have Raspberry Pi with `my-rpi` Husarnet hostname (with the same Husarnet network as this Nginx Proxy Manager instance) running wordpress on port `80`.

1. Go to `http://<your-vps-ip>:81` in your web browser and login with default username (`admin@example.com`) and password (`changeme`)

2 Go To **Hosts** -> **Proxy Hosts** tab

3. Click **[ Add Proxy Host ]** button and type:

    | field | value |
    | - | - |
    | Domain Names | `<your-vps-ip>` |
    | Scheme | `http` |
    | Forward Hostname / IP | `my-rpi` (or IPv6 addr in `[]`) |
    | Forward Port | `80` (port on VPS, not from RPi) |

    Place other tabs (`Custom locations`, `SSL`, `Advanced`) empty

    Click **[ Save button ]**

5. Visit `http://<your-vps-ip>` to see Wordpress website hosted by your Raspberry Pi


### Adding more locations under the URL

Let's assume you have a local development server running on port `3000` on your laptop (`my-latop` Husarnet hostname) and want to make it available under `http://<your-vps-ip>/demo-server` location:

Select `Proxy Host` and go to `Custom locations` tab:

| field | value |
| - | - |
| location | `/demo-server` |
| Scheme | `http` |
| Forward Hostname / IP | `my-latop/` |
| Forward Port | `3000` |

## Backup

If you want to save / restore settings of your Nginx Proxy Manager, go to `Actions` tab and manually trigger `Save Backup` or `Restore Backup` workflows. The backup will be stored as `.backup.tgz` in your GitHub repository.


