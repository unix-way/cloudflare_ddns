# Cloudfare dynamic DNS updater

## Description
The main script is `update_ddns.py` - it can be used either independenty or in a Docker container. The script uses the Cloudflare Python library and is based on the following [example](https://github.com/cloudflare/python-cloudflare/blob/master/examples/example_update_dynamic_dns.py) with the slightest changes. In runs in foreground and attempts to update a given DNS record every six hours. 

## Prerequisites
* Python 3
* Module dependencies
  * CloudFlare
  * requests
  * schedule

## Running

### Command line
Either declare the following variables in your environment:

```
export CF_API_EMAIL=your@email.address
export CF_API_KEY=your_cloudflare_api_key
```

or create a file named `.cloudflare.cnf` with the following content in a directory containing the script"

```
[CloudFlare]
email = your@email.address
token = your_cloudflare_api_key
```

*Optional:* record update interval can be set in environment variable `SCHED_TIME`, which must be a number of seconds. 
If not set or set incorrectly, it defaults to 21600 seconds (6 hours).

Run the script by issuing the following command: `./update_ddns.py your.domain.name`.

### Docker
```
docker run -d -e CF_API_EMAIL=your@email.address \
              -e CF_API_KEY=your_cloudflare_api_key \
              zonked/cloudflare_ddns your.domain.name
```

Or you can mount a configuration file like this:

```
docker run -d -v $(PWD)/.cloudflare.cnf:/app/.cloudflare.cnf:ro \
           zonked/cloudflare_ddns your.domain.name
```
