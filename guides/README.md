# Ransomwatch Infrastructure Documentation

This documentation provides a comprehensive analysis of the ransomwatch project infrastructure, architecture, and operational aspects.

## Table of Contents

1. [How Ransomwatch Works](./how-it-works.md)
2. [Infrastructure Diagrams](./infrastructure-diagrams.md)
3. [GitHub Actions Orchestration](./github-actions-orchestration.md)
4. [Infrastructure Critique](./infrastructure-critique.md)
5. [Data Storage and Frontend](./data-storage-frontend.md)

## Overview

Ransomwatch is an automated monitoring system that tracks ransomware group extortion sites and surfaces an aggregated feed of victim claims. The system operates entirely on GitHub infrastructure using GitHub Actions for orchestration, GitHub Pages for hosting, and GitHub Container Registry for Docker images.

## Key Features

- **Automated Monitoring**: Crawls 492+ sites belonging to 216+ unique ransomware groups
- **Real-time Updates**: Runs every 2 hours via GitHub Actions
- **Multi-format Output**: JSON APIs and web interface
- **Tor Support**: Uses Tor proxy for accessing .onion sites
- **Parser System**: 140+ custom parsers for extracting victim data
- **Social Integration**: Discord, Teams, and Twitter notifications

## Architecture Highlights

- **Serverless**: No dedicated infrastructure required
- **Git-based Storage**: All data stored in Git repository
- **Container-based**: Uses Docker for consistent execution environment
- **Static Site**: Frontend served via GitHub Pages with Docsify