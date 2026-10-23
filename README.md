# field-site-weather

Weather observation registry for our three US field sites. After last week's storms knocked out two sites, our incident reports had no weather context to cite. This registry exists so that every incident report can reference documented site coordinates, the nearest weather observation stations, and a baseline conditions snapshot.

## What this registry contains

| File | Site | Location |
| --- | --- | --- |
| `sites/manhattan.json` | Manhattan office | Midtown East, New York, NY |
| `sites/chicago.json` | Chicago office | The Loop, Chicago, IL |
| `sites/san-francisco.json` | San Francisco office | SoMa (South of Market), San Francisco, CA |

Each site file records:

- **Site name and description**
- **Coordinates** (decimal degrees, WGS84) with the **public web source URL** they were taken from — no eyeballed coordinates
- **Nearest weather observation stations**, with station IDs, names, elevations, and distances from the site
- **Current-conditions snapshot**, captured from the nearest reporting station as a baseline at registry creation time

## Where the data comes from

- **Site coordinates:** looked up on the public web (Wikipedia). The exact source URL for each site's coordinates is recorded in that site's JSON file:

  | Site | Coordinates | Source page |
  | --- | --- | --- |
  | Manhattan (Midtown East, New York, NY) | 40.7547, -73.9842 | https://en.wikipedia.org/wiki/Midtown_Manhattan |
  | Chicago (The Loop) | 41.88111, -87.62972 | https://en.wikipedia.org/wiki/Loop_(Chicago) |
  | San Francisco (SoMa) | 37.77722, -122.41111 | https://en.wikipedia.org/wiki/South_of_Market_(San_Francisco) |

  The coordinates are the geocoded feature coordinates published on each page: `40.7547, -73.9842` for Midtown Manhattan, `41.88111, -87.62972` for The Loop, and `37.77722, -122.41111` for South of Market.

- **Weather stations:** the nearest observation stations (ASOS/AWOS and other automated stations) for each site's coordinates, looked up from the public weather-observation-station service.
- **Current conditions:** baseline snapshots retrieved from the nearest reporting weather observation station at registry creation time (baseline captured 2026-10-23; station reporting lag is noted per site where it applies).

## Data flow

Site data files live on the `field-site-data` branch and are merged into the default branch (`main`) via pull request. The `.github/workflows/validate-sites.yml` workflow lives on the default branch only; it is manually triggerable (`workflow_dispatch`) and validates that every `sites/*.json` file parses as valid JSON.

## Usage

1. Locate the site in the table above and open its JSON file.
2. Cite the coordinates, the coordinate source URL, and the nearest station ID in your incident report.
3. To refresh conditions, use the site's coordinates and nearest station as the observation point.

## Maintenance

- When a site's coordinates change, update the JSON file **and** the coordinate source URL together.
- Re-run the validation workflow on the `field-site-data` branch after syncing it with `main` before merging.