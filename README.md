# PiHole on Raspberry Pi with Podman & Ansible

This project deploys PiHole to a Raspberry Pi (or Ubuntu server) using Ansible and Podman (Docker Compose compatible).

## Prerequisites
- **Ansible** installed on your control machine.
- **SSH Access** to your Raspberry Pi/Ubuntu server.
- **Python** on the target machine (usually installed by default).

## Setup

1. **Clone the repository**:
   ```bash
   git clone <repository_url>
   cd <repository_name>
   ```

2. **Prepare Inventory**:
   Copy the sample inventory file and edit it with your server details.
   ```bash
   cp inventory/hosts.sample inventory/hosts
   nano inventory/hosts
   ```
   **Important**: Do not commit `inventory/hosts` to Git (it is ignored by `.gitignore`).

   Example `inventory/hosts` content:
   ```ini
   [pihole]
   192.168.1.100 ansible_user=ubuntu ansible_ssh_pass=secret
   # Or using SSH keys (preferred):
   # 192.168.1.100 ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/id_rsa
   ```

   **Option B: Environment Variables (Recommended)**
   You can skip creating an inventory file and define connection details in `env.sh`.

3. **Set Environment Variables**:
   Copy `env.sh.sample` to `env.sh` and configure your settings.
   ```bash
   cp env.sh.sample env.sh
   nano env.sh
   # Set TARGET_HOST, TARGET_USER, etc., and PIHOLE_PASSWORD
   source env.sh
   ```

4. **Configure PiHole (Optional)**:
   Modify defaults in `roles/pihole/templates/docker-compose.yml.j2` if needed (e.g., timezone, ports).

## Deployment

Run the playbook.

If using **Environment Variables**:
```bash
ansible-playbook site.yml
```

If using **Inventory File**:
```bash
ansible-playbook -i inventory/hosts site.yml
```


## Post-Deployment

Access your PiHole admin interface at:
`http://<your-pi-ip>/admin`

To view the password (if you didn't set one):
```bash
# SSH into your Pi
ssh ubuntu@<your-pi-ip>
cd ~/pihole
podman logs pihole 2>&1 | grep password
```
