![Banner](banner.png)

# net-port-service-fingerprint
Attempts to fingerprint services running on specific ports by sending crafted requests and analyzing responses. Goes beyond simple banner grabbing, using protocol-specific probes. Focused on basic network operations.

## Install
`git clone https://github.com/ShadowGuard-AI/net-port-service-fingerprint`
`pip install -r requirements.txt`

## Usage
`python3 main.py HOST PORT`

## Parameters
- `-h`: Show help message and exit
- `host`: The hostname or IP address to scan. Required.
- `port`: The port number to scan. Required, an integer from 1 to 65535.

## Verified behavior
- A run against a local HTTP server on 127.0.0.1:8932 printed "HTTP Server: SimpleHTTP/0.6 Python/3.12.3".
- A run against a refused local port printed one connection refused error per probe, then "Unknown service running on 127.0.0.1:8931" and exited 0. Refusal is reported as unknown, not as an error exit.
- Probes run in a fixed order, HTTP, then SSH, then SMTP, each opening a fresh connection, so a silent service can burn a full 5 second timeout per probe before the next one tries.
- An invalid host or a port of 0 prints an error and exits 1. A non-integer port is rejected by argparse with exit 2.
- Host validation is lenient: shorthand forms such as 127.1 parse as addresses, and anything else falls through to a DNS lookup.

## License
See LICENSE.
