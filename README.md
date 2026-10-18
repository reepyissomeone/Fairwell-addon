# Fairwell Heaven — MSDoors tab script

Fairwell is a **direct-running script**, not a native MSDoors addon. It adds a **Fairwell** tab to the existing MSDoors Obsidian window and keeps the feature controls in their existing groupboxes. It does not create a second window.

## Run it

1. Start MSDoors and wait for its existing UI to finish loading.
2. Run the contents of `Fairwell_AddTab.txt` in your supported script executor.
3. The loader locates the existing Obsidian window, adds the Fairwell tab, and loads `Fairwell_Content.txt`.
4. If loading fails, check the executor console for a specific `[Fairwell]` error.

## Source layout

- `Fairwell_AddTab.txt` — direct-running tab loader. It locates the existing MSDoors window and creates the Fairwell tab.
- `Fairwell_Content.txt` — downloads and compiles the ordered feature fragments.
- `src/features/` — existing feature fragments, plus `10-feature-connector.txt` for independently loaded new features.
- The feature connector lets new feature files return `function(tab, context)` and registers them in `FEATURE_MODULE_URLS` inside `10-feature-connector.txt`. It adds controls to the existing Fairwell tab and never creates a separate window or tab.
- `src/config/config-manager.txt` — config save/load and auto-load behavior.

## Important implementation notes

The feature files are source fragments, not independent modules. They share one Lua builder scope and must be concatenated in the existing order. Keep the config manager last so it can access the controls and registries created by the features.

The loader checks exposed globals first and then looks for the existing Obsidian window through executor GC inspection when available. It never intentionally creates a separate UI window. The Fairwell tab is guarded against duplicate creation in the same window.

## Updating features

1. Edit the appropriate fragment under `src/features/`.
2. If adding a fragment, add its raw GitHub URL to `MODULE_URLS` in `Fairwell_Content.txt` in the correct order.
3. Bump the content URL cache key in `Fairwell_AddTab.txt` after changing the loader/content contract.
4. Test in MSDoors and check the console for download, compile, or feature initialization errors.
