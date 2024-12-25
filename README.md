# Web Application Honeypot System

A honeypot system designed to detect new attacks on web applications by emulating popular CMS platforms. It attracts potential attackers and analyzes their behavior while logging all interactions.

## Features

- Emulation of popular CMS platforms (currently supports WordPress)
- Legitimate search engine traffic filtering (Google, Yandex, Bing)
- Detection of various attack types:
  - SQL injections
  - XSS (Cross-Site Scripting)
  - Path Traversal
  - Shell injection attempts
- Advanced logging in Apache Combined Log Format
- Prometheus metrics collection
- Real-time monitoring capabilities

## Requirements

### System Requirements
- Python 3.8+
- Open ports 8000 (web server) and 9090 (Prometheus metrics)
- Minimum 512MB RAM
- 1GB free disk space (for logs)

### Python Dependencies
```bash
fastapi>=0.68.0
uvicorn>=0.15.0
prometheus-client>=0.11.0
aiofiles>=0.8.0
```

## Installation

1. Clone the repository:
```bash
git clone [repository-url]
cd web-honeypot
```

2. Create and activate virtual environment:
```bash
python -m venv venv
source venv/bin/activate  # Linux/Mac
venv\Scripts\activate     # Windows
```

3. Install dependencies:
```bash
pip install -r requirements.txt
```

## Running the System

1. Basic start with default settings:
```bash
python honeypot.py
```

2. Advanced start with custom parameters:
```bash
python honeypot.py --host 0.0.0.0 --port 8000 --metrics-port 9090 --log-dir ./logs
```

## Log Structure

### Access Log
Located in `logs/access.log`, contains entries in Apache Combined Log Format:
```
127.0.0.1 - - [25/Dec/2024:12:34:56 +0000] "GET /wp-login.php HTTP/1.1" 200 1234 "-" "Mozilla/5.0..."
```

### Attack Log
Located in `logs/attacks.json`, contains detailed attack information:
```json
{
    "timestamp": "2024-12-25T12:34:56",
    "ip": "127.0.0.1",
    "method": "POST",
    "path": "/wp-login.php",
    "headers": {},
    "payload": "username=admin&password=1234",
    "query_params": {},
    "response_code": 200,
    "response_time": 0.123,
    "cms_type": "wordpress",
    "cms_version": "5.8.2"
}
```

## Prometheus Metrics

Available on port 9090, including:
- `honeypot_requests_total` - total number of requests
- `honeypot_response_time_seconds` - response time metrics
- `honeypot_attacks_total` - number of detected attacks

## Emulated CMS Platforms

### WordPress
- Version: 5.8.2
- Emulated endpoints:
  - /wp-login.php
  - /wp-admin/
  - /wp-json/
  - /xmlrpc.php
  - /wp-content/
  - /wp-includes/

## Security Features

- Read-only operation mode
- No code execution
- No storage of uploaded files
- Complete logging of all attack attempts

## Integration with External Systems

### Prometheus
Metrics available at:
```
http://your-server:9090/metrics
```

### Grafana
Recommended dashboards:
- Honeypot Overview
- Attack Detection
- Response Time Analysis

## Future Development

- [ ] Add Joomla support
- [ ] Add Bitrix support
- [ ] Implement reverse DNS checking for bots
- [ ] Add notification system (email, Slack)
- [ ] Integrate with threat analysis systems

## Contributing

To contribute to the project:
1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Submit a Pull Request

## Architecture

The system consists of several key components:

1. Core Honeypot Server:
   - FastAPI-based web server
   - CMS emulation modules
   - Request handling middleware

2. Traffic Analysis:
   - Bot detection system
   - Attack pattern recognition
   - Payload analysis

3. Monitoring System:
   - Prometheus metrics
   - Logging subsystem
   - Real-time analytics

## Performance Considerations

- Minimal resource footprint
- Efficient request handling
- Optimized log rotation
- Non-blocking I/O operations

## Troubleshooting

Common issues and solutions:
1. Port conflicts:
   ```bash
   lsof -i :8000  # Check if port 8000 is in use
   lsof -i :9090  # Check if port 9090 is in use
   ```

2. Log directory permissions:
   ```bash
   chmod 755 logs/  # Set correct permissions
   ```

3. Python environment issues:
   ```bash
   python -V  # Verify Python version
   pip list   # Check installed packages
   ```

## License

MIT

## Author

opermanus

## Support

For support inquiries:
- Open an issue in the repository
- Contact the maintainers
- Check the documentation
