# Cart ni Isko Toolkit

**IT 123 – System Administration · Final Project, Option 1A (Operations and Data Protection)**
Group G01 · Block 4B

A set of six Bash scripts that install, back up, restore and monitor our capstone system, **Cart ni Isko**: an online store with a Laravel 12 API, a React 19 / Vite storefront and a Supabase PostgreSQL database, served by Nginx and PHP-FPM on Ubuntu Server.

## Files in this repository

| File | What it is |
|---|---|
| `IT123_FP_4B_G01_CartNiIsko_1A.tar.gz` | The submission archive: the six scripts, the shared settings file, the manual page, README.pdf and sample outputs |
| `README.pdf` | Full project documentation: overview, runbook, Skills Map and the Specification Walkthrough with screenshots |
| `AI-Use-Disclosure.pdf` | How we used AI and how we checked its output |

Inside the archive:

```
IT123_FP_4B_G01_CartNiIsko_1A/
├── README.pdf
├── scripts/
│   ├── cart-ni-isko-install.sh      # Script 1  Installation
│   ├── cart-ni-isko-backup.sh       # Script 2  Backup and verify
│   ├── cart-ni-isko-restore.sh      # Script 3  Restore
│   ├── cart-ni-isko-logsummary.sh   # Script 4  Log summary
│   ├── cart-ni-isko-health.sh       # Script 5  Health report
│   ├── cart-ni-isko-diskalert.sh    # Script 6  Disk alert
│   └── cart-ni-isko.conf            # shared settings (no passwords)
├── man/
│   └── cart-ni-isko-toolkit.8       # manual page for all six scripts
└── samples/                         # real output of each script from our VM
```

## The six scripts

| # | Script | What it does |
|---|---|---|
| 1 | `cart-ni-isko-install.sh` | Installs Nginx, PHP-FPM, Composer and Node.js, deploys the app, builds the frontend, connects to the database, sets owners and permissions, and checks everything |
| 2 | `cart-ni-isko-backup.sh` | Backs up the database, app files, `.env`, uploads and server settings into one archive, verifies it, and removes old backups |
| 3 | `cart-ni-isko-restore.sh` | Restores the database and files from a chosen backup, after saving an undo copy and asking for confirmation |
| 4 | `cart-ni-isko-logsummary.sh` | Summarizes errors, failed logins, backups and restores from the system, web and app logs |
| 5 | `cart-ni-isko-health.sh` | Reports CPU, memory, disk, services, website, API and database status as OK, WARN or FAIL |
| 6 | `cart-ni-isko-diskalert.sh` | Warns when disk use passes a threshold, and reports changes to important files |

Scripts 2, 4, 5 and 6 can install their own systemd timers: daily backup at 02:00, daily log summary at 06:00, an hourly health report and a disk check every 15 minutes.

## Requirements

- Ubuntu Server 24.04 or 26.04 (we used 26.04 in VirtualBox)
- sudo access and an internet connection
- PostgreSQL 17 client tools
- A Supabase project (use a **test** project, never live data)
- The Cart ni Isko source code with a filled-in `backend/.env`

## Quick start

```bash
tar -xzf IT123_FP_4B_G01_CartNiIsko_1A.tar.gz
cd IT123_FP_4B_G01_CartNiIsko_1A/scripts
chmod +x *.sh

sudo ./cart-ni-isko-install.sh                       # first run copies the settings to /etc/cart-ni-isko/ and stops
sudo nano /etc/cart-ni-isko/cart-ni-isko.conf        # check APP_URL and PHP_VER
sudo bash -c './cart-ni-isko-install.sh 2>&1 | tee /home/$SUDO_USER/install.log'   # install the store

sudo ./cart-ni-isko-backup.sh                        # make and verify a backup
sudo ./cart-ni-isko-restore.sh --latest              # restore the newest backup
sudo ./cart-ni-isko-logsummary.sh                    # log summary
sudo ./cart-ni-isko-health.sh                        # health report
sudo ./cart-ni-isko-diskalert.sh --threshold 80      # disk alert
```

Every script has `--help`, and the full manual is in `man ./cart-ni-isko-toolkit.8`.

## Exit statuses

| Status | Meaning |
|---|---|
| 0 | Success |
| 1 | Error (for example, not run as root) |
| 2 | Bad option |
| 3 | A check failed, or a DISK ALERT |
| 4 | Risk found (Script 6) |

## Security

- No passwords are stored in the scripts. The database settings are kept in `/etc/cart-ni-isko/db.conf` (root only, mode 600).
- `.env`, `db.conf` and backup archives are never included in this repository.
- All testing used a separate Supabase test project with test data.

## Group members

- Brucelo, Alyssa Mae B.
- Ebuenga, Kristel Ann B.
- Fenis, Austin B.
- Marfil, John Marvin G.

## AI use

The scripts and documents were drafted with help from Claude (Anthropic) and reviewed, tested and explained by the group. Each script says so in its comment block. See `AI-Use-Disclosure.pdf` for details.
