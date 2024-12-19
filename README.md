# DJI Mavic 3 Enterprise Documentation Project

> ⚠️ **IMPORTANT**: This is a working document being developed with the assistance of Large Language Models (LLMs). All links, code examples, and technical specifications should be independently verified before use in production environments.

## Documentation Quick Links
- [Research & Technical Findings](documentation/research.md)
- [DJI Terra API Documentation](documentation/terra.md)
- [Waypoint 2.0 Implementation Guide](documentation/waypoint_2.md)

## Overview
This repository contains research and implementation documentation for DJI Mavic 3 Enterprise drone operations, focusing on waypoint missions, RTK integration, and the DJI Terra platform.

## Repository Structure

- `/documentation/`
  - `research.md` - Comprehensive research findings and technical details
  - `terra.md` - DJI Terra API exploration and implementation notes
  - `waypoint_2.md` - DJI Waypoint 2.0 API implementation guide

## Current Status

This documentation is actively being developed and should be considered a work in progress. Key areas include:
- DJI Terra capabilities and limitations
- Waypoint 2.0 API implementation
- RTK integration requirements
- Mission planning for large waypoint sets
- RC Pro controller integration

## Important Notes

1. **LLM Development**: This documentation is being developed with the assistance of AI language models. All technical specifications, code examples, and API references should be verified against official DJI documentation.

2. **Verification Status**:
   - 🔴 Links and URLs need verification
   - 🔴 Code examples need testing
   - 🔴 API endpoints and parameters need confirmation
   - 🔴 Technical specifications need validation

3. **Work in Progress**: This is an active research project, and documentation is continuously being updated and refined.

## Data Directory Structure

The `data/` directory contains drone flight and mission planning files organized as follows:

### External Data (`data/external/`)

Raw input files:
- `template.kml` - DJI Mavic 3 Enterprise waypoint mission template
- `oct_12_exclosure_mission.kml` - Polygon boundary for exclosure study area
- `ninety_nine.csv` - Raw flight data for 99m mission
- `one_thousand.csv` - Raw flight data for 1000m mission

### Interim Data (`data/interim/`)

Processed/intermediate files:
- `ninety_nine.kml` - Processed flight path data with coordinates and altitudes

The KML files contain waypoints, flight paths, and mission boundaries for drone operations. CSV files contain the raw telemetry data that was processed into KML format. 