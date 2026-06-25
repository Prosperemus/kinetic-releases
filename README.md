# Kinetic Suite for Power BI

A suite of Power BI custom visuals for operational and geospatial data built around the MIL-STD-2525C symbology standard.

| Visual | Description |
|---|---|
| **Kinetic Map** | Plot unit and event symbols on a satellite basemap (MGRS or lat/long) |
| **Kinetic Map 3D** | Same as Kinetic Map, rendered on a photorealistic 3D globe |
| **Kinetic Timeline** | Scrollable day-strip slicer; pairs with either map visual to filter by date and time |

---

## Download

The latest release is **v1.3.0.0**. Download the `.pbiviz` files from the [Releases](../../releases/latest) page and import them into Power BI via **Insert → Get more visuals → Import a visual from a file**.

| Visual | Download |
|---|---|
| Kinetic Map | [KineticMap-1.3.0.0.pbiviz](../../releases/download/v1.3.0.0/KineticMap-1.3.0.0.pbiviz) |
| Kinetic Map 3D | [KineticMap3D-1.3.0.0.pbiviz](../../releases/download/v1.3.0.0/KineticMap3D-1.3.0.0.pbiviz) |
| Kinetic Timeline | [KineticTimeline-1.3.0.0.pbiviz](../../releases/download/v1.3.0.0/KineticTimeline-1.3.0.0.pbiviz) |

---

## Sample files

The [`samples/`](samples/) folder contains ready-to-open `.pbix` files demonstrating the visuals with real data.

| File | Description |
|---|---|
| [Gettysburg.pbix](samples/Gettysburg.pbix) | Battle of Gettysburg (July 1–3, 1863) — unit positions plotted via MGRS with the Kinetic Timeline for day-by-day filtering |

---

## Kinetic Map — field reference

### Data field wells

| Field well | Type | Description |
|---|---|---|
| **Name** | Text | Unit or event label shown beneath the symbol |
| **MGRS** | Text | Military Grid Reference System string (e.g. `16SBK1234567890`). Takes priority over Lat/Lon when both are bound. |
| **Lat** | Decimal | Latitude in decimal degrees |
| **Lon** | Decimal | Longitude in decimal degrees |
| **Symbol Type** | Text | MIL-STD-2525C SIDC string (see [Symbol codes](#symbol-codes) below) |
| **Affiliation** | Text | `F` Friendly · `H` Hostile · `N` Neutral · `U` Unknown |
| **Organization** | Text | Groups units for color-by-organization mode |
| **Color** | Text | Hex (`#FF6600`) or CSS color name to override the symbol fill |
| **Date** | Date/Time | Enables movement trails and timeline filtering |
| **Filter** | Any | Category field exposed in the filter overlay panel *(paid)* |
| **Priority** | Number | Z-order for overlapping symbols — higher number renders on top |

### Symbol codes (SIDC)

Kinetic Map uses the **MIL-STD-2525C** Symbol Identification Code (SIDC) — a 15-character string that encodes the symbol's scheme, affiliation, category, function, and modifiers.

**Structure:**

```
Position:  1    2    3    4    5-6   7-10   11-12  13-14  15
           |    |    |    |    |     |      |      |      |
           S    F    G    P    U C   ------  --    --     -
           ^    ^    ^    ^    ^ ^
           |    |    |    |    | Echelon
           |    |    |    |    Status
           |    |    |    Function ID (4 chars, pos 7-10)
           |    |    Category
           |    Affiliation
           Scheme
```

**Position 1 — Coding scheme**

| Code | Scheme |
|---|---|
| `S` | Warfighting |
| `I` | Intelligence |
| `O` | Operations/orders |
| `E` | Emergency management |

**Position 2 — Affiliation** (also drives frame color)

| Code | Affiliation | Frame color |
|---|---|---|
| `F` or `A` | Friendly / Assumed friendly | Blue |
| `H` or `S` | Hostile / Suspect | Red |
| `N` | Neutral | Green |
| `U` or `P` | Unknown / Pending | Yellow |

**Position 3 — Category (battle dimension)**

| Code | Category |
|---|---|
| `P` | Space |
| `A` | Air |
| `G` | Ground |
| `S` | Sea surface |
| `U` | Subsurface |
| `F` | Special operations forces (SOF) |

**Position 4 — Status**

| Code | Status |
|---|---|
| `P` | Present (solid frame) |
| `A` | Anticipated / Planned (dashed frame) |

**Positions 5–6 — Function ID (first two characters)**

Common ground unit codes:

| SIDC prefix (pos 1–6) | Unit type |
|---|---|
| `SFGP**` | Infantry |
| `SFGPA*` | Airborne infantry |
| `SFGPE*` | Mechanized infantry |
| `SFGPD*` | Armor / tank |
| `SFGPF*` | Airborne |
| `SFGPC*` | Cavalry |
| `SFGPCA` | Armored cavalry |
| `SFGX**` | Task force (no function) |
| `SFAPE*` | Fixed-wing aircraft |
| `SFAPM*` | Rotary-wing (helicopter) |
| `SFAP**` | Aviation |
| `SHGP**` | Hostile ground unit (infantry) |
| `SNGP**` | Neutral ground unit |
| `SUGP**` | Unknown ground unit |

**Positions 11–12 — Echelon**

| Code | Echelon |
|---|---|
| `--` | None |
| `A-` | Team / crew |
| `B-` | Squad |
| `C-` | Section |
| `D-` | Platoon / detachment |
| `E-` | Company / battery / troop |
| `F-` | Battalion / squadron |
| `G-` | Regiment / group |
| `H-` | Brigade |
| `I-` | Division |
| `J-` | Corps / MEF |
| `K-` | Army |
| `L-` | Army group / front |
| `M-` | Region |
| `N-` | Command |

**Example SIDCs**

| SIDC | Description |
|---|---|
| `SFGPUCI---E----` | Friendly ground infantry company |
| `SFGPUCF---F----` | Friendly ground infantry battalion |
| `SFGPUCF---G----` | Friendly ground infantry regiment |
| `SFGPUCD---E----` | Friendly armored company |
| `SFGPUCD---F----` | Friendly armor battalion |
| `SFAPMFQ---****-` | Friendly helicopter (attack) |
| `SHGPUCI---E----` | Hostile infantry company |
| `SHGPUCD---F----` | Hostile armor battalion |
| `SUGPUCI--------` | Unknown infantry unit |

**Minimal working SIDC**

If you only need a symbol to appear without a specific type, use a 15-character string padded with `-`:

```
SFGP-----------    <- Friendly ground (generic)
SFAP-----------    <- Friendly air (generic)
SHGP-----------    <- Hostile ground (generic)
```

---

## Kinetic Timeline — field reference

### Data field wells

| Field well | Type | Description |
|---|---|---|
| **Date** | Date/Time | The column to filter on. Bind the same column as the Kinetic Map's Date field to keep both visuals synchronized. |

### Key settings (Format pane)

| Card | Setting | Description |
|---|---|---|
| Timeline | Fit to data | Anchors the initial view to the earliest date in the dataset instead of today |
| Timeline | Day width | Width of each day cell in pixels |
| Time of Day | Enabled | Shows the within-day hour range slider when a single day is selected *(paid)* |
| Colors | Scheme | Choose from High Contrast (free), Steel, Forest, Slate, Earth, or military branch themes *(paid)* |
| License | License Key | Enter your `KIN-XXXX-XXXX-XXXX-XXXX` key to unlock paid features |

---

## Licensing

Free features are available without a license key. Paid features — movement trails, MGRS grid, filter overlay, custom basemaps, time-of-day filtering, and color themes — require a Kinetic license.

Get a license at **[license.prosperemus.com/pricing](https://license.prosperemus.com/pricing)**.

---

## Support

- Kinetic Map: [prosperemus.com/kinetic-map/support/](https://prosperemus.com/kinetic-map/support/)
- Kinetic Timeline: [prosperemus.com/kinetic-timeline/support/](https://prosperemus.com/kinetic-timeline/support/)
- Email: [support@prosperemus.com](mailto:support@prosperemus.com)
