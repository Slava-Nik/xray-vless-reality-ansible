# Xray VLESS + Reality (Ansible setup)

One-command deploy of Xray with VLESS + Reality on a VPS. Without a valid client key, the server shows your own nginx site. Happ `vless://` links are saved to `output/<server-ip>/` (3 clients by default: `client-1` … `client-3`).

## Checklist (before deploy)

1. **Domain** — buy at Namecheap (or any registrar).
2. **DNS** — A record for `@` → VPS IP (optional: `www` → same IP or CNAME to `@`).
3. **Site** — put static files in `sites/<your-domain>/` with `index.html` (see `sites/README.md`).
4. **SSH** — key access to the VPS (`ssh-copy-id root@YOUR_IP`).
5. **Inventory** — copy and edit hosts file:

```bash
cp inventory/hosts.yml.example inventory/hosts.yml
# set: ansible_host, ansible_ssh_private_key_file, vpn_domain
# vpn_domain must match the folder name under sites/
```

6. **Deploy** (limit to one host if inventory has several):

```bash
brew install ansible
ansible-galaxy collection install -r requirements.yml
ansible vps -m ping --limit indiegateway
ansible-playbook playbooks/setup-vless-reality.yml --limit indiegateway
```

The playbook checks `index.html` and that DNS A points at the VPS, then installs nginx + Let's Encrypt, configures Reality fallback to your site, and writes Happ links.

7. **Connect** — paste the `vless://` link from e.g. `output/<ip>/indiegateway-client-1-client-happ.txt` into Happ.

## Add another client

```bash
ansible-playbook playbooks/add-another-vless-client.yml -e client_name=john --limit indiegateway
```

Re-running with the same `client_name` only refreshes local files. Re-running `setup-vless-reality.yml` keeps existing clients in server credentials and only adds missing default `client-1`…`client-3` if needed.

## Requirements

- Ubuntu/Debian VPS with SSH key access
- macOS or Linux with Ansible (`dig` available locally for DNS checks)
- Domain A record already pointing at the VPS
