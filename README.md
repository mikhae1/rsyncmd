# rsyncmd

Remote sync and run helper

The idea is to create helper script to copy your current working directory to any SSH-enabled host and be able to launch any commands afterwards.
This can greatly help for development and troubleshooting in variety of situations:

- long-running or CPU-intensive tasks
- multi-OS development
- local security limitations
- backup and access existing development environment from multiple machines or locations

This script is designed to be used independently or with any IDE you like.

## Dotenv

You could use `.env` files to set all your variables. Store this files at your project directories.

## Installation

- Install [Rsync](https://wiki.archlinux.org/index.php/Rsync)
- Setup SSH login to remote server
- You may want to copy `rsyncmd` to your `bin` `PATH`:

      $ cp ./rsyncmd /usr/local/bin

## Variables

    SRС - source directory
    DST - remote destination folder for $SRC rsyncing
    CMD - command to run remotely at $DST folder
    SSH_ALIVE_IN - SSH keep alive interval
    SSH_OPTS - SSH options
    RSYNC_OPTS - Rsync options
    RSYNC_SKIP - Rsync excluded directories
    SRV - remote server to rsync $SRC, connect and run $CMD

## VSCode integration example

### tasks.json

```json
{
  "version": "2.0.0",
  "inputs": [
    {
      "type": "pickString",
      "description": "Ssh host to sync and run command remotely",
      "id": "ssh_host",
      "options": ["user@centos", "user@ubuntu"],
    }
  ],
  "tasks": [
    {
      "label": "ansible - remote",
      "type": "shell",
      "command": "/path/to/rsyncmd",
      "problemMatcher": [],
      "group": {
        "kind": "build",
        "isDefault": true
      },
      "options": {
        "env": {
          "CMD": "ansible-playbook -i 127.0.0.1, play.yml",
          "SRV": "${input:ssh_host}",
          "SRC": "${workspaceFolder}"
        }
      }
    }
  ]
}

```
