# bulkcp

Bash script to copy files using a CSV file as input.

The CSV file contains the source files to be copied and their destination directory. Files can be copied locally or to a remote host.

## Contents
  - [Installation](#installation)
  - [CSV file](#csvfile)
  - [Usage](#usage)
  - [Options](#options)
  - [FAQ](#faq)

## Installation

Download `bulkcp` into `/usr/local/bin` so that it can be run from anywhere:-

```bash
sudo wget https://raw.githubusercontent.com/madaboutbrighton/bulkcp/master/bulkcp -O /usr/local/bin/bulkcp
sudo chmod a=rx /usr/local/bin/bulkcp
```

## CSV file

The CSV file contains the source files to be copied and their destination directory. It should be in the following format...

| NAME | DONE | SOURCE | DESTINATION DIRECTORY |
|:---|:---:|:---|:---|
| My first directory |   | "/some/folder/My first directory"  | /mnt/usb/data |
| Another directroy |   | "/Some/folder/Another directory" | /mnt/usb/data |
| Favourites  | Y | /another/folder/Favourites | /mnt/usb/data |

A header row is expected, but optional. Use the `--no-header` option if your CSV file does not have a header row. Data inside the CSV file can be double-quoted, but this is not required unless the data contains a comma. 

## Usage

```bash
bulkcp data.csv
```
This would process `data.csv`, copying all source files specified in the CSV file to their corresponding destination directories on the local machine. This process uses the `cp` command.

```bash
bulkcp --remote-host "myuser@192.168.1.123" data.csv
```
This would process `data.csv`, copying all source files specified in the CSV file to their corresponding destination directories on the remote machine, `192.168.1.123`. This process uses the `scp` command. It is only possible to copy files to a remote host if you set up [ssh keys](https://www.cyberciti.biz/faq/how-to-set-up-ssh-keys-on-linux-unix/).

```bash
bulkcp --dry-run data.csv
```
This would process `data.csv` without any changes being made to the file system. Verbose output will be printed to the terminal.

## Options

#### CSV file path

The path of the CSV file to be processed. **This is required**.

#### Dry run `-n `  `--dry-run `

Perform a trial run with **no changes** being made to the file system. Verbose output will be printed to the terminal.

#### No header `--no-header `

Flag to indicate that the CSV file **does not** have a header row.

#### Notify `-t `  `--notify `

Service to notify when script completes. Currently supports a Slack Webhook URL.

#### Remote host `-h `  `--remote-host `

Remote host where the files will be copied to. In the format `name@ipaddress`, for example, `myuser@192.168.1.123`. It is only possible to copy files to a remote host if you set up [ssh keys](https://www.cyberciti.biz/faq/how-to-set-up-ssh-keys-on-linux-unix/).

#### Verbose `-v `  `--verbose`

Increase the amount of information printed to the terminal.

## FAQ

### Why do i need to set up ssh keys to copy files to a remote host?

`bulkcp` uses `scp` to connect to a remote host and `ssh` to issue any other commands, such as `mkdir`. It would be insecure to store the password of your remote host in a variable, and inconvenient to have to enter the password multiple times. It is therefore recommended to use [ssh keys](https://www.cyberciti.biz/faq/how-to-set-up-ssh-keys-on-linux-unix/) when requiring `bulkcp` to copy files to a remote host.
