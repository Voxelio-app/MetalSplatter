# Voxelio fork changes

This fork preserves the history and MIT license of
[`scier/MetalSplatter`](https://github.com/scier/MetalSplatter).

## Point-cloud reveal animation

Voxelio adds `SplatRenderer.SplatAnimation`, an optional per-frame animation
implemented in the existing Metal vertex-processing path. It does not mutate
or duplicate the source splat data.

The `pointCloudReveal` mode runs in two outward waves:

1. Splats appear as uniform screen-space points moving outward from a supplied
   world-space center.
2. Those points transition into their full Gaussian covariance and opacity.

```swift
renderer.animation = .pointCloudReveal(
    center: sceneCenter,
    radius: sceneRadius,
    progress: animationProgress
)
```

`progress` is clamped to `0...1`. The API also exposes feather width, point
scale, and point alpha controls. The default value is `.none`, so existing
MetalSplatter integrations retain their original rendering behavior.

The implementation changes are limited to:

- `MetalSplatter/Sources/SplatRenderer.swift`
- `MetalSplatter/Resources/ShaderCommon.h`
- `MetalSplatter/Resources/SplatProcessing.metal`

All other upstream targets, sample code, tests, and supported Apple platforms
remain available.

## License

The original project and Voxelio modifications remain licensed under the MIT
License. Copyright in Voxelio's modifications is held by Voxelio.
