# TikTrace

**TikTok Public Profile OSINT Tracer** — a lightweight Bash tool that reads the publicly visible data of a TikTok profile and turns it into a clean text report.

> Coded by **Mr Sabaz Ali Khan** (Cyber Security Engineer)

![Shell](https://img.shields.io/badge/shell-bash-4EAA25?logo=gnubash&logoColor=white)
![Version](https://img.shields.io/badge/version-1.0-blue)
![Platform](https://img.shields.io/badge/platform-Linux%20%7C%20macOS%20%7C%20Termux-lightgrey)

---

## Features

- Trace a single username or a whole list from a file
- Extracts only **public** profile information:
  - User ID, nickname, bio, bio link
  - Verified / private status
  - Public region and language
  - Account creation date
  - Followers, following, total likes, videos, friends
- Simple engagement ratio (likes per follower)
- Heuristic flags (verified, private, no videos, follows far more than followers, empty bio)
- Detects non-existent profiles (username may be available)
- Saves a text report per user, optional raw JSON
- Batch mode with configurable delay between requests
- Works without `jq` (basic fallback parser), but `jq` gives best results
- Colored terminal output and ASCII banner

## Requirements

- `bash`
- `curl`
- `jq` (optional, recommended)

Install on Debian/Ubuntu/Kali:

```bash
sudo apt update && sudo apt install curl jq -y
```

Termux:

```bash
pkg install curl jq -y
```

## Installation

```bash
git clone https://github.com/whoami592/Tiktok_Osint.git
cd tiktrace
chmod +x tiktrace.sh
```

## Usage

```
./tiktrace.sh [options]

  -u <username>   Trace a single TikTok username (with or without @)
  -f <file>       Trace many usernames from a file (one per line)
  -o <dir>        Output directory for reports (default: ./tiktrace_reports)
  -d <seconds>    Delay between requests in batch mode (default: 3)
  -j              Also save raw parsed JSON per user
  -h              Show help
```

### Examples

Single user:

```bash
./tiktrace.sh -u tiktok
```

Multiple users from a file, 5 second delay, save JSON too:

```bash
./tiktrace.sh -f users.txt -d 5 -j
```

Custom output folder:

```bash
./tiktrace.sh -u tiktok -o ./reports
```

`users.txt` format:

```
tiktok
@someuser
anotheruser
```

## Sample Output

```
==================== TikTrace Report ====================
Target        : @tiktok
Profile URL   : https://www.tiktok.com/@tiktok
User ID       : ...
Nickname      : TikTok
Bio           : ...
Verified      : true
Private       : false
Followers     : ...
Following     : ...
Likes (total) : ...
Videos        : ...
Engagement    : ... likes per follower
Flags         : verified account
Scanned       : 2026-10-28 13:43:00 UTC
=========================================================
```

Reports are saved to `tiktrace_reports/<username>.txt`.

## How It Works

1. Requests the public profile page `https://www.tiktok.com/@<username>`
2. Reads the embedded `__UNIVERSAL_DATA_FOR_REHYDRATION__` data block
3. Parses the public user and stats fields
4. Builds the report and saves it to disk

## Limitations

- TikTok may rate-limit, serve a captcha, or change its page layout, which can break parsing
- Private or banned accounts expose little or no data
- Only data visible to any anonymous visitor is available; no login is used

## Disclaimer

This tool uses **only publicly available profile data**. No login, no private data, no bypassing of protections.
It is intended for lawful OSINT, research and awareness purposes. Respect TikTok's Terms of Service, privacy laws and the privacy of others. Do not use it for harassment, stalking or any illegal activity. The author is not responsible for misuse.

## Contributing

Pull requests and issues are welcome. If TikTok changes its layout and the parser breaks, please open an issue with details.

## License

Released under the MIT License. Add a `LICENSE` file to your repository.

## Author

**Mr Sabaz Ali Khan** — Cyber Security Engineer
