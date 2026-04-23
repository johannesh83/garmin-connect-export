garmin-connect-export
=====================

Modern Python 3 script to download GPX, TCX, FIT, or JSON files from your Garmin Connect account.

Description
-----------
This script uses the modern `garminconnect` library to authenticate with Garmin Connect and download your activities in multiple formats.

**Features:**
- Modern OAuth authentication (works with current Garmin Connect API)
- Support for GPX, TCX, FIT, and JSON formats
- Automatic FIT file extraction from ZIP archives
- Date-prefixed filenames for easy organization (YYYY-MM-DD_activityid)
- Secure credential management via `.env` file
- Configurable output directory
- Activity summary saved as JSON

**File Formats:**
- **FIT** (default): Binary format with full workout data (power, heart rate, cadence, etc.)
- **TCX**: XML format with detailed trackpoint data
- **GPX**: GPS track format (may be empty for indoor activities)
- **JSON**: Complete activity metadata and details

All files are saved with the activity date in the filename (e.g., `2025-11-30_12345678.fit`) for easy sorting and identification.

Requirements
------------
```bash
pip3 install garminconnect
```

or

```bash
pip3 install -f requirements.txt
```

Setup
-----
1. Create a `.env` file in the same directory as the script:
```bash
GARMIN_USERNAME=your_email@example.com
GARMIN_PASSWORD=your_password

# Optional: Set default output directory
GARMIN_OUTPUT_DIR=/path/to/your/export/directory
```

2. Make sure `.env` is in your `.gitignore` to keep credentials secure (a `.env.example` file is provided as a template)

Usage
-----
```bash
python3 garmin_export.py [-h] [--username USERNAME] [--password PASSWORD] 
                         [-c COUNT] [-f {gpx,tcx,fit,json}] [-d DIRECTORY]
```

**Options:**

- `-h, --help` - Show help message
- `--username USERNAME` - Garmin Connect username (or use `.env` file)
- `--password PASSWORD` - Garmin Connect password (or use `.env` file)
- `-c COUNT, --count COUNT` - Number of recent activities to download (default: 10)
- `-f {gpx,tcx,fit,json}, --format {gpx,tcx,fit,json}` - Export format (default: fit)
- `-d DIRECTORY, --directory DIRECTORY` - Output directory (default: `GARMIN_OUTPUT_DIR` from `.env` or `./garmin_exports`)

Examples
--------

**Download last 15 activities as FIT files (uses default directory):**
```bash
python3 garmin_export.py -c 15
```

**Download last 5 activities as TCX to custom directory:**
```bash
python3 garmin_export.py -c 5 -f tcx -d ~/MyActivities
```

**Download last 10 activities as JSON with full details:**
```bash
python3 garmin_export.py -c 10 -f json
```

**Use credentials from command line (not recommended):**
```bash
python3 garmin_export.py -c 3 --username user@example.com --password mypassword
```

**Best practice:** Use a `.env` file for credentials to keep them secure and out of command history.

Output
------
The script will:
1. Authenticate with Garmin Connect
2. Download the requested number of activities
3. Save each activity with a date-prefixed filename (e.g., `2025-11-30_12345678.fit`)
4. For FIT format: Automatically extract files from ZIP archives and clean up temporary ZIP files
5. Show progress and success count

**File naming:** Activities are saved as `YYYY-MM-DD_activityid.ext` making it easy to sort chronologically and identify specific workouts.

Notes
-----
- **Credentials:** Store in `.env` file for security. Never commit credentials to version control.
- **Output Directory:** Configure default output directory via `GARMIN_OUTPUT_DIR` in `.env` file to avoid hardcoding paths.
- **Indoor Activities:** GPX files may be empty for indoor workouts (no GPS data). Use FIT or TCX for full workout metrics.
- **FIT Files:** Contain the most complete data including power, heart rate, cadence, and other sensor data.

Legacy Script
-------------
The original `gcexport.py` script is still included but uses outdated authentication and may not work with current Garmin Connect. Use `garmin_export.py` instead.

Contributions
-------------
Contributions are warmly welcome, particularly if this script stops working with Garmin Connect. You may consider opening a GitHub issue first. New features, however simple, are encouraged.

Golden Cheetah & Garmin Connect
-------------------------------
You are a runner, cyclist or triathlete? You love Golden Cheetah? You track your activities with Garmin devices? You want to download all of them from Garmin Connect? Okay, this got answered already within here.

But now, you want to archive, cloud-backup and import automatically into Golden Cheetah? There are tons of alternatives and workarounds. Here is mine.

[Download, archive, cloud-backup and auto-import your activities.](https://www.johannesheinrich.de/posts/golden-cheetah-garmin-connect-script/)

Thank You
---------
Other than that, thx for using this script.

DISCLAIMER
----------
No Guarantee

This script does NOT guarantee to get all your data or even download it correctly. Against my Garmin Connect account it works quite fine and smooth, but different Garmin Connect account settings or different data types could potentially cause problems.

Garmin Connect API

This is NOT an official feature of Garmin Connect, Garmin may very well make changes to their APIs that breaks this script (and they certainly did since this project got created for several times).

THIS SCRIPT IS FOR PERSONAL USE ONLY

It simulates a standard user session (i.e., in the browser), logging in using cookies and an authorization ticket. This makes the script pretty brittle. If you're looking for a more reliable option, particularly if you wish to use this for some production service, Garmin does offer a paid API service.

Security Dislaimer

Using the `--username` and `--password` flags are not recommended because your password will be stored in your command line history. Instead, omit them to be prompted (and note that nothing will be displayed when you type your password).

Contributing
------------
Contributions are welcome! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

This project follows a [Code of Conduct](CODE_OF_CONDUCT.md). By participating, you are expected to uphold this code.

License
-------
[MIT](LICENSE) &copy; 2015 Kyle Krafka, 2025 AJ Enns
