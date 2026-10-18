# Fairwell Heaven — MSDoors Addon

Fairwell is packaged as a native MSDoors addon. The installed entry point stays at `addons/Fairwell.txt`.

## Repository layout

- `addons/Fairwell.txt` — native addon entry point; builds the standard MSDoors `Elements` list.
- `Fairwell_Content.txt` — runtime source loader used by the entry point.
- `src/features/` — ordered source fragments for gameplay features:
  - `01-communicator-and-stem.txt`
  - `02-anti-creak.txt`
  - `03-anti-groundskeeper.txt`
  - `04-auto-closet.txt`
  - `05-delete-gate.txt`
  - `06-friendly-entities.txt`
  - `07-item-notifications-and-floor-detection.txt`
  - `08-auto-giggle-glowstick.txt`
  - `09-auto-open-locked-doors.txt`
- `src/config/config-manager.txt` — config save/load/override and auto-load behavior.

## Why the source files are fragments

The existing features share one Lua builder scope and contain local callbacks and registries. To reduce regression risk, the feature files are concatenated in the order listed by `Fairwell_Content.txt` before compilation. Keep that order stable and preserve the beginning/end of the complete source when editing. These files are source fragments, not independent Lua modules.

## Adding or changing a feature

1. Edit the appropriate feature fragment under `src/features/`.
2. If adding a new fragment, add its raw GitHub URL to `MODULE_URLS` in `Fairwell_Content.txt` at the correct source-order position.
3. Keep the config manager after the feature code so it can see all controls and registries.
4. Update the cache key in `addons/Fairwell.txt` when a release needs to bypass a cached loader.
5. Test the addon in MSDoors and check the executor console for download/compile errors.

## Installation

Install `addons/Fairwell.txt` into `msdoors/addons/Fairwell.txt`. The loader downloads the source files from this repository's `main` branch, so changes become live once merged to `main`.
