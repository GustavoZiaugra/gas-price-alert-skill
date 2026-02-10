# Gas Price Alert

**⛽ Find cheapest gas prices with daily notifications**

This OpenClaw skill helps you find and monitor gas prices, with special focus on Costco and other discount stations.

## Features

- 🔍 **ZIP code search** - Search by any US ZIP code
- 💰 **Best price detection** - Shows cheapest options first
- 📏 **Configurable radius** - Set your preferred search range
- 🏠 **Costco support** - Special tracking of Costco locations
- ⏰ **Daily alerts** - Schedule automated notifications
- 🌍 **Works anywhere** - Supports any US location

## Quick Start

```bash
# Search by ZIP code
python3 scripts/gas_alternative.py --zip 43215 --radius 20 --fuel 87 --summary

# Save to file
python3 scripts/gas_alternative.py --zip 43215 --radius 20 --fuel 87 --output results.json
```

## Installation

1. **Clone this repository:**
```bash
git clone https://github.com/GustavoZiaugra/gas-price-alert-skill.git
cd gas-price-alert-skill
```

2. **Install dependencies:**
```bash
pip install requests geopy
```

3. **Load skill into OpenClaw:**
   - Open OpenClaw Control UI
   - Go to Skills → Import Skill
   - Select this directory

## Usage

### By ZIP Code (Recommended)

```bash
# Columbus, OH
python3 scripts/gas_alternative.py --zip 43215 --radius 20 --fuel 87 --summary

# Miami, FL
python3 scripts/gas_alternative.py --zip 33101 --radius 15 --fuel 91 --summary

# Los Angeles, CA
python3 scripts/gas_alternative.py --zip 90210 --radius 10 --fuel 87 --summary
```

### By Coordinates

```bash
python3 scripts/gas_alternative.py --lat 39.9612 --lon -82.9988 --radius 20 --fuel 87 --summary
```

### Daily Alerts

Set up automated daily alerts using OpenClaw's cron system:

```json
{
  "name": "Gas price alert",
  "schedule": {
    "kind": "cron",
    "expr": "0 8 * * *",
    "tz": "America/New_York"
  },
  "payload": {
    "kind": "systemEvent",
    "text": "⛽ Gas Prices (87 Octane) - Columbus, OH\n\nRun: python3 scripts/gas_alternative.py --zip 43215 --radius 20 --fuel 87 --summary"
  },
  "sessionTarget": "main"
}
```

## Parameters

| Parameter | Description | Default | Example |
|-----------|-------------|----------|---------|
| `--zip` | ZIP code | none | `--zip 43215` |
| `--lat` | Latitude | 39.9612 | `--lat 40.7128` |
| `--lon` | Longitude | -82.9988 | `--lon -74.0060` |
| `--radius` | Search radius (miles) | 20 | `--radius 15` |
| `--fuel` | Fuel type | 87 | `--fuel 91` |
| `--base-price` | Base price for estimation | 2.89 | `--base-price 3.15` |
| `--output` | Output file | gas_prices.json | `--output results.json` |
| `--summary` | Print summary | false | `--summary` |

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

💡 Tips:
• Costco typically has gas $0.15-0.25 below market average
• For exact prices, check GasBuddy.com or station's app
• Total stations found: 137
```

## How It Works

1. **Geocoding:** Converts ZIP code to precise coordinates
2. **OpenStreetMap Search:** Finds all gas stations within radius
3. **Costco Detection:** Identifies Costco stations (typically cheapest)
4. **Price Estimation:** Estimates Costco prices based on market averages
5. **Distance Calculation:** Uses geodesic distance for accurate mileage
6. **Smart Filtering:** Removes duplicates and sorts by relevance

## Limitations

- **Real-time prices:** Uses estimated prices for Costco. For exact prices, check GasBuddy.com or station apps.
- **API availability:** OpenStreetMap/Overpass API may experience timeouts (try again later).
- **Coverage quality:** Depends on OpenStreetMap data completeness for your area.

## Contributing

To add more Costco locations or improve the skill:

1. Edit `scripts/gas_alternative.py`
2. Add locations to `search_costco_locations()`
3. Submit a pull request

## License

MIT License - Use freely for personal and commercial purposes.

## Credits

Created by **Gustavo (Cleber)** with OpenClaw
- OpenStreetMap for station data
- Geopy for geocoding
- Requests for HTTP handling

---

**Find this and more OpenClaw skills at ClawHub.com**

⭐ **Star this repository if you find it useful!**
