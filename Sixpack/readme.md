# Sixpack Setup

> [Sixpack](http://sixpack.seatgeek.com/) is a framework to enable A/B testing across multiple programming languages. It does this by exposing a simple API for client libraries. Client libraries can be written in virtually any language.

We use it to help run various experiments on content, design and interface elements that help inform what our users prefer and respond better to.

## Requirements
- Redis >= 2.6
- Python >= 2.7

## Installation

#### Step 1: Install Python Tools & Sixpack
On your local [Homestead](https://github.com/DoSomething/communal-docs/tree/master/Homestead) virtual environment, SSH into the box and install the python development tools:

```shell
$ sudo apt-get install python-dev
```

Then install **sixpack** from the DoSomething fork:

```shell
$ sudo pip install -e git+https://github.com/DoSomething/sixpack.git#egg=sixpack
```

#### Step 2: Create Sixpack Configuration
Create and save the following configuration file in `/etc/sixpack/config.yml`:

```yml
redis_port: 6379                            # Redis port
redis_host: localhost                       # Redis host
redis_prefix: sixpack                       # all Redis keys will be prefixed with this
redis_db: 15                                # DB number in redis

metrics: false                              # send metrics to StatsD (response times, # of calls, etc)?
statsd_url: 'udp://localhost:8125/sixpack'  # StatsD url to connect to (used only when metrics: true)

# The regex to match for robots
robot_regex: $^|trivial|facebook|MetaURI|butterfly|google|amazon|goldfire|sleuth|xenu|msnbot|SiteUptime|Slurp|WordPress|ZIBB|ZyBorg|pingdom|bot|yahoo|slurp|java|fetch|spider|url|crawl|oneriot|abby|commentreader|twiceler
ignored_ip_addresses: []                    # List of IP

asset_path: gen                             # Path for compressed assets to live. This path is RELATIVE to sixpack/static
secret_key: '<your secret key here>'        # Random key (any string is valid, required for sixpack-web to run)
```

**Make sure to update the `<your secret key here>` with a custom random string key.**

#### Step 3: Start Sixpack Server
You should now be able to start the Sixpack server:

```shell
$ SIXPACK_CONFIG=/etc/sixpack/config.yml sixpack &
```

And also start the Sixpack web experience dashboard:

```shell
$ sudo SIXPACK_CONFIG=/etc/sixpack/config.yml sixpack-web &
```

_Note: the `&` at the end of each command, just tells the shell to run the command in the background._

If you need to terminate either or both of the server processes running, you can do so using `pkill`:

```
$ pkill sixpack
```

```
$ pkill sixpack-web
```

You _may_ have to use `sudo` to run the above commands.

#### Step 4: Set Local Host
To easily access Sixpack and the Sixpack web interface in the browser add the following line to your `/etc/hosts` file:

```
192.168.10.10 sixpack.test
```

You can now access Sixpack at `sixpack.test:5000` to confirm it is running and access the Sixpack web interface at `sixpack.test:5001`.
