# Raspberry Pi  
[https://www.raspberrypi.com/](https://www.raspberrypi.com/)

## Copying files

There are many ways to copy files.

This method will copy files between two locations, including over a network. And resume if there was a loss of connection if you start again. 

Also running the same rsync command again, will only copy changes/differences. So if the source directory has more files, or deleted files, it'll update the destination directory. And if nothing is different between the two, running it again even on even a very large source directory like 500Gb, will complete almost instantly.

`rsync -avz source-directory destination-user@destination-device:"destination-directory"`

**Key**
- a - Archive mode = recursive, preserves permissions, times, symlinks, etc.
- v - Verbose output (see what's happening as it is copying)
- z - Compress data during transfer (great over network)

## SSH  
**Enable secure, passwordless SSH using keypair authentication**

---

### 1. Generate SSH Keypair

First access the device you want to SSH from. So if you want to SSH from your laptop to your raspberry pi, open a terminal on your laptop.

```bash
ssh-keygen -t ed25519 -C "macbook-air-2013"
# Press Enter to accept default location (~/.ssh/id_ed25519)

Also no passphrase needed for this
```

> This creates:
> - `~/.ssh/id_ed25519` → **private key** (keep safe!)
> - `~/.ssh/id_ed25519.pub` → **public key** (copy to Pi)

---

### 2. Copy Public Key to destination device

```bash
# Replace user@raspberrypi with your username on your Pi, and the Pi's IP e.g. tommy@192.168.2.1
ssh-copy-id user@raspberrypi
```

> First-time login may ask for password — **this is the last time**.

---

### 3. Disable Password Login & Lock Down SSH

On the **Pi**, run:

```bash
sudo nano /etc/ssh/sshd_config
```

**Set or add these lines**:

```conf
PasswordAuthentication no
```

Save (`Ctrl+X`, `y`, `Enter`) and restart:

```bash
sudo systemctl restart ssh
```

---

### 4. Verify No Root SSH Keys Exist

```bash
sudo cat /root/.ssh/authorized_keys 2>/dev/null || echo "No root keys"
```

**Expected output**:  
```
No root keys
```

If you see a key (`ssh-rsa AAAAB3...`), **delete it**:

```bash
sudo rm -rf /root/.ssh
```

---

### 5. Test It Works

From your laptop:

```bash
ssh pi@raspberrypi
# → Should log in instantly (no password)
```

Run `sudo whoami` → should return `root`.

---

### SSH using Tailscale DNS

Login to your Tailscale dashboard, and go to the DNS tab.

The url is https://login.tailscale.com/admin/dns

There at the top you should see *Tailnet DNS name*. Copy that unique name.

It should look something like `happy-kitty.ts.net`

Now before where you were typing something like

`ssh timmy@192.168.8.111`

to SSH into another machine, you can now type

`ssh timmy@burmese.happy-kitty.ts.net`

where
- `timmy` is the username on the machine you want to SSH into
- `burmese` is the name of the machine according your Tailscale [machine names](https://login.tailscale.com/admin/machines)

You can do this from any location. No need to use local IPs anymore!

---

### Optional: Use **Tailscale SSH** (still testing...)

```bash
# On Pi
sudo tailscale set --ssh=true

# Disable traditional SSH (zero open ports)
sudo systemctl disable --now ssh
```

Now connect from anywhere:

```bash
tailscale ssh pi@raspberrypi
```

---

