# Patched managed controller image for CVE-2026-111497 (libbrotli).
# Source: https://docs.cloudbees.com/docs/cloudbees-ci-kb/latest/client-and-managed-controllers/libbrotli-package-upgrade
# Tag: Prudential modern MM 2.555.1.36488 (Zendesk 279135)
FROM cloudbees/cloudbees-core-mm:2.555.1.36488 AS base

FROM registry.access.redhat.com/ubi9 AS build
COPY --from=base / /cbroot
RUN dnf update -y && \
  dnf install --installroot /cbroot -y --setopt=install_weak_deps=False libbrotli && \
  dnf clean --installroot /cbroot all

FROM base
COPY --from=build /cbroot/ .
