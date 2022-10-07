# Cronic v3 - cron job report wrapper

Utility to simplify running and maintaining cronjobs.

## Usage

### Basic usage

To run a task and discard the output unless it fails or outputs to STDERR:
```bash
cronic /path/to/mytask
```

### Enhanced usage

To run a task, always log to a file and only output to stdout if the task fails or outputs to STDERR
```bash
CRONIC_DIR=/var/log/cronic CRONIC_NAME=mytask cronic /path/to/mytask
```

### Timeout

By default, commands timeout after 1 hour. A custom number of seconds can be set using `CRONIC_TIMEOUT`:
```bash
CRONIC_TIMEOUT=2h /run/my/task
```

A value of `0` disables said timeout.

### Logging

Each execution will log output to timestamped `*.out`, `*.err` and `*.trace` files in the directory `${CRONIC_DIR}/${CRONIC_NAME}`. Presuming cronic itself survives these will be combined post-run to a single `*.log` file in the same folder.

By default, `CRONIC_DIR` is set to `/var/log/cronic`, and `CRONIC_NAME` is set to the first parameter to cronic (the task executable).

Any log files older than 60 days in this directory will be deleted at the end of each cronic run.

### Output

Cronic is intended to be used as part of cronjobs, which are presumed to have failed whenever STDOUT or STDERR are not empty.

As such, cronic is silent and discards the task output unless the task:
- Times out;
- Outputs anything to STDERR; or
- Quits with a non-zero exit code.

In such case, the STDOUT and STDERR from the task will be merged and output to cronic's STDOUT, together with information about the task's exit code. If running under cron, this will become the e-mail body.

## Authors / License

Public Domain CC0: http://creativecommons.org/publicdomain/zero/1.0/

- Copyright 2007 Chuck Houpt
- Amended 2013 Andrew Coulton
- Amended 2021 & 2022 Rui Pinheiro