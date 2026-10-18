# Fairwell connected feature modules

New features can live in separate `.txt` files in this folder and be connected to the **existing Fairwell tab** through `src/features/10-feature-connector.txt`.

## Feature module template

Each connected feature file must return a function that receives the existing tab and a context table:

```lua
return function(tab, context)
    local box = tab:AddLeftGroupbox("My Feature", "puzzle")

    box:AddToggle("FairwellMyFeatureEnabled", {
        Text = "Enable my feature",
        Default = false,
        Callback = function(enabled)
            -- Feature behavior goes here.
        end
    })

    box:AddLabel({
        Text = "This control was added to the existing Fairwell tab.",
        DoesWrap = true
    })
end
```

Use the existing tab passed in as `tab`. **Do not call `AddTab`, create a new window, or make a separate ScreenGui for ordinary controls.**

## Connect a feature

1. Add the new module file under `src/new-features/`.
2. Make it return `function(tab, context) ... end`.
3. Add its raw GitHub URL to `FEATURE_MODULE_URLS` in `src/features/10-feature-connector.txt`.
4. Give the URL a cache version (for example `?v=1`) and bump it when updating that feature.
5. Test in MSDoors. The connector reports download, compile, and initialization failures separately so one bad feature does not stop the others.

The context currently includes:
- `context.Tab` — the same tab passed as `tab`.
- `context.Reload` — function that reloads the main Fairwell content.
- `context.HotelTab` — optional hotel tab reference, if provided by the loader.
- `context.Environment` — executor environment.

The connector catches each module's errors independently. Existing features remain in the original ordered source fragments and are not converted into standalone addon files.
