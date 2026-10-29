---
name: flutter-perf-images-and-assets
description: >
  Audits and fixes Flutter image and asset performance: oversized source
  images, images decoded far larger than displayed, cacheWidth/cacheHeight,
  resolution-aware asset variants (arbitrary densities, not just 1x/2x/3x),
  duplicate density variants, and choosing SVG/vector vs raster for logos, icons,
  illustrations, photos and thumbnails. Use for high memory on image-heavy
  screens, slow scrolling with images, or large app assets. Do not convert
  assets just because another format could be smaller.
---

# Images and Assets

## 1. Detect

- Flutter Inspector oversized-image diagnostics (where supported) flags images decoded larger than their display size.
- DevTools memory view in profile mode for image cache growth.
- Source dimensions and file sizes in `assets/`; compare to the largest size actually rendered.
- Images inside scrolling lists: decoded repeatedly and at full size?

## 2. Fix oversized decoding

Order of preference:

1. **Resize the source asset** to a sensible maximum for its use (preferred when the asset is under your control).
2. **`cacheWidth` / `cacheHeight`** on `Image`/`Image.network`/`ResizeImage` to decode near display size, accounting for device pixel ratio. Set one dimension to keep aspect ratio.

Do not set cache sizes smaller than the displayed pixel size (blurry) and be careful where the same image is shown at several sizes.

## 3. Resolution-aware assets

Flutter picks the closest variant for the device pixel ratio from density-named folders (e.g. `assets/2.0x/logo.png`, `assets/3.5x/logo.png`).

- Do **not** assume or enforce exactly 1x, 2x, 3x. Flutter supports additional densities.
- Remove variants that are duplicates with no visual or size benefit; add a variant only where the one-size-fits-all asset is demonstrably too large or too soft.
- Keep the declared asset path and variant folders consistent with `pubspec.yaml` conventions in the Flutter assets docs.

## 4. Choose the right asset type

| Asset | Evaluate |
|---|---|
| Logos, simple illustrations | Vector/SVG suitability |
| Icons | Flutter icon fonts or vector implementation |
| Gradients | Native Flutter rendering (`LinearGradient`) rather than raster gradient images |
| Photos | Appropriately sized raster |
| Thumbnails | Decode at thumbnail size |
| Complex artwork | Decide from its actual characteristics |

Converting only because another format is "theoretically smaller" is not a reason. Any SVG approach that adds a dependency needs justification under the no-unnecessary-dependencies rule in [guardrails](../flutter-performance-specialist/references/guardrails.md). Visual output must stay materially unchanged.

## 5. Verify

Before/after: peak image memory, decode sizes in the Inspector, scroll jank on image-heavy lists, and a visual side-by-side at target densities. Method: [flutter-perf-profiling-and-validation](../flutter-perf-profiling-and-validation/SKILL.md).

## Hard rules

- Act only on what the user asked. Before answering or changing anything, open the relevant files and check what is already done; report only what you checked, never guess, and do not redo finished work. Reason: guesses and repeated work waste the user's credit.
