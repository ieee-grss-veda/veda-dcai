# GeoCroissant Metadata Standard

GeoCroissant extends MLCommons Croissant with geospatial properties for Earth observation and machine learning datasets. It provides structured, machine-actionable metadata so tools and frameworks can find, index, and load geospatial data directly into training pipelines.

## Core Geospatial Extensions

GeoCroissant adds five main property groups to Croissant:

1. **Spatial and temporal bounds**: `spatialCoverage` (geographic bounding box or geometry) and `temporalCoverage` (observation date or range).
2. **Coordinate reference systems**: `geocr:coordinateReferenceSystem` identifies the projection (such as `EPSG:4326` or `EPSG:32610`) required to locate pixels on Earth.
3. **Resolution**: `geocr:spatialResolution` gives the ground sampling distance (e.g. 30 meters per pixel), and `geocr:temporalResolution` defines observation frequency.
4. **Band layout**: `geocr:bandConfiguration` defines the total band count and ordered band names (`geocr:bandNamesList`).
5. **Physical band physics**: `geocr:spectralBandMetadata` describes individual channels with `geocr:centerWavelength` and `geocr:bandwidth`.

## Conformance

Datasets declare conformance in the `@context` and `dct:conformsTo` fields:

```json
"conformsTo": [
  "http://mlcommons.org/croissant/1.1",
  "http://mlcommons.org/croissant/geo/1.0"
]
```

## Example Record

This snippet shows optical sensor properties structured with the `geocr` vocabulary:

```json
{
  "geocr:coordinateReferenceSystem": "EPSG:32610",
  "geocr:spatialResolution": {
    "@type": "QuantitativeValue",
    "value": 30.0,
    "unitText": "m"
  },
  "geocr:bandConfiguration": {
    "@type": "geocr:BandConfiguration",
    "geocr:totalBands": 6,
    "geocr:bandNamesList": ["Blue", "Green", "Red", "NIR", "SWIR1", "SWIR2"]
  },
  "geocr:spectralBandMetadata": [
    {
      "@type": "geocr:SpectralBand",
      "name": "Blue",
      "geocr:centerWavelength": {
        "@type": "QuantitativeValue",
        "value": 490,
        "unitText": "nm"
      },
      "geocr:bandwidth": {
        "@type": "QuantitativeValue",
        "value": 65,
        "unitText": "nm"
      }
    }
  ]
}
```

## Domain Extensions

The specification includes fields for specific workflows:

- **Responsible AI (`geocr:spatialBias`, `geocr:samplingStrategy`)**: Documents geographic imbalances (such as datasets sampled primarily over North America) and windowing methods used to cut image chips.
- **Space weather & heliophysics (`geocr:multiWavelengthConfiguration`, `geocr:solarInstrumentCharacteristics`)**: Encodes solar instrument channels and observatories (such as SDO/AIA) without geographic coordinates.
- **Spatial indexing (`geocr:spatialIndex`)**: Holds precomputed discrete global grid tokens (such as H3 or S2 cells) for fast catalog lookups before polygon intersections.
