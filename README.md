# Gas Price Alert

OpenClaw skill for finding and monitoring gas prices with daily notifications.

## Features

- 🔍 Search gas stations by ZIP code or coordinates
- 💰 Identify cheapest options (focus on discount stations)
- 📏 Configurable search radius
- ⏰ Schedule daily alerts via cron
- 🏠 Special support for Costco (typically $0.15-0.25 below market average)
- 🌍 Works anywhere in the US

## Quick Start

### Basic Usage (Columbus, OH)

```bash
# Search by ZIP code
python3 scripts/gas_alternative.py --zip 43215 --radius 20 --fuel 87 --summary

# Search by coordinates
python3 scripts/gas_alternative.py --lat 39.9612 --lon -82.9988 --radius 20 --fuel 87 --summary

# Save to file
python3 scripts/gas_alternative.py --zip 43215 --radius 20 --fuel 87 --output gas_prices.json
```

### Parameters

| Parameter | Description | Default | Example |
|-----------|-------------|----------|---------|
| `--zip` | ZIP code for location | none | `--zip 43215` |
| `--lat` | Latitude | 39.9612 | `--lat 40.7128` |
| `--lon` | Longitude | -82.9988 | `--lon -74.0060` |
| `--radius` | Search radius in miles | 20 | `--radius 15` |
| `--fuel` | Fuel type (87, 89, 91, diesel) | 87 | `--fuel 91` |
| `--base-price` | Base price for estimation | 2.89 | `--base-price 3.15` |
| `--output` | Output file | gas_prices.json | `--output results.json` |
| `--summary` | Print human-readable summary | false | `--summary` |

## Installation

1. **Install dependencies:**

```bash
pip install requests geopy
```

2. **Install the skill:**

Move the `.skill` file to your OpenClaw skills directory or load it via the Control UI.

## Usage Examples

### Find gas near your ZIP code

```bash
# Columbus, OH - 20 miles radius
python3 scripts/gas_alternative.py --zip 43215 --radius 20 --fuel 87 --summary

# Miami, FL - 15 miles radius
python3 scripts/gas_alternative.py --zip 33101 --radius 15 --fuel 87 --summary

# Los Angeles, CA - 10 miles radius
python3 scripts/gas_alternative.py --zip 90210 --radius 10 --fuel 91 --summary
```

### Set up daily alerts

Using OpenClaw's cron system:

```bash
openclaw cron add --schedule "0 8 * * *" --tz "America/New_York" --message "Get me gas prices for ZIP 43215, 20 mile radius"
```

## How It Works

1. **Geocoding:** Converts ZIP code or coordinates to precise location
2. **OpenStreetMap Search:** Finds all gas stations within radius
3. **Costco Detection:** Identifies Costco stations (typically cheapest)
4. **Price Estimation:** Estimates Costco prices based on market averages
5. **Distance Calculation:** Uses geodesic distance for accurate mileage
6. **Smart Filtering:** Removes duplicates and sorts by relevance

## Example Output

```
⛽ Gas Prices (87 Octane) - Columbus, OH

💰 Best Prices Available (3 with prices)

• Costco Gas - $2.69 (est.)
  📍 5000 Morse Rd, Columbus, OH 43213 (7.9 miles) ⭐

• Costco Gas - $2.69 (est.)
  📍 1350 Hilliard Rome Rd, Columbus, OH 43228 (8.2 miles) ⭐

📍 Nearest Stations (Top 10 by distance)

1. Speedway (1.6 miles)
2. GetGo (2.2 miles)
3. Sunoco (2.3 miles)
4. Shell (2.8 miles)
5. Circle K (2.8 miles)

... and 129 more stations within 20 miles

💡 Tips:
• Costco typically has gas $0.15-0.25 below market average
• For exact prices, check GasBuddy.com or station's app
• Total stations found: 137
```

## Limitations

- **Real-time prices:** Uses estimated prices for Costco. For exact prices, check GasBuddy.com or station apps.
- **API availability:** OpenStreetMap/Overpass API may experience timeouts (try again later).
- **Coverage quality:** Depends on OpenStreetMap data completeness for your area.

## Data Sources

- **OpenStreetMap/Overpass:** Gas station locations and metadata
- **Geopy:** Geocoding for ZIP codes (Nominatim)
- **Costco locations:** Known major Costco locations (can be extended)

## Contributing

To add more Costco locations:

Edit `scripts/gas_alternative.py` and add to the `costco_stations` list in `search_costco_locations()`:

```python
{
    'name': 'Costco Gas',
    'address': '123 Main St, City, ST ZIP',
    'lat': 40.0123,
    'lon': -82.4567
}
```

## License

MIT License - Use freely for personal and commercial purposes.

## Support

For issues or questions:
- Check `SKILL.md` for detailed documentation
- See `references/locations.md` for city coordinates
- See `references/zip_codes.md` for Columbus area ZIP codes

---

**Created for OpenClaw** - Find this and more skills at ClawHub.com
