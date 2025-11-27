# IDI-FLY-V3 API Documentation

Official documentation for the IDI-FLY-V3 Hybrid API for Mission Telemetry.

## View Documentation

Visit the live documentation site: **[https://YOUR_USERNAME.github.io/IDI-DOCUMENTATION](https://YOUR_USERNAME.github.io/IDI-DOCUMENTATION)**

## Local Development

To run this documentation site locally:

1. Install Ruby and Bundler
2. Clone this repository
3. Run `bundle install`
4. Run `bundle exec jekyll serve`
5. Open `http://localhost:4000` in your browser

## Deployment

This site is automatically deployed via GitHub Pages when changes are pushed to the `main` branch.

### Setup Instructions

1. Push this repository to GitHub
2. Go to **Settings > Pages**
3. Under "Source", select **Deploy from a branch**
4. Select the `main` branch and `/ (root)` folder
5. Click **Save**
6. Wait a few minutes for the site to build

## API Overview

The IDI-FLY-V3 API provides:

- **9 Endpoints** for mission control and telemetry
- **Hybrid Architecture**: REST (HTTPS), MQTTS, and WebSocket (WSS)
- **Real-time Streaming** at 1 Hz frequency
- **JWT Authentication** across all protocols
- **Role-Based Access Control**: VIEWER, PILOT, MANAGER

## File Structure

```
IDI-DOCUMENTATION/
├── _config.yml          # Jekyll configuration
├── index.md             # Main documentation (GitHub Pages entry)
├── IDI-FLY-V3-API.md    # Source API documentation
├── README.md            # This file
└── .gitignore           # Git ignore rules
```

## License

Copyright 2025. All rights reserved.
