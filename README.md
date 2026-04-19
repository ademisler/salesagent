# SalesAgent

SalesAgent is a WordPress plugin that adds a proactive AI sales assistant to a website.

The repository root contains the distribution wrapper, while the plugin source lives in `asa-ai-sales-agent/`.

## What It Does

- Starts contextual conversations based on the page a visitor is viewing
- Uses Google Gemini for AI responses
- Supports mobile-first chat interactions
- Lets you customize colors, positioning, and behavior
- Stores chat history locally for a lightweight experience

## Repository Layout

- `asa-ai-sales-agent/` - main plugin source
- `assets/` - plugin assets and generated images
- `README.md` - repository overview

## Requirements

- WordPress 5.0+
- PHP 7.4+
- Google Gemini API key

## Installation

1. Copy the `asa-ai-sales-agent/` folder into `wp-content/plugins/`
2. Activate the plugin in WordPress admin
3. Add your Gemini API key in the plugin settings
4. Configure the assistant behavior and appearance

## Development

The plugin source inside `asa-ai-sales-agent/` contains the main PHP entry point, assets, and documentation.

## License

GPL v2 or later
