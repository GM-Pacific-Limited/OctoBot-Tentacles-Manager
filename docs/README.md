# OctoBot-Tentacles-Manager Documentation

## Overview
OctoBot-Tentacles-Manager is a module manager for the OctoBot trading platform. It provides a comprehensive system for managing "tentacles," which are modular extensions or plugins that enhance OctoBot's functionality.

The system enables users to:
- Install, update, and uninstall tentacles
- Configure tentacles through both global and profile-specific settings
- Create and package tentacles for distribution
- Export tentacles to repositories (S3, Nexus)
- Manage dependencies between tentacles

## Core Architecture
OctoBot-Tentacles-Manager is built around several key systems:

### Configuration Management
- `TentaclesSetupConfiguration`: Central class for managing which tentacles are active
- Multi-layered configuration (reference, user-specific, profile-specific)
- Activation state tracking for each tentacle

### Package Management
- Worker classes for each operation (`InstallWorker`, `UpdateWorker`, `UninstallWorker`)
- Package fetching and extraction utilities
- Dependency resolution

### Loading Mechanism
- Discovers and initializes tentacles from the filesystem
- Manages tentacle metadata
- Handles dependencies between tentacles

### Export/Packaging System
- Creates distributable tentacle packages
- Support for single tentacles and tentacle bundles
- Options for different formats (folder, zip) and processing (cythonization)

## Directory Structure
```
octobot_tentacles_manager/
├── api/                  # Public API functions
│   ├── configurator.py   # Configuration API
│   ├── installer.py      # Installation API
│   ├── creator.py        # Creation API
│   └── ...
├── configuration/        # Configuration management
│   ├── tentacles_setup_configuration.py  # Core configuration class
│   └── tentacle_configuration.py         # Individual tentacle config
├── constants.py          # Constants, paths, and structure definitions
├── workers/              # Worker classes for operations
│   ├── install_worker.py
│   ├── uninstall_worker.py
│   └── update_worker.py
├── models/               # Data models
│   ├── tentacle.py       # Tentacle model
│   └── tentacle_package.py  # Package model
├── loaders/              # Tentacle loading functionality
├── exporters/            # Export and packaging
├── uploaders/            # S3 and Nexus upload capabilities
└── util/                 # Utility functions
```

## Tentacles Structure
Tentacles are organized in a structured hierarchy:

```
tentacles/
├── Automation/           # Automation components
│   ├── trigger_events/
│   ├── conditions/
│   └── actions/
├── Backtesting/          # Backtesting components
│   ├── collectors/
│   ├── converters/
│   └── importers/
├── Evaluator/            # Market evaluators
│   ├── TA/               # Technical analysis
│   ├── Social/           # Social media analysis
│   ├── RealTime/         # Real-time data analysis
│   ├── Strategies/       # Trading strategies
│   └── Util/             # Evaluator utilities
├── Meta/                 # Metadata components
├── Services/             # Service components
│   ├── Interfaces/       # User interfaces
│   ├── Notifiers/        # Notification systems
│   └── Services_feeds/   # Data feeds
└── Trading/              # Trading components
    ├── Mode/             # Trading modes
    ├── Exchange/         # Exchange connectors
    └── Supervisor/       # Trading supervisors
```

## Main Operations

### Installation
Tentacles can be installed from various sources:
- Online repositories
- Local packages
- Git repositories

The installation process:
1. Fetches the tentacle package
2. Extracts the contents
3. Resolves dependencies
4. Updates configuration files
5. Registers the tentacle in the system

### Configuration
Tentacles are configured through a layered system:
- Default reference configuration
- User-specific configuration
- Profile-specific configuration

Configuration files define:
- Which tentacles are active
- Tentacle-specific settings
- Dependencies between tentacles

### Packaging & Export
Tentacles can be packaged for distribution:
- As individual tentacles
- As bundles containing multiple tentacles
- With various formats and processing options

### Usage in Profiles
OctoBot supports user profiles, each with its own tentacle configuration:
- Profiles can activate/deactivate different tentacles
- Profile-specific configuration overrides global settings
- Tentacle manager keeps profile configurations in sync

## Usage Examples

### Installing Tentacles

```python
# Using the API
import octobot_tentacles_manager.api as tm_api

# Install all available tentacles
await tm_api.install_all_tentacles(path_or_url="https://tentacles.octobot.online/latest/full",
                                  tentacles_setup_config=tentacles_setup_config)

# Install specific tentacles
await tm_api.install_tentacles(["MyTradingMode", "MyEvaluator"],
                              path_or_url="https://tentacles.octobot.online/latest/full",
                              tentacles_setup_config=tentacles_setup_config)
```

### Configuring Tentacles

```python
# Using the API
import octobot_tentacles_manager.api as tm_api

# Activate specific tentacles
activation_config = {
    "MyTradingMode": True,
    "MyEvaluator": True,
    "OtherEvaluator": False
}
tentacles_setup_config.update_activation_configuration(activation_config, False, False)
tentacles_setup_config.save_config()

# Check if a tentacle is activated
is_active = tentacles_setup_config.is_tentacle_activated("MyTradingMode")
```

### Creating and Packaging Tentacles

```python
# Using the API
import octobot_tentacles_manager.api as tm_api

# Create a tentacle package
await tm_api.create_tentacles_package(output_dir="./output",
                                     tentacles=["MyTradingMode", "MyEvaluator"],
                                     tentacles_setup_config=tentacles_setup_config)
```

## CLI Interface
OctoBot-Tentacles-Manager provides a command-line interface for common operations:

```bash
# Install all tentacles
python octobot_tentacles_manager/cli.py install all

# Install specific tentacles
python octobot_tentacles_manager/cli.py install MyTradingMode MyEvaluator

# Update tentacles
python octobot_tentacles_manager/cli.py update all

# Uninstall tentacles
python octobot_tentacles_manager/cli.py uninstall MyTradingMode

# Create a tentacle package
python octobot_tentacles_manager/cli.py package MyTradingMode MyEvaluator --output ./output
```
