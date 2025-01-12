# Net-Mon

- Updated apprise to the latest version to support latest notification methods (like Ntfy)

Get notified for new devices on your network. This app runs [nmap](https://nmap.org/) periodically and saves found hosts, and send you a notification whenever a new device (mac-address) is found.

![](/assets/screenshot.jpg)

## Prerequisites
- A notification service supported by [Apprise](https://github.com/caronc/apprise#popular-notification-services) and the required API keys or other configuration for your chosen services
- Have [Python](https://www.python.org/) 3.8+ or [Docker](https://www.docker.com/) installed

## Installation
This app can be used stand-alone and run with Python, or it is also available as a Docker image.
If using Python, install the requirements first:
`pip install -r requirements.txt`

## Configuration
All configuration is done via environment variables:
1. Apprise configuration url, for your chosen providers:
`NETMON_NOTIFICATION=tgram://bottoken/ChatID`
2. Subnet for scanning in CIDR form or range form:
`NETMON_SUBNET=192.168.1.0/24` or `NETMON_SUBNET=192.168.1.1-100`
3. Interval for scanning, in minutes:
`NETMON_MINUTES=15`


## Usage
- Stand-alone:
	`sudo python app.py`
- Docker:
	```
    docker run -e \
        NETMON_NOTIFICATION=tgram://bottoken/ChatID \
        NETMON_SUBNET=192.168.1.0/24 \
        NETMON_MINUTES=15 \
        --net=host \
        vetusto/net-mon:1.0
    ```
- Docker-Compose:
    ```
    version: "3.8"

    services:
        net-mon:
            container_name: net-mon
            image: vetusto/net-mon:1.0
            restart: unless-stopped
            network_mode: host # needed for nmap to get mac addresses
            volumes:
            - ./results.json:/app/results.json # optional, if you want to keep found hosts persistent
                                               # create an empty results.json first
            environment:
            - NETMON_NOTIFICATION=tgram://bottoken/ChatID
            - NETMON_SUBNET=192.168.1.0/24
            - NETMON_MINUTES=60
    ```

## License
[MIT](https://choosealicense.com/licenses/mit/)
