# proxy-gui

**[Nginx Proxy Manager](https://nginxproxymanager.com/) + Husarnet + Ansible**

Easy way to share services hosted on your Husarnet devices over proxy server with nice UI. 

## Installation

> This repo is powered by GitHub Actions and Ansible, so after providing proper GitHub secrets, everything needed to run the proxy-gui on your own server will be done automatically.

1. Click **[Use this template](https://github.com/husarnet/proxy-gui/generate)** button and create your own public or private copy of this repo
2. Prepare a new server with public IP address with clear installation of **Ubuntu 20.04** 
3. Setup SSH key pair **(with NO passphrase!!!)**:
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

| Repository Secret | Description |
| - | - |
| `PUBLIC_IP` | Public IP address of your VPS |
| `SSH_PRIVATE_KEY` | private SSH key (for root at your VPS) |
| `HUSARNET_JOINCODE` | Join Code from https://app.husarnet.com |

## Usage

Info on how to setup domains, issue SSL certificates and define proxy rules are in [this blog post](https://husarnet.com/blog/reverse-proxy-gui).

## Backup

If you want to save / restore settings of your Nginx Proxy Manager, go to `Actions` tab and manually trigger `Save Backup` or `Restore Backup` GitHub workflows. The backup will be stored as `.backup.tgz` in your GitHub repository.


