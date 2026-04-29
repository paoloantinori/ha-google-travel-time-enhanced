# Google Maps Travel Time Enhanced

A Home Assistant custom integration for travel time estimation using the **Google Routes API**, with support for **intermediate waypoints** and **alternative routes**.

This is an enhanced version of the built-in `google_travel_time` integration, rebuilt on top of the modern Google Routes API (`google-maps-routing` SDK) instead of the legacy Distance Matrix API.

## Features

- **Intermediate waypoints**: Force routes through specific locations (addresses, coordinates, or entity IDs) to monitor travel times for specific known routes
- **Alternative routes**: Request multiple route alternatives via service calls
- **Config flow**: Full UI-based setup with options flow for runtime configuration
- **All travel modes**: Driving, walking, bicycling, and transit
- **Traffic-aware**: Real-time traffic data with configurable traffic models (best_guess, pessimistic, optimistic)
- **Entity tracking**: Use `device_tracker`, `sensor`, `zone`, or `person` entities as origin/destination
- **Services**: `get_travel_times` and `get_transit_times` for on-demand route queries

## Differences from Built-in Integration

| Feature | Built-in | Enhanced |
|---------|----------|----------|
| API | Distance Matrix | Routes API |
| Waypoints | No | Yes |
| Alternative routes | No | Yes |
| Config flow | No (YAML only) | Yes |
| Transit support | Limited | Full |
| Python SDK | `googlemaps==2.5.1` | `google-maps-routing==0.6.15` |

## Installation

### HACS (recommended)

1. Add this repository as a custom HACS repository
2. Search for "Google Maps Travel Time Enhanced" in HACS
3. Install and restart Home Assistant

### Manual

1. Copy the `custom_components/google_travel_time_enhanced/` directory to your Home Assistant `custom_components/` directory
2. Restart Home Assistant

## Setup

### Prerequisites

1. A Google Cloud project with the **Routes API** enabled (not Distance Matrix API)
2. An API key with access to the Routes API

### Configuration

1. Go to **Settings > Devices & Services**
2. Click **Add Integration** and search for "Google Maps Travel Time Enhanced"
3. Enter your API key, origin, destination, and name
4. Configure options (travel mode, waypoints, traffic model, etc.) in the integration options

### Intermediate Waypoints

Add waypoints in the integration options to force the route through specific locations:

- Addresses: `"Corso Milano, Novara"`
- Coordinates: `"45.4642,8.6234"`
- Entity IDs: `"zone.school"`

Multiple waypoints are supported. The route will pass through all specified waypoints in order.

## Services

### `get_travel_times`

Get travel times for non-transit modes with optional waypoints.

```yaml
service: google_travel_time_enhanced.get_travel_times
data:
  config_entry_id: YOUR_CONFIG_ENTRY_ID
  origin: "Via Roma 1, Milan"
  destination: "Piazza Duomo, Milan"
  mode: driving
  intermediates:
    - "Corso Milano, Novara"
  traffic_model: best_guess
```

### `get_transit_times`

Get travel times for public transit.

```yaml
service: google_travel_time_enhanced.get_transit_times
data:
  config_entry_id: YOUR_CONFIG_ENTRY_ID
  origin: "Via Roma 1, Milan"
  destination: "Piazza Duomo, Milan"
  transit_mode: train
```

## License

MIT
