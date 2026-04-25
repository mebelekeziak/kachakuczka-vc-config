# SolVolumetricClouds

Earth-only Blackrack/EVE raymarched volumetric cloud replacement for the Sol planet pack in quarter-scale mode.

This package is intentionally config-only. It does not include or copy Blackrack, Sol, RSSVE, NASA or other texture assets. It references textures that are already installed locally:

- `Sol-Textures/PluginData/03_Earth-System/03_Earth/EVE/EarthTemperate`
- `Sol-Textures/PluginData/03_Earth-System/03_Earth/EVE/EarthTropical`
- `Sol-Textures/PluginData/03_Earth-System/03_Earth/EVE/EarthCirrus`
- `StockVolumetricClouds/Clouds/Textures/PluginData/detail1`
- `StockVolumetricClouds/Clouds/Textures/PluginData/uvnoise1`

## Requirements

- Kerbal Space Program 1.12.x
- ModuleManager
- Kopernicus
- Sol planet pack with `SystemScale = Quarter`
- EnvironmentalVisualEnhancements / Blackrack volumetric clouds with `layerRaymarchedVolumeV5` support
- StockVolumetricClouds installed for shared detail/noise textures and sounds

## Install

Place the `SolVolumetricClouds` folder directly inside `GameData`:

```text
GameData/
  SolVolumetricClouds/
    SolVolumetricClouds.cfg
    README.md
```

Restart KSP after installing or editing the config so ModuleManager can rebuild its cache.

## What it changes

The ModuleManager cleanup runs only when Sol is set to `SystemScale = Quarter`. It removes these bundled Sol Earth cloud objects, plus any stale duplicate `SolVC-Earth-*` objects:

- `Earth-TropicalCumulus`
- `Earth-TemperateCumulus`
- `Earth-Cirrus`

It then adds three new V5 raymarched Earth layers:

- `SolVC-Earth-LowWeather`
- `SolVC-Earth-MidBrokenBands`
- `SolVC-Earth-HighCirrus`

It also raises the shared EVE raymarched-cloud quality preset from `temporalUpscaling = x32` to `x16`, with a higher light-volume resolution, to reduce blocky ray reconstruction artifacts.

For the sky itself, it enables Scatterer temporal anti-aliasing and prevents Scatterer from disabling TAA during short frame-rate dips. This reduces moving-camera grain in the atmosphere and cloud godray reconstruction.

Auroras, city lights, shadows, wet surfaces, Scatterer planet/atmosphere definitions, Parallax, Kopernicus and non-Earth planet cloud definitions are left untouched.

## Quick tuning

Open `SolVolumetricClouds.cfg` and search for these values:

- Altitude: `altitude`, `minAltitude`, `maxAltitude`
- Density/opacity: `_Color`, `density`, `coverageCurve`
- Motion/time progression: `speed`, `detailSpeed`, `upwardsCloudSpeed`
- Orbit visibility/detail: `_DetailScale`, `_DetailDist`, `scaledFadeStartAltitude`, `scaledFadeEndAltitude`
- Global ray quality: `temporalUpscaling`, `horizontalResolution`, `directLightTimeSlicing`
- Sky temporal smoothing: `useTemporalAntiAliasing`, `disableTaaBelowFrameRateThreshold`
- Performance: `baseStepSize`, `maxStepSize`, `lightMarchSteps`, `lightMarchDistance`

For more FPS, first increase `baseStepSize` on the lower and mid layers, then reduce `lightMarchSteps`.
