# Xray VLESS + Reality (Ansible setup)

One-command deploy of Xray with VLESS + Reality on a VPS. Client config is saved to `output/`.

## Quick start

```bash
# 1. Install
brew install ansible
ansible-galaxy collection install -r requirements.yml

# 2. Add your server
cp inventory/hosts.yml.example inventory/hosts.yml
# edit inventory/hosts.yml — IP, user, SSH key

# 3. Deploy
ansible vps -m ping
ansible-playbook playbooks/vless-reality.yml

# 4. Connect
# import output/my-vps-client-config.json into Nekoray / v2rayN / Hiddify / v2rayNG
```

## Requirements

- Ubuntu/Debian VPS with SSH key access (`ssh-copy-id root@YOUR_IP`)
- macOS or Linux with Ansible

