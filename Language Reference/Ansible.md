## Configuration Files
- Environment Variables
- Config file in the working directory
- default directory

## Inventory

You need ssh access

located in /etc/ansible/hosts

inventory parameters:

- ansible_host: aliase for server IP/FQDN
- ansible_connection: ssh or winrm
- ansible_port:
- ansible_user: user ansible uses
- ansible_ssh-pass:

## Inventory Formats
- .ini: simplest and most straightfoward
- .yaml: more structed and flexible

## Grouping and Parent Child Relationships

use labels to target a group of a servers at the same time

## Variables

store information that vary with each host

can be included in the playbook or in a separate file

## Variable Precedence

host variables take precednce over group variables

but playbook overrides host and group

but if you pass in a var through the command line, that takes presesndnce over the play book vars

## Variable Scoping

host scope

play scope

global

## Magic Variables
- hostvars: variables form other hosts
- groups: returns all hosts of the that groups
- group_names: returns all groups for a host
- inventory_hostname: returns name configured for host in the inventory files

## Facts

Facts are bits of information collected from the server once ansible connects to it

Uses setup module (run automatically)

Facts are stored in variable "ansible_facts"

Can disable facts by used gather_facts: no

Can also be setup in the ansible configuration file (gathering = implicit)

Facts only gathered for hosts part of the playbook

## Playbooks

A single yaml containing a set of plays

a play defines a set of task to run on a host/set of hosts

an action to perform on the host

the yaml format

playbook is an list (or array) of dictionaries

each dictionary has a hosts, name and tasks key

The tasks is a list of dictionaries

### Modules

action run by a task ("command" is the module in the example below)

```yaml
tasks:
  - name: Execute command 'data'
	command: date
```

### Running Playbooks

```
ansible-playbook <playbook file name>
```

## Verifying Playbooks
### Check Mode

dry run where no actual changes to the hosts

use --check option when running the playbook

### Diff Mode

Shows a before and after

user the --diff option

### Syntax Check

use --syntax-check option

## Ansible Lint

command line style checker

```
ansible-lint playbook.yml
```

## Conditionals

run certain tasks based on conditions

also can be used in loops

## Conditionals Based on Facts, Etc.
## Loops
### Loop
### With_*
## Modules
### Groups
- system: system level stuff: firewalls, etc
- commands: commands and scripts
- Files:
- Database
- Cloud: different cloud provider helpers
- Windows:

### Exmamples
#### Command

```yaml # basic command task
tasks:
  - name: execute 'date' commmand
    command: date
```

```yaml
tasks: # these tasks do the same thing, but the second use parameters
  - name: Display resolv.conf contents
	command: cat /etc/resolv.conf

  - name: Dipslay resolv.conf contents
    command: cat resolv.conf chdir=/etc
```

free form parameter:

#### Script

executes a local script on a remove node after transferring it

#### Service

start vs started - idempotency (a command can be run multiple times without modified )

started = ensure the service is started

#### Lineinfile

serach for a line and replace or add it to a file

it is idempotent

## Plugins

help with cloud stuff

## Modules and Plugins Index
## Handlers
- tasks triggered by events/notifications
- defined in playbook , executed when notified by a task

## Roles

assigning a role means doing everything the server needs to do to accomplish it's purpose

it is a set of tasks, and it basically as playbook, that is typically set as constant and can be easily reused

## Collections

collections of plugins, collections,

## Templating

Jinja2 - templating engine for python