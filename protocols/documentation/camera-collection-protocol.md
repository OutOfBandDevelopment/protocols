# Camera Collection Documentation Protocol

Standard procedure for documenting cameras, lenses, and photography accessories.

---

## Overview

This protocol ensures consistent documentation of camera bodies, lenses, and accessories. The goal is to maintain a comprehensive inventory with technical specifications, mount compatibility, and condition tracking.

---

## Directory Structure

```
Camera Collection/
├── readme.md                    # Collection index
├── Bodies/
│   ├── nikon-d800/
│   │   └── README.md
│   ├── nikon-fg-20/
│   │   └── README.md
│   └── mamiya-sekor-1000-dtl/
│       └── README.md
├── Lenses/
│   ├── sigma-300mm-f2.8-ex-dg/
│   │   └── README.md
│   └── helios-44m-6/
│       └── README.md
├── Teleconverters/
│   └── sigma-1.4x-ex-dg/
│       └── README.md
└── Accessories/
    └── arca-swiss-clamp/
        └── README.md
```

---

## Naming Conventions

- Directories: lowercase with hyphens
- Format: `brand-model-variant` (e.g., `nikon-d800`, `sigma-300mm-f2.8-ex-dg`)
- Lenses include focal length and aperture in name

---

## Camera Body Template

```markdown
# [Brand] [Model]

**[Film/Digital] [Format] Camera Body**

## Unit Information

| Field | Value |
|-------|-------|
| Manufacturer | [Brand] |
| Model | [Model] |
| Type | [DSLR/SLR/Mirrorless/Rangefinder] |
| Format | [Full Frame/APS-C/Medium Format/35mm Film] |
| Mount | [F-mount/M42/etc.] |
| Status | [Working/Needs Repair/For Parts] |
| Date Ordered | [YYYY-MM-DD] |
| Date Received | [YYYY-MM-DD] |
| Purchase Price | $[amount] |
| Source | [eBay (seller)/Store] |

## Specifications

| Specification | Value |
|--------------|-------|
| Sensor/Film | [type and size] |
| Resolution | [megapixels or N/A] |
| ISO Range | [range] |
| Shutter Speed | [range] |
| Viewfinder | [type and coverage] |
| Autofocus | [Yes/No - points] |
| Metering | [modes] |
| Flash Sync | [speed] |
| Battery | [type] |
| Weight | [grams] |

## Features

- [Key feature 1]
- [Key feature 2]
- [etc.]

## Compatible Lenses

- Native [mount] lenses
- [Adapted mount] via [adapter type]

## Documentation

- [User Manual (PDF)](url)
- [Service Manual (PDF)](url)

## Local Files

- `[Model]_Manual.pdf` - User manual

## Condition Notes

[Any issues, repairs needed, or cosmetic notes]

## Revision History

| Date | Change |
|------|--------|
| YYYY-MM-DD | Initial entry |
```

---

## Lens Template

```markdown
# [Brand] [Focal Length] [Aperture] [Line/Variant]

**[Type] Lens - [Mount]**

## Unit Information

| Field | Value |
|-------|-------|
| Manufacturer | [Brand] |
| Model | [Full model name] |
| Mount | [F-mount/M42/EF/etc.] |
| Status | [Working/Needs Repair/For Parts] |
| Date Ordered | [YYYY-MM-DD] |
| Date Received | [YYYY-MM-DD] |
| Purchase Price | $[amount] |
| Source | [eBay (seller)/Store] |

## Specifications

| Specification | Value |
|--------------|-------|
| Focal Length | [mm or range] |
| Maximum Aperture | f/[number] |
| Minimum Aperture | f/[number] |
| Lens Construction | [elements in groups] |
| Angle of View | [degrees] |
| Minimum Focus | [distance] |
| Filter Size | [mm] |
| Dimensions | [diameter x length] |
| Weight | [grams] |
| Autofocus | [Yes/No - motor type] |
| Stabilization | [Yes/No - type] |

## Features

- [Key feature 1]
- [Key feature 2]

## Compatible Bodies

- Native: [mount] cameras
- Adapted: [other mounts] via [adapter]

## Documentation

- [Product Page](url)
- [Review](url)

## Local Files

- `[Model]_Manual.pdf` - User manual (if available)

## Condition Notes

[Any issues, repairs needed, or cosmetic notes]

## Revision History

| Date | Change |
|------|--------|
| YYYY-MM-DD | Initial entry |
```

---

## Teleconverter Template

```markdown
# [Brand] [Magnification] [Line/Variant]

**Teleconverter - [Mount]**

## Unit Information

| Field | Value |
|-------|-------|
| Manufacturer | [Brand] |
| Model | [Full model name] |
| Magnification | [1.4x/2x] |
| Mount | [F-mount/EF/etc.] |
| Status | [Working/Needs Repair] |
| Date Ordered | [YYYY-MM-DD] |
| Date Received | [YYYY-MM-DD] |
| Purchase Price | $[amount] |
| Source | [eBay (seller)/Store] |

## Specifications

| Specification | Value |
|--------------|-------|
| Magnification | [factor] |
| Light Loss | [stops] |
| AF Compatibility | [requirements] |
| Construction | [elements in groups] |
| Weight | [grams] |

## Compatible Lenses

[List of compatible lenses, especially any restrictions]

## Revision History

| Date | Change |
|------|--------|
| YYYY-MM-DD | Initial entry |
```

---

## Accessory Template

```markdown
# [Brand] [Model/Type]

**[Accessory Type]**

## Unit Information

| Field | Value |
|-------|-------|
| Manufacturer | [Brand] |
| Model | [Model] |
| Type | [Tripod/Clamp/Adapter/Filter/etc.] |
| Status | [Working] |
| Purchase Price | $[amount] |
| Source | [eBay (seller)/Store] |

## Specifications

[Relevant specs for the accessory type]

## Compatibility

[What it works with]

## Revision History

| Date | Change |
|------|--------|
| YYYY-MM-DD | Initial entry |
```

---

## Lens Mount Reference

| Mount | Flange Distance | Notes |
|-------|-----------------|-------|
| Nikon F | 46.5mm | Can adapt most other mounts |
| M42 (Pentax/Praktica) | 45.46mm | Universal screw mount, adapts to F-mount |
| Canon EF | 44mm | |
| Sony A | 44.5mm | Minolta A-mount |
| Sony E | 18mm | Mirrorless, adapts everything |
| Micro Four Thirds | 19.25mm | Adapts everything |

---

## Condition Ratings

| Rating | Description |
|--------|-------------|
| Mint | Like new, no signs of use |
| Excellent | Minimal wear, fully functional |
| Good | Normal wear, fully functional |
| Fair | Visible wear, functional with minor issues |
| Poor | Heavy wear, functional issues |
| For Parts | Non-functional, useful for parts only |
| Needs Repair | Specific repair needed (document in notes) |

---

## Information Sources

### Nikon
- [Nikon Product Archive](https://www.nikonusa.com/en/nikon-products/product-archive/)
- [Nikon Imaging - Historical Products](https://imaging.nikon.com/history/)

### Sigma
- [Sigma Global](https://www.sigma-global.com/)
- [Sigma Lens Specifications](https://www.sigma-global.com/en/lenses/)

### Vintage Lenses (M42, etc.)
- [Pentax Forums Lens Database](https://www.pentaxforums.com/lensreviews/)
- [Adapted Lens Review](https://phillipreeve.net/blog/)
- [Vintage Camera Lab](https://vintagecameralab.com/)

### General
- [DPReview (archived)](https://www.dpreview.com/)
- [LensTip](https://www.lenstip.com/lenses.html)
- [Photozone](https://www.photozone.de/)

---

## Cross-References

When documenting cameras/lenses:
- Link related items (body + compatible lenses)
- Reference adapters when discussing mount compatibility
- Update purchase info in `ShoppingOnline.md`

---

*Protocol version: 1.0*
*Last updated: December 2025*
