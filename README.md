# CloudBees CI libbrotli patch (`2.555.1.36488`)

Workaround images for **CVE-2026-111497** (UBI `libbrotli`) on CloudBees CI **modern** Operations Center and managed controller at version `2.555.1.36488`.

- Zendesk: [279135](https://cloudbees.zendesk.com/agent/tickets/279135)
- KB: [Upgrading libbrotli package for existing versions of CloudBees CI](https://docs.cloudbees.com/docs/cloudbees-ci-kb/latest/client-and-managed-controllers/libbrotli-package-upgrade)

These are **workaround** images (same product version, fixed `libbrotli-1.0.9-10.el9_8.1`). They are **not** an official CloudBees product release.

## Public images (GHCR)

```text
ghcr.io/csakala-art/cbci-libbrotli-patch-25551/cloudbees-cloud-core-oc:2.555.1.36488-libbrotli-patch
ghcr.io/csakala-art/cbci-libbrotli-patch-25551/cloudbees-core-mm:2.555.1.36488-libbrotli-patch
```

Platform: `linux/amd64`.

### Verify checksum

```bash
docker pull ghcr.io/csakala-art/cbci-libbrotli-patch-25551/cloudbees-cloud-core-oc:2.555.1.36488-libbrotli-patch
docker run --rm --platform linux/amd64 --entrypoint sha1sum \
  ghcr.io/csakala-art/cbci-libbrotli-patch-25551/cloudbees-cloud-core-oc:2.555.1.36488-libbrotli-patch \
  /usr/lib64/libbrotlienc.so.1.0.9
```

**Pass (amd64):** `be22a097e1301721a93631627fa9d47f12492a60`

Repeat for the managed controller image.

## Point CloudBees CI at these tags

**Operations Center (Helm)** — confirm keys with `helm show values` for your chart version:

```yaml
OperationsCenter:
  image:
    registry: "ghcr.io"
    repository: "csakala-art/cbci-libbrotli-patch-25551/cloudbees-cloud-core-oc"
    tag: "2.555.1.36488-libbrotli-patch"
```

**Managed controllers (preferred for existing controllers):** Manage Jenkins → Configure Controller Provisioning → add the full MM image reference above → select it → reprovision/restart.

**Helm controller image (chart shape like `Master.image`):**

```yaml
Master:
  image:
    registry: "ghcr.io"
    repository: "csakala-art/cbci-libbrotli-patch-25551/cloudbees-core-mm"
    tag: "2.555.1.36488-libbrotli-patch"
```

If you mirror through Artifactory, allow `ghcr.io/csakala-art/cbci-libbrotli-patch-25551/*` (or pull GHCR directly from the cluster). Public packages do not need a pull secret.

## Build

GitHub Actions on `main` / `workflow_dispatch` builds both Dockerfiles (`oc/`, `mm/`) for `linux/amd64` and pushes to GHCR.
