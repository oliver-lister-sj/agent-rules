---
name: manage-configs
description: Manage configurations in fe-service-desktop-configs, fe-worker-desktop-configs, and fe-customer-desktop-configs. Use this skill when adding new features, updating flags, or modifying language strings across apps.
disable-model-invocation: false
---

# Manage Configs

## When to Use

- Use this skill when you need to add or update configuration flags, language strings, or app settings.
- Use this when working on `fe-service-desktop`, `fe-worker-desktop`, or `fe-customer-desktop` and need to reflect changes in the centralized config repo.
- Use this to understand the hierarchy of base and override configurations.

## Instructions

### 1. Select the Repository

Determine which configuration repository to use based on the user's request or the current context.

- **Service**: `../fe-service-desktop-configs`
- **Worker**: `../fe-worker-desktop-configs`
- **Customer**: `../fe-customer-desktop-configs`

If the repository is not specified, ask the user or check for the existence of the directories.

1.  Verify the repository exists (example for service): `ls -F ../fe-service-desktop-configs`
2.  If it doesn't exist, ask the user for the correct path.

### 2. Determine the Scope of Change

Decide where to make the change based on its scope:

- **Global (All Partners/Envs)**: Edit `base/{app}/{config|flags|lang}.json`.
- **Partner-Specific (All Envs)**: Edit `overrides/{partner}/shared/{app}/{config|flags|lang}.json`.
- **Environment-Specific**: Edit `overrides/{partner}/{env}/{app}/{config|flags|lang}.json`.
- **Shared Across Apps**: Edit `base/shared/{config|flags|lang}.json` or `base/shared/{app1_app2}/{config|flags|lang}.json`.

**Note:** `config.json` is for general settings, `flags.json` for feature flags, and `lang.json` for localization.

### 3. Best Practices

- **New Flags**: Always default new flags to `false` in the `base` configuration. Override them to `true` per partner or environment as needed.
- **Partner-Specific Configs**: If a config value should only be used for a specific partner, initialize it as an empty string `""` in the `base` configuration. Then, override it in the specific partner's configuration file.

### 4. Partners and Environments

Partners are defined by directories in `overrides/`. Common partners include:

- `sj` (SwipeJobs)
- `rs` (Randstad)
- `tb` (TrueBlue)
- `sjuk`, `rsuk`, `rsca` (International variants)

Environments are subdirectories within partners (e.g., `dev`, `preprod`, `staging`, `preview`).
Check `Makefile` for the full list of valid `ENVIRONMENTS` (e.g., `sj-dev`, `rs-preprod`).

**Shared Environment**:
There is a special `shared` directory within each partner folder (e.g., `overrides/sj/shared/`). Configurations placed here apply to **all** environments for that partner (dev, preprod, staging, preview, etc.). This is the preferred place for partner-level defaults that shouldn't vary between non-prod environments.

### 5. How Configs Are Generated

In the main core repositories (e.g., `fe-service-desktop-core`), the application start process triggers a process that generates the configurations.

1.  The start script runs a watcher (via `configs-fast` script).
2.  This watcher uses `@swipejobs/fe-core-library` to monitor changes in the `base` and `overrides` directories of the config repo.
3.  When a file changes, it merges the configurations in the following priority (lowest to highest):
    1.  `base/{app}/{configFile}.json`
    2.  `overrides/{partner}/{env}/{configFile}.json` (All Apps Env Override)
    3.  `overrides/{partner}/shared/{app}/{configFile}.json` (App Partner Shared Override)
    4.  `overrides/{partner}/{env}/{app}/{configFile}.json` (App Env Override)
4.  The merged result is written to the `configurations/` directory in the config repo.

### 6. Make the Update

1.  Read the target file to understand the current structure.
2.  Add or update the key/value pair.
3.  Ensure valid JSON syntax.
4.  If adding a new app, ensure you execute the `add-apps` script first.

### 7. Validate Changes

1.  Run `make test` in the selected configuration directory to validate schemas.
    ```bash
    cd ../fe-service-desktop-configs && make test
    # OR
    cd ../fe-worker-desktop-configs && make test
    # OR
    cd ../fe-customer-desktop-configs && make test
    ```
2.  **Important**: The `configurations/` folder is the "output". While the local watcher in the core repo updates it automatically during development, you should ensure your changes are valid and that `configurations/` is updated (if you are not running the core app) before committing.

## Usage Examples

### Adding a Feature Flag globally (Service)

```bash
# Check if base flags exist
ls ../fe-service-desktop-configs/base/orders/flags.json

# Read file
read_file ../fe-service-desktop-configs/base/orders/flags.json

# Add flag "enableNewFeature": false (default to false)
```

### Overriding a Config for a Partner (Worker)

```bash
# Check if partner override exists
ls ../fe-worker-desktop-configs/overrides/rs/shared/profile/config.json

# If not, create it (copy from base or start empty)
# Then add override
```

## Additional Resources

- `README.md` in the respective config repo.
- `Makefile` contains test commands and environment definitions.
