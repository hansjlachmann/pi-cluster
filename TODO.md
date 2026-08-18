# TODO

Operational follow-ups for the pi-cluster.

## Seafile — deploy memcached (stop seahub log spam)

**Problem:** `seahub_settings.py` is configured for a memcached cache at
`memcached:11211`, but no memcached is deployed. Every cache call fails with
`pylibmc.ServerDown`, which floods `seahub.log` (grew to ~98 MB active +
~5 rotated files ≈ 350 MB). It also caused a brief login 500 during setup
(the crash path was `cache.incr()` on a failed login).

**Interim cleanup (symptom only — logs regrow):** run on the Pi
```bash
kubectl -n seafile exec seafile-0 -c seafile -- sh -c '
  truncate -s 0 /shared/seafile/logs/seahub.log
  rm -f /shared/seafile/logs/seahub.log.[1-9]*
'
```

**Real fix (do this):** add a `memcached` Deployment + Service to
`apps/base/seafile/` named to match the existing `memcached:11211` config, so
**no seahub change is needed**. Add both files to
`apps/base/seafile/kustomization.yaml`, then reconcile. Suggested:
`memcached:1.6-alpine`, `args: ["-m","128"]`, Service `memcached` on port 11211
in namespace `seafile`. No pod restart required — pylibmc reconnects on the next
request once the Service resolves.

## Seafile — change the default admin password

`seafileadmin@example.com` was created from the chart's default credentials
(`secretpassword`) and the instance is publicly reachable at
`https://seafile.hansjlachmann.net/`. Change it via the web UI
(avatar → Settings → Password), or confirm it was already changed.
