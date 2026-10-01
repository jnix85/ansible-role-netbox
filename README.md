# ansible-role-netbox

Deploys a NetBox (netbox-docker) active/standby pair with scheduled
pg_dump/media sync to the standby and a manual failover script.

Extracted from the `proxmox-ansible-roles` monorepo (`roles/netbox`, played
by `playbooks/50-netbox.yml`) into a standalone role directory. This is a
copy, not a symlink — the original still lives in `proxmox-ansible-roles`
and is the version actually run against production.

## Layout

```
.
├── ansible.cfg
├── requirements.yml
├── playbooks/site.yml          # play applying the role
├── inventory/
│   ├── hosts.yml                 # netbox_servers group (netbox1, netbox2)
│   ├── host_vars/{netbox1,netbox2}.yml
│   └── group_vars/
│       ├── all/                  # domain, vips.netbox, net_prefix, secrets toggle
│       └── netbox_servers/       # role vars + secrets.yml + vault.yml
└── roles/
    └── netbox/                   # the role itself
```

## Shared inventory

`domain`, `vips`, `net_prefix`, `vm_fqdn`, `secrets_backend`, and the
Infisical connection vars come from `../shared-inventory` (see
`ansible.cfg`'s `inventory` list) instead of being duplicated here — see
`../shared-inventory/README.md` for how that's wired and what does/doesn't
belong there.

## Dependencies not vendored here

`playbooks/site.yml` also runs `docker`, `acme_cert`, and `keepalived` —
sibling roles from the source monorepo. `keepalived` has its own standalone
copy at `../ansible-role-keepalived`; `docker`/`acme_cert` were not copied.
Supply them yourself before running the playbook, or strip them and
pre-bake the equivalent.

## Secrets

`inventory/group_vars/netbox_servers/vault.yml` is ansible-vault encrypted
(copied as-is — safe to keep encrypted at rest, but you'll need the
original vault password to decrypt it, or to re-key it with your own).
`secrets_backend` in `group_vars/all/secrets.yml` toggles between that vault
and Infisical (`ansible-role-infisical`) lookups.
