# Nitter Aggregator for YunoHost

[![Integration level](https://dash.yunohost.org/integration/nitter_aggregator.svg)](https://dash.yunohost.org/appci/app/nitter_aggregator)
[![Install Nitter Aggregator with YunoHost](https://install-app.yunohost.org/install-with-yunohost.svg)](https://install-app.yunohost.org/?app=nitter_aggregator)

> *This package allows you to install Nitter Aggregator quickly and simply on a YunoHost server.*  
> *If you don't have YunoHost, please consult [the guide](https://yunohost.org/#/install) to learn how to install it.*

## Overview

A simple web application to follow multiple Twitter/X accounts via Nitter without needing an X.com account. Features a unified timeline view with automatic refresh, retweet filtering, and persistent account storage.

**Shipped version:** 1.0

## Screenshots

![Nitter Aggregator Interface](./doc/screenshots/screenshot.png)

## Features

- **No X.com account required**: Follow accounts via Nitter instances
- **Unified timeline**: All tweets from followed accounts in one chronological feed
- **Auto-refresh**: Configurable automatic timeline updates
- **Retweet filtering**: Toggle retweets on/off
- **Account management**: Add/remove unlimited accounts
- **Local storage**: Your account list is saved in your browser
- **Dark theme**: Easy on the eyes
- **Privacy-focused**: No tracking, no ads, all client-side

## Documentation and resources

* Official app website: <https://github.com/Citify/nitter-aggregator>
* YunoHost Store: <https://apps.yunohost.org/app/nitter_aggregator>
* Report a bug: <https://github.com/Citify/nitter-aggregator_ynh/issues>

## Installation

### Install from YunoHost admin interface

1. Go to your YunoHost admin panel
2. Navigate to **Applications** → **Install**
3. Click **Install custom app**
4. Enter this repository URL: `https://github.com/Citify/nitter-aggregator_ynh`
5. Choose your domain and path (e.g., `/nitter`)
6. Set permissions (default: public)
7. Click **Install**

### Install from command line

```bash
sudo yunohost app install https://github.com/Citify/nitter-aggregator_ynh
```

Or, if you need to specify custom parameters:

```bash
sudo yunohost app install https://github.com/Citify/nitter-aggregator_ynh \
  --args "domain=yourdomain.com&path=/nitter&init_main_permission=visitors"
```

## Upgrade

```bash
sudo yunohost app upgrade nitter_aggregator -u https://github.com/Citify/nitter-aggregator_ynh
```

## Configuration

After installation, access the app at `https://yourdomain.com/nitter/nitter-aggregator.html`

### Adding accounts

1. Enter a Twitter/X username (without @)
2. Click "Add Account"
3. The account will appear in your list and tweets will load

### Settings

- **Auto-refresh interval**: Set how often the timeline updates (in seconds)
- **Show retweets**: Toggle to filter out retweets from the timeline
- **Nitter instance**: By default uses `nitter.net`, but you can modify in the code

### Backup and restore

The app's user data (followed accounts) is stored in your browser's localStorage, not on the server. To backup:

1. Export your account list from the browser's developer console:
   ```javascript
   console.log(localStorage.getItem('nitterAccounts'));
   ```
2. Save the output

To restore, paste it back:
```javascript
localStorage.setItem('nitterAccounts', 'YOUR_SAVED_DATA');
```

## Limitations

* **Nitter availability**: This app relies on public Nitter instances. If an instance goes down, you may need to switch to another one.
* **Rate limiting**: Nitter instances may have rate limits. If you follow many accounts, you might hit these limits.
* **No server-side storage**: Account lists are stored in browser localStorage only

## Developer info

### Structure

```
nitter-aggregator_ynh/
├── manifest.toml          # YunoHost app manifest
├── scripts/
│   ├── install           # Installation script
│   ├── remove            # Removal script
│   ├── upgrade           # Upgrade script
│   ├── backup            # Backup script
│   ├── restore           # Restore script
│   └── _common.sh        # Common functions
├── conf/
│   └── nginx.conf        # NGINX configuration template
└── README.md             # This file
```

### Building from source

To create a new release:

1. Update version in `manifest.toml`
2. Update the SHA256 checksum:
   ```bash
   curl -L https://github.com/Citify/nitter-aggregator/archive/refs/heads/main.tar.gz | sha256sum
   ```
3. Replace the `sha256` value in `manifest.toml`
4. Commit and push
5. Install/upgrade using the repository URL

### Testing

```bash
# Install in testing mode
sudo yunohost app install https://github.com/Citify/nitter-aggregator_ynh --debug

# Check logs
sudo yunohost log show nitter_aggregator --share
```

## License

This YunoHost package is released under MIT License.

The Nitter Aggregator application itself is also MIT licensed.

## Troubleshooting

### App won't load

1. Check NGINX configuration:
   ```bash
   sudo nginx -t
   sudo systemctl status nginx
   ```

2. Check PHP-FPM:
   ```bash
   sudo systemctl status php8.2-fpm
   ```

### Nitter instance not working

Try changing the Nitter instance in `nitter-aggregator.html`:

```javascript
const NITTER_INSTANCE = 'nitter.poast.org'; // or another instance
```

See [Nitter instances list](https://github.com/zedeus/nitter/wiki/Instances) for alternatives.

### Tweets not loading

- Check browser console for errors (F12)
- Verify the Nitter instance is accessible
- Try reducing the number of accounts you follow
- Clear browser cache and localStorage

## Support

For issues related to:
- **The YunoHost package**: Open an issue at <https://github.com/Citify/nitter-aggregator_ynh/issues>
- **The app itself**: Open an issue at <https://github.com/Citify/nitter-aggregator/issues>
- **YunoHost in general**: See <https://yunohost.org/help>

---

**Developer**: Citify  
**YunoHost package maintainer**: Citify
