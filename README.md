# IDI Integration API Documentation

Official documentation for the IDI Integration API.

## View Documentation

Visit the live documentation site: **[https://doc.idi-fly.com](https://doc.idi-fly.com)**

## Local Development

To run this documentation site locally:

1. Install Ruby and Bundler
2. Clone this repository
3. Run `bundle install`
4. Run `bundle exec jekyll serve`
5. Open `http://localhost:4000` in your browser

## Deployment

This site is automatically deployed via GitHub Pages when changes are pushed to the `main` branch.

## API Overview

The IDI Integration API provides:

- **9 Endpoints** for mission control and telemetry
- **Hybrid Architecture**: REST (HTTPS), MQTTS, and WebSocket (WSS)
- **Real-time Streaming** at 1 Hz frequency
- **JWT Authentication** across all protocols
- **Role-Based Access Control**: VIEWER, PILOT, MANAGER

## License

Copyright 2025. All rights reserved.
