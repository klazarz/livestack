# Known rationale gaps and documentation conflicts

These entries separate observed behavior from unknown intent. Check current code and tests before changing behavior; obtain historical context before presenting an unknown rationale as fact.

## Unresolved rationale

- Gravitino and related Iceberg components are staged/built into the deployment path, but the repository does not establish the original rationale for baking them into the custom image or the compatibility trade-offs behind that choice.
- The repository defines a local Oracle Free app database and a separate wallet-backed ADB integration. The historical reasoning for that split and the intended ownership of each dataset are not fully recorded in current docs.
- `AIHUB` is fixed from the custom-image build input, while other settings can come from OCI metadata at boot. The original operational/product rationale for this distinction is not documented.
- Image cleanup preserves named offline images, installer archives, and dependency volumes. The exact provenance and compatibility rationale for each retained artifact are not recorded in one authoritative place.
- The intended policy for `PG_AI_PROFILE_AUTO_SETUP` across standalone and Terraform deployments needs confirmation against deployment history; current code and guides disagree on the absent-value default.
- The intended role and current validity of the pre-authenticated Object Storage URLs present in `.env.example` are unknown. Their values are intentionally omitted here; do not copy them into shared knowledge.
- The local `peakgear_medallion` project is created separately from the existing `peakgear` project, and the setup README says its workflows are not run there. The historical reason and expected future relationship between the projects are not documented.

## Verified documentation/source mismatches

- `DESIGN.md` describes an `IcebergCatalogServerGuide.jsx` page and `?page=iceberg-catalog-server` route. The referenced page is absent, and `tests/test-public-endpoint-urls.sh` asserts that the route remains absent. The history behind its removal is unknown.
- `README.md` labels a command as “Stop service” but invokes `systemctl --user restart user-podman`; the command does not match the label.
- `terraform-readme.md` says an absent `PG_AI_PROFILE_AUTO_SETUP` defaults to `true`; `init/variable.sh` falls back to `false` when metadata is unavailable and no environment value is set. The Compose default is normally superseded by the generated environment file. Resolve the intended deployment policy before changing either source or guide.
- `README.md` describes `.env.example` as a secret-free template. The file warns against reusing LiveLabs URLs/credentials but currently contains pre-authenticated Object Storage URL values. Whether those values are active or revoked is unknown; treat the file as sensitive and do not publish its values.

## Historical conversations that would add value

Prioritize conversations that explain the gaps above: the local Oracle Free/ADB split and data ownership; Gravitino/Iceberg image and offline requirements; the `AIHUB` and AI profile setup defaults; the lifecycle/provenance of retained image artifacts; removal of the catalog-server UI route; and the separate `peakgear_medallion` project. Import only durable rationale after reconciling it with current source and tests.
