Below is a **clear, step-by-step guide** to setting up **rclone with rsync.net** using **SFTP/SSH**, which is the most common and reliable setup.

This works well for photo backups and general data.

---

## 1️⃣ Prerequisites

Before starting, make sure you have:

- An **rsync.net account**
- Your **rsync.net username**
- Your **rsync.net storage hostname** (looks like `yourname@yourname.rsync.net`)
- **SSH access enabled** (default on rsync.net)
- rclone installed
- `rclone version`

If rclone isn’t installed, see: [https://rclone.org/install/](https://rclone.org/install/)

---

## 2️⃣ Set Up SSH Access (Important)

rclone uses **SFTP over SSH**, so you should use **SSH keys** (recommended).

### Generate an SSH Key (if You don’t Already Have one)

`ssh-keygen -t ed25519 -C "rclone-rsync"`

Just press **Enter** for defaults.

### Upload Your Public Key to rsync.net

`cat ~/.ssh/id_ed25519.pub`

Copy the output and add it in the **rsync.net customer portal** under **SSH Keys**.

Test login:

`ssh yourusername@yourusername.rsync.net`

If you get a shell prompt, you’re good 👍

---

## 3️⃣ Create the Rclone Remote

Run:

`rclone config`

### Step-by-step Answers

`n) New remote name> rsyncnet`

Choose **SFTP**:

`Storage> sftp`

Now answer the prompts:

`host> yourusername.rsync.net user> yourusername port> 22`

Authentication:

`password> n key_file> ~/.ssh/id_ed25519 key_file_pass> (leave blank if no passphrase)`

Advanced config:

`y/n> n`

Confirm:

`y) Yes this is OK`

✅ Your rclone remote is now created as `rsyncnet:`.

---

## 4️⃣ Test the Connection

List your remote root:

`rclone lsd rsyncnet:`

Create a backup directory:

`rclone mkdir rsyncnet:backups`

---

## 5️⃣ First Backup (Photos Example)

Example: backing up your local `~/Pictures` folder.

`rclone sync ~/Pictures rsyncnet:backups/Pictures \   --progress \   --verbose`

### What This Does

- `sync` → makes the destination match the source
    
- Deletes files on rsync.net **only if they were deleted locally**
    
- Good for backups if you understand this behavior

⚠️ If you want **no deletions**, use `copy` instead:

`rclone copy ~/Pictures rsyncnet:backups/Pictures --progress`

---

## 6️⃣ Enable Snapshots (Huge rsync.net Benefit)

rsync.net supports **automatic snapshots** if enabled on your account.

Snapshots protect you from:

- Accidental deletes
    
- Ransomware
    
- Bad syncs

👉 Enable snapshots in the rsync.net portal (hourly/daily/weekly).

Once enabled, **you don’t need to do anything special in rclone** — it just works.

---

## 7️⃣ Optional: Encrypt Your Backups (Highly Recommended)

Create an encrypted layer on top of rsync.net.

`rclone config`

`n) New remote name> rsyncnet-crypt Storage> crypt`

`remote> rsyncnet:backups filename_encryption> standard directory_name_encryption> true password> (choose a strong password) password2> (confirm)`

Now back up **securely**:

`rclone sync ~/Pictures rsyncnet-crypt:Pictures --progress`

rsync.net will only see encrypted data 🔒

---

## 8️⃣ Automate With a Simple Script

Example backup script:

`#!/bin/bash rclone sync /home/user/Pictures rsyncnet-crypt:Pictures \   --log-file=$HOME/rclone.log \   --log-level INFO`

Make executable:

`chmod +x backup.sh`

Run via cron:

`crontab -e`

Daily at 2am:

`0 2 * * * /home/user/backup.sh`

---

## 9️⃣ Useful Rclone Commands

Check storage usage:

`rclone about rsyncnet:`

List files:

`rclone ls rsyncnet:backups`

Dry-run before syncing:

`rclone sync ~/Pictures rsyncnet:backups/Pictures --dry-run`

---

## ✅ Summary

**Best setup for rsync.net + rclone**

- Use **SFTP**
    
- Use **SSH keys**
    
- Enable **snapshots**
    
- Add **rclone crypt** for encryption
    
- Use `sync` for true backups or `copy` for append-only

If you want, I can:

- Tune rclone for **very large photo libraries**

- Help design a **3-2-1 backup strategy**
    
- Compare rsync.net vs Backblaze B2 vs Wasabi for _your_ data size

Just tell me 👍
