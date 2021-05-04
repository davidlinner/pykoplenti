# Python Library for Accessing Kostal Plenticore Inverters

This repository provides a python library and command line interface for the REST-API of Kostal Plenticore Solar Inverter.

This library is not affiliated with Kostal and is no offical product. It uses the interfaces of the inverter like other libs (eg. https://github.com/kilianknoll/kostal-RESTAPI) and uses information from their swagger documentation (<ip>/api/v1/).

![CI](https://github.com/stegm/kostal_plenticore/workflows/CI/badge.svg)

## Features

* Authenticate
* Read/Write settings
* Read process data
* Read events
* Download of log data
* Full async-Support for reading and writing data
* Commandline interface for shell access
* Dynamic data model

## Getting Started

### Prerequisites

You will need Python >=3.7.

### Installing the library

Packages of this library are released on [PyPI](https://pypi.org/project/kostal-plenticore/) and can be 
installed with `pip`. Alternatively the packages can also be downloaded from 
[GitHub](https://github.com/stegm/kostal_plenticore/releases/).


I recommend to use a [virtual environment](https://docs.python.org/3/library/venv.html) for this, 
because it installs the dependecies independently from the system. The installed CLI tools can then be called
without activating the virtual environment it.

```shell
# Install with command line support
$ pip install kostal_plenticore[CLI]

# Install without command line support
$ pip install kostal_plenticore
```

### Using the command line interface


Installing the libray with `CLI` provides a new command.

```shell
$ plenticore --help
Usage: plenticore [OPTIONS] COMMAND [ARGS]...

  Handling of global arguments with click

Options:
  --host TEXT           hostname or ip of plenticore inverter
  --port INTEGER        port of plenticore (default 80)
  --password TEXT       the password
  --password-file TEXT  password file (default "secrets" in the current
                        working directory)

  --help                Show this message and exit.

Commands:
  all-processdata   Returns a list of all available process data.
  all-settings      Returns the ids of all settings.
  read-processdata  Returns the values of the given process data.
  read-settings     Read the value of the given settings.
  repl              Provides a simple REPL for executing API requests to...
  write-settings    Write the values of the given settings.
```

Visit [Command Line Help](doc/command_line.md) for example usage.
 
### Using the library from python

The library is fully async, there for you need an async loop and an async `ClientSession`. Please refer to the
example directory for full code.

Import the client module:

```python
from kostal.plenticore import PlenticoreApiClient
```

To communicate with the inverter you need to instantiate the client:
 
```python
# session is a aiohttp ClientSession
client = PlenticoreApiClient(session, '192.168.1.100')
```

Login to gain full access to process data and settings:

```python
await client.login(passwd)
```

Now you can access the API. For example to read process data values:

```python
data = await client.get_process_data_values('devices:local', ['Inverter:State', 'Home_P'])

device_local = data['devices:local']
inverter_state = device_local['Inverter:State']
home_p = device_local['Home_P']
```

See the full example here: [read_process_data.py](examples/read_process_data.py).


## Documentation

*  [Command Line Interface](doc/command_line.md)
*  [Examples](examples/)

## Built With

* [AIOHTTPO](https://docs.aiohttp.org/en/stable/) - asyncio for HTTP
* [click](https://click.palletsprojects.com/) - command line interface framework

## License

apache-2.0

## Acknowledgments



* [kilianknoll](https://github.com/kilianknoll) for the kostal-RESTAPI project 
