## runc 
- runc is the low-level OCI (Open Container Initiative) runtime that actually creates and runs Linux containers.
- "Create a new process with these namespaces, these cgroups, this filesystem, these devices, and start this command."

### Step 1. Install runc
```BASH
sudo apt install runc
```

Verify
```BASH
runc --version
```

### Step 2. Create a bundle
```BASH
mkdir mycontainer
cd mycontainer
mkdir rootfs
```
Your directory is now
```BASH
mycontainer/
    rootfs/
```
### Step 3. Create a root filesystem

Unlike Docker, runc does not pull images. <br>

You need a Linux root filesystem.
<br>
One easy method is using Docker:
```BASH
docker export $(docker create ubuntu:22.04) | tar -C rootfs -xvf -
```
Now
```BASH
mycontainer/

config.json
rootfs/
    bin/
    etc/
    lib/
    usr/
    ...
```
No Docker image is needed anymore.

### Step 4. Generate OCI config

Run
```
runc spec
```
This creates
```
config.json
```
Example
```
bundle/

config.json

rootfs/
```

### Step 5. Edit config

Find
```
"args": [
    "sh"
]
```
or
```
"args": [
    "bash"
]
```
If your rootfs contains bash.

### Step 6. Run container
```
sudo runc run testcontainer
```
You'll immediately enter
```
root@
```
You're now inside a container created directly by runc.
<br>
Check
```
hostname
```
or
```
ps aux
```
You'll notice
```
PID 1
```

#### In another terminal

List running containers
```
sudo runc list
```
Output
```
ID              PID     STATUS
testcontainer   13245   running
```
#### Kill it 
Inside
```BASH
exit
```
or
```BASH
sudo runc kill testcontainer KILL
```
Delete
```BASH
sudo runc delete testcontainer
```

#### Running detached

Instead of
```BASH
runc run
```
use
```BASH
sudo runc run -d testcontainer
```
List
```BASH
sudo runc list
```
Attach
```BASH
sudo runc exec testcontainer bash
```

#### Example with BusyBox

BusyBox is much smaller.

Download
```BASH
mkdir bundle
cd bundle
mkdir rootfs

docker export $(docker create busybox) | tar -C rootfs -xvf -
```
Generate
```BASH
runc spec
```
Modify
```JSON
"args": [
    "sh"
]
```
Run
```BASH
sudo runc run busybox
```
