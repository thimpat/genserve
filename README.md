> Genserve (server generator) allows spawning servers in the background.
>
> It is optimised to use with a CI system, in a development environment, etc.

## Installation

```shell
npm install genserve -g
```

<br/>

> ##### Note: Do not install from Github. 
> The repo version will be available after setting up the CI.

---

## Usage

### In a Terminal

```shell
$> genserve [command] [target] [--port number] [--dir path] [--silent] [--timeout number] [--apiport 
number] [--disableapi]
```

<br/>

### In a script

#### For CommonJs

```javascript
// CommonJs
const {startGenServer, stopGenServer, runCommand} = require("genserve");

await startGenServer({...});                    // Start a server
await stopGenServer({...});                     // Stop a server
const json = await infoGenServer({...});        // Get server info in a JSON formatted string

```

<br/>

#### For ESM

```javascript
// ESM
import {startGenServer, stopGenServer, infoGenServer} from "genserve";

await startGenServer({...});
await stopGenServer({...});
const json = await infoGenServer({...})
```

<br/>

---

## Overview

###### Launch a server in the background and run some tests (using that server) in the same terminal

> * **Step 1: Spawn a non-blocking server**
> ```shell 
> $> genserve start server
> ```
> * **Step 2: Run tests (from the same terminal)**
> ```shell 
> $> npm test
> ```
> * **Step 3: Stop the server**
> ```shell 
> $> genserve stop server
> ```


![tests-from-shell.gif](https://github.com/thimpat/genserve/blob/main/tests-from-shell.gif)

<br/>

---

## Commands

> In the descriptions below,
> **[servername]** must be replaced by any name.

---

### start

Start a server named [servername] on any available port.

When [servername] is missing, it uses "default" as a name.

```shell
# [servername] must be replaced by any name you wish to set
$> genserve start [servername]
```

---

### restart

Restart a server

```shell
$> genserve restart [servername]
```

---

### stop

Stop a server

```shell
$> genserve stop [servername]
```

---

### remove

Remove a server from registered server list

```shell
$> genserve remove [servername]
```

---

### lock

Prevent a server to be stopped from the registered server list.
The commands **stop** _[all]_, **start** _[all]_, **restart** _[all]_, **set** _[all]_ will have no effect on this
server.

```shell
$> genserve lock [servername]
```

or

```shell
$> genserve start [servername] --lock
```

---

### protect

Prevent a server to be removed from the registered server list.
The commands **flush**, **remove** _[all]_ will have no effect on this server.

```shell
$> genserve protect [servername]
```

or

```shell
$> genserve start [servername] --protect
```

---

### log

Open log for a server

```shell
$> genserve log [servername]
```

---

### list

Show list of created named servers.

```shell
$> genserve list
```

---

### save

Save named servers current list

```shell
# Save server list into a ./testfile.json file
$> genserve save testfile
```

---

### load

Load named servers from files

```shell
# Load server list from ./testfile.json file
$> genserve load testfile
```

---

### scan

Show a list of servers and show which ones are running

```shell
$> genserve scan
```

---

### flush

Remove all registered servers (must be stopped and unprotected).

```shell
$> genserve flush
```

---

### edit

Edit session file in an editor

```shell
$> genserve edit servers          # Edit settings for registered servers

$> genserve edit settings         # Edit global settings
```

---

### set

Modify server property without starting it.

> Note: Users must stop the server before being able to change properties.

```shell
$> genserve set tom --port 6000 --dir public 
```

## Batch Operation

<br/>

### start all

Start all registered and unlocked servers

```shell
$> genserve start all
```

---

### restart all

restart all registered and unlocked servers

```shell
$> genserve restart all
```

---

### stop all

Stop all running and unlocked servers

```shell
$> genserve stop all
```

---

### remove all

Remove all registered and unprotected servers

```shell
$> genserve remove all
```

---

### status all

Display all registered servers statuses

```shell
$> genserve status all
```

<br/>

---

## Options

#### Access to server options

###### To obtain the path to the file that holds all of the servers configurations:

```shell
$> genserve path servers
```

###### To edit it

```shell
$> genserve edit servers
```

<br/>

---

### Options modifiable via CLI

> The options below can be used with the start, restart, set commands

| **Options**   | **default** | **Expect** | **Description**                  | 
|---------------|-------------|------------|----------------------------------|
| --help        | false       | boolean    | _Display help_                   |
| --version     | false       | boolean    | _Display version_                |
| --dir         | .           | path       | _Directory to serve_             |
| --port        | random      | integer    | _Port to run the server on_      |
| --protocol    | http://     | string     | _Protocol used by server_        |
| --apiport     | random      | integer    | _Port to control server_         |              
| --defaultPage | index.html  | string     | _Default page to serve_          |
| --silent      | false       | boolean    | _Whether to display messages_    |           
| --disableapi  | false       | boolean    | _Disable server API_             |           
| --open        | ""          | string     | _Open browser on specified path_ |           
| --ssl         | false       | boolean    | _Enable https_                   |           
| --cert        | ""          | string     | _Path to ssl cert file_          |           
| --key         | ""          | string     | _Path to ssl key file_           |           

<br/>

#### SSL options

> To enable SSL:

```shell
$> genserve --ssl --cert /path/to/cert --key /path/to/key
```

<br/>

---

#### Enable CORS

To enable CORS:

```shell
$> genserve enable servername cors
```

To customise CORS edit servers file

```shell
$> genserve edit servers
```

<br/>

---

### Global Options modifiable Via Configuration file

To obtain the path to the file that holds some default settings:

```shell
$> genserve path settings
```

To edit

```shell
$> genserve edit settings
```

📝 _.genserve.cjs_ ↴

```javascript
module.exports = {
    "protocol": "http://",
    "host"    : "desktop-amd",
    API       : {
        "GREETINGS"    : "Hello!",
        "SERVER_STATUS": {
            "SERVER_STARTED" : "SERVER_STARTED",
            "SERVER_RUNNING" : "SERVER_RUNNING",
            "GET_SERVER_INFO": "GET_SERVER_INFO"
        }
    }
};
```

staticDirs: Directories to serve (The first directory specified has precedence over the other and so on)

<br/>

---

## Examples

<br/>

### Start server serving multiple directories

```shell
$> genserve start --dir public --dir build
```

<br/>

### Stop default server

```shell
$> genserve stop
```

<br/>

### Start a named server on port 10040 serving the working directory

```shell
$> genserve start genesis --port 10040
```

<br/>

### Stop the named server

```shell
$> genserve stop genesis
```

<br/>

### Show list of registered servers

```shell
$> genserve scan
```

<br/>

💻↴

```
active │ serverName │ serverUrl                 │ port  │ apiPort │ staticDirs                         │ defaultPage  │
────── │ ────────── │ ───────────────────────── │ ───── │ ─────── │ ────────────────────────────────── │ ──────────── │
false  │ default    │ http://localhost:60287/   │ 60287 │ 60287   │ C:\projects\genserve\public        │ index.html   │
false  │ genesis    │ http://localhost:10040/   │ 10040 │ 10040   │ C:\projects\genserve               │ index.html   │
```

<br/>
<br/>

---

### Run server from a shell or in a CI system

```shell
# Start the server in the background
$> genserve start testserver --dir . --port 5000

# Run your tests
$> npm test

# Stop the server
$> genserve stop testserver
```

<br/>

---

### Run a server from within a script

```javascript
(async function ()
{

    const {startGenServer, stopGenServer, infoGenServer} = require("genserve");

    // Server name
    const SERVER_NAME = "my-test-server";

    // Start the server which name will be "my-test-server"
    const serverStarted = await startGenServer({name: SERVER_NAME, port: 9880});
    if (!serverStarted)
    {
        console.error("Failed to start server");
        process.exit(1);
    }

    // Display info server (Optional)
    const data = await infoGenServer({name: SERVER_NAME});
    console.log(data);

    // Stop the server "my-test-server"
    await stopGenServer({name: SERVER_NAME});

}())

```

<br/>

---

## How to

### Setup and configure a server

###### 1. Set server properties

```shell
$> genserve set rserver --port 8080 --dir ./public
```

###### 2. Edit server file properties for more advanced setup

```shell
$> genserve edit servers
```
###### Look for the rserver section in your editor and modify the options you want to change
```json
{
    "rserver": {
    "serverName": "rserver",
    "defaultPage": "index.html",
    "protocol": "http://",
    "host": "localhost",
    "port": 8080,
    "staticDirs": [
    "C:/projects/genserve/public"
    ],
    "serverUrl": "http://localhost:5050/index.html",
    "enableapi": true,
    "webSocketStarted": false,
    "active": false,
    "timeout": 5000,
    "perMessageDeflate": false,
    "strictApiPort": false,
    "strictServerPort": true,
    "apiPort": 8080,
    "dynamicDirs": [],
    "dynamicExts": ".server.[cm]?js"
    }
}
```

###### Save and close the file 

<br/>

###### 3. Start the server

```shell
$> genserve start rserver
```

###### 4. Check that the server is up and running

```shell
$> genserve scan
```

💻↴

```shell
active │ serverName │ serverUrl                                  │ port  │ apiPort │ staticDirs                      │ defaultPage  │ locked │ protected │                                                                          
────── │ ────────── │ ────────────────────────────────────────── │ ───── │ ─────── │ ─────────────────────────────── │ ──────────── │ ────── │ ───────── │                                                                          
true   │ patrice    │ http://localhost:10060/index.html          │ 10060 │ 10060   │ C:/projects/genserve/public     │ index.html   │ no     │ yes       │                                                  
false  │ cloned     │ http://localhost:10060/public/index.html   │ 10060 │ 10060   │ C:/projects/genserve/public     │ index.html   │ no     │ yes       │                                                 
false  │ server     │ http://localhost:58352/undefined           │ 58352 │ 58352   │ C:/projects/genserve            │ index.html   │ no     │ yes       │                                                 
false  │ tom        │ http://localhost:10095/undefined           │ 10095 │ 10095   │ C:/projects/genserve/public     │ index.html   │ no     │ no        │
true   │ noemi      │ http://localhost:10090/index.server.cjs    │ 10090 │ 10090   │ C:/projects/genserve/public     │ index.html   │ no     │ no        │
false  │ rserver    │ http://localhost:5050/index.html           │ 8080  │ 8080    │ C:/projects/genserve/public     │ index.html   │ no     │ no        │
```