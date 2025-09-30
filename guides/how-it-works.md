# How Ransomwatch Works

## System Overview

Ransomwatch is a comprehensive ransomware monitoring system that automatically tracks and indexes victim claims from ransomware group leak sites. The system operates as a fully automated pipeline that scrapes, parses, and publishes data about ransomware activities.

## Core Components

### 1. Main Controller (`ransomwatch.py`)
The central orchestrator that provides four main operations:
- **add**: Add new ransomware groups and their sites
- **scrape**: Fetch HTML content from monitored sites
- **parse**: Extract victim information from scraped content
- **markdown**: Generate documentation and visualizations

### 2. Web Scraping System

#### Standard Scraping (`sharedutils.py`)
- Uses `requests` library with SOCKS5 proxy for Tor routing
- Handles HTTP/HTTPS requests through Tor network
- Implements random user agent rotation
- Supports timeout and error handling

#### JavaScript Rendering (`geckodrive.py`)
- Uses Selenium WebDriver with Firefox for sites requiring JavaScript
- Configured for headless operation
- Supports Tor proxy configuration
- Handles dynamic content loading with wait times

### 3. Data Storage Structure

#### Groups Configuration (`groups.json`)
```json
{
  "name": "group_name",
  "captcha": false,
  "parser": true,
  "javascript_render": false,
  "meta": "additional_notes",
  "locations": [
    {
      "fqdn": "example.onion",
      "title": "Site Title",
      "version": 3,
      "slug": "http://example.onion",
      "available": true,
      "updated": "2025-06-17",
      "lastscrape": "2025-06-17 12:00:00",
      "enabled": true
    }
  ],
  "profile": ["reference_links"]
}
```

#### Posts Database (`posts.json`)
```json
{
  "post_title": "Victim Company Name",
  "group_name": "ransomware_group",
  "discovered": "2025-06-17 12:00:00.000000"
}
```

### 4. Parsing System (`parsers.py`)

The parsing system contains 140+ custom parsers, each tailored to specific ransomware group site structures:

#### Parser Architecture
- **Shell-based**: Uses grep, awk, sed, and Perl for text extraction
- **Group-specific**: Each parser handles unique HTML structures
- **Robust**: Includes error handling and validation
- **Extensible**: Easy to add new parsers for new groups

#### Example Parser (Simplified)
```python
def lockbit3():
    stdlog('parser: lockbit3')
    parser = '''
    grep '<div class="post-title">' source/lockbit3-*.html -C 1 --no-filename | 
    grep '</div>' | cut -d '<' -f 1 | 
    sed -e 's/^ *//g' -e 's/[[:space:]]*$//' | 
    sort --uniq | tr '[:upper:]' '[:lower:]'
    '''
    posts = runshellcmd(parser)
    for post in posts:
        appender(post, 'lockbit3')
```

### 5. Documentation Generation (`markdown.py`)

Automatically generates multiple documentation formats:
- **Main page**: Summary statistics and overview
- **Index page**: Complete site listing with status
- **Recent posts**: Latest victim claims
- **Stats page**: Graphs and visualizations
- **Profile pages**: Individual group details

### 6. Visualization System (`plotting.py`)

Creates data visualizations using matplotlib:
- Posts per day trend analysis
- Posts by group bar charts
- Group activity pie charts
- 7-day activity summaries

## Data Flow Process

### 1. Scraping Phase
```
GitHub Actions Trigger → 
Tor Proxy Container Start → 
Site List Iteration → 
Content Fetching (Standard/JS) → 
HTML Storage in /source/
```

### 2. Parsing Phase
```
HTML Files → 
Group-specific Parsers → 
Victim Extraction → 
Duplicate Detection → 
Posts.json Update → 
Social Notifications
```

### 3. Documentation Phase
```
Data Processing → 
Statistics Calculation → 
Markdown Generation → 
Graph Creation → 
GitHub Pages Deployment
```

## Site Monitoring Logic

### Site Classification
- **Version Detection**: Automatically detects Tor v2/v3 onion services
- **Capability Flags**: Tracks captcha, JavaScript requirements
- **Status Monitoring**: Maintains availability and last-seen timestamps

### Error Handling
- **Timeout Management**: 35-second request timeouts
- **Connection Errors**: Graceful handling of network issues
- **DNS Resolution**: Proper Tor DNS routing for .onion sites
- **Rate Limiting**: Respects site limitations and proxy constraints

## Security Considerations

### Tor Integration
- **DNS Leak Prevention**: Uses socks5h:// protocol
- **Proxy Validation**: Checks Tor proxy availability before operations
- **User Agent Rotation**: Randomized browser fingerprints
- **Certificate Handling**: Accepts self-signed certificates

### Data Sanitization
- **Length Limits**: Post titles limited to 90 characters
- **Character Encoding**: UTF-8 support for international content
- **HTML Escaping**: Prevents injection in generated content
- **Input Validation**: Validates URLs and data formats

## Scalability Features

### Modular Design
- **Parser Independence**: Each group parser operates independently
- **Incremental Updates**: Only processes changed data
- **Stateless Operations**: No persistent connections or sessions
- **Container Isolation**: Each run operates in clean environment

### Performance Optimizations
- **Conditional Rendering**: JavaScript rendering only when needed
- **Parallel Processing**: Multiple parsers can run simultaneously
- **Caching Strategy**: Reuses existing data when possible
- **Efficient Storage**: Git-based versioning and compression