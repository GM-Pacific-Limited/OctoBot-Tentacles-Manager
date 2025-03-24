# OctoBot-Tentacles-Manager Modules

## Configuration
The `configuration` module manages tentacle configuration:
- `tentacles_setup_configuration.py`: Core configuration class
- `tentacle_configuration.py`: Individual tentacle configuration
- `config_file.py`: Configuration file operations

## Workers
The `workers` module contains worker classes for operations:
- `install_worker.py`: Handles tentacle installation
- `uninstall_worker.py`: Handles tentacle removal
- `update_worker.py`: Handles tentacle updates
- `tentacles_worker.py`: Base worker class

## Models
The `models` module defines data models:
- `tentacle.py`: Tentacle model
- `tentacle_package.py`: Package model
- `tentacle_factory.py`: Creates tentacle instances
- `tentacle_type.py`: Defines tentacle types
- `tentacle_requirements_tree.py`: Dependency management

## Loaders
The `loaders` module handles tentacle loading:
- `tentacle_loading.py`: Loads tentacles from filesystem

## Exporters
The `exporters` module manages tentacle packaging:
- `tentacle_exporter.py`: Exports tentacles
- `tentacle_package_exporter.py`: Creates packages
- `tentacle_bundle_exporter.py`: Creates bundles
- `artifact_exporter.py`: Base exporter class

## Uploaders
The `uploaders` module handles package distribution:
- `s3_uploader.py`: Uploads to Amazon S3
- `nexus_uploader.py`: Uploads to Nexus repository

## Utilities
The `util` module provides helper functions:
- `tentacle_fetching.py`: Fetches and extracts tentacles
- `tentacle_explorer.py`: Explores tentacle structure
- `tentacle_cleaner.py`: Cleans tentacle files
