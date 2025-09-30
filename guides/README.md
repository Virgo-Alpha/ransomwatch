# Ransomwatch Infrastructure Documentation

This documentation provides a comprehensive analysis of the ransomwatch project infrastructure, architecture, and operational aspects, including migration strategies and alternative deployment options.

## Table of Contents

### Core Architecture
1. [How Ransomwatch Works](./how-it-works.md)
2. [Infrastructure Diagrams](./infrastructure-diagrams.md)
3. [GitHub Actions Orchestration](./github-actions-orchestration.md)
4. [Infrastructure Critique](./infrastructure-critique.md)
5. [Data Storage and Frontend](./data-storage-frontend.md)

### Cloud Migration Strategies
6. [Tor Integration and AWS Replication](./tor-integration-aws-replication.md)
7. [Serverless Lambda Architecture](./serverless-lambda-architecture.md)
8. [GitHub to GitLab Migration](./github-to-gitlab-migration.md)

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

## Migration and Scaling Options

### AWS Cloud Migration
- **[ECS Fargate Approach](./tor-integration-aws-replication.md)**: Traditional container orchestration with VPC networking ($85-130/month)
- **[Serverless Lambda](./serverless-lambda-architecture.md)**: Step Functions + Lambda with DynamoDB/Aurora storage ($49-86/month)

### Platform Migration
- **[GitLab Migration](./github-to-gitlab-migration.md)**: Enhanced security with private repositories and advanced DevOps features ($51-84/month)

## Cost Comparison Summary

| Architecture | Monthly Cost | Key Benefits | Best For |
|--------------|--------------|--------------|----------|
| **Current GitHub** | $0 | Free, transparent, simple | Public projects, cost-sensitive |
| **AWS ECS** | $85-130 | Unlimited runtime, traditional architecture | Enterprise, long-running tasks |
| **AWS Lambda** | $49-86 | 3x faster execution, auto-scaling | Modern serverless, cost-effective |
| **GitLab + AWS** | $51-84 | Private repos, enhanced security | Security-focused, compliance needs |