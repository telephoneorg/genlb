# Generic Load Balancer

## Usage

You will only need to install the unit file it will pull everything else it needs automatically and it will automatically pull the latest version of the image every time it restarts.


**NOTE**: This is a private repo so for docker to pull the image correctly you have to be logged into docker.

##### If not logged in you need to:
```
docker login -u callforamerica -p <pass>
```


### Install the genlb.service systemd unit:
```bash
cp genlb.service /etc/systemd/system/
```

### Create the environment file:
```
echo "UPSTREAM_SERVERS=<ip-address>,<ip-address>,<ip-address>,<ip-address>" >> /etc/default/genlb.env
```

### Start the genlb.service:
```bash
systemctl start genlb
```

### If everything is working set it to auto start:
```bash
systemctl enable genlb
```

## How to

### Get logs
```bash
# without follow
docker logs genlb.service

# with follow
docker logs -f genlb.service

# get logs for the systemd unit supervising the container
journalctl -f -u genlb
```

### Test
```bash
curl -v localhost:80
```

### Get status
```bash
systemctl status genlb
```

### Restart container
```bash
systemctl restart genlb
```

### Stop container
```bash
systemctl stop genlb
```

### Start container
```bash
systemctl start genlb
```

### Disable from auto starting
```bash
systemctl disable genlb
```

### Change the Upstream servers
The upstream servers are read from the environment variable `UPSTREAM_SERVERS` of which the value is a comma delimited list of ip addresses or hostnames.

* Edit the environment file: `/etc/default/genlb.env`.  On the first line you'll see `UPSTREAM_SERVERS=xxx.xxx.xxx.xxx,xxx.xxx.xxx.xxx,etc`.
* Restart the service: `systemctl restart genlb`.
* Check that the service is running with no errors: `systemctl status genlb`

### Stats
Goto the following url: [http://localhost/basic_status](http://localhost/basic_status)

```yaml
Credentials:
  user: admin
  pass: <same as docker login>
```

Stats are really basic on open source nginx and nginx plus is ridiculous. If you need more stats you should use another tool that parses the logs like GoAccess [https://goaccess.io/](https://goaccess.io/)
