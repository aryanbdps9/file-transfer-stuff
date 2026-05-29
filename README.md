# Transferring Files Over LAN with rsync (WSL2 -> WSL2)

## 1. Open the port on the receiver (Windows, PowerShell as Admin)

If you already have a firewall rule from a previous time, enable it:

```powershell
# Use admin powershell
Enable-NetFirewallRule -DisplayName "rsync-temp"
```

If you are on a clean machine, use this to create a rule:

```powershell
# Use admin powershell
New-NetFirewallRule -DisplayName "rsync-temp" -Direction Inbound -Protocol TCP -LocalPort <PORT> -Action Allow
```

## 2. Create the daemon config on the receiver

```bash
# WSL 2 shell
echo -e "[files]\npath=/path/to/receiver/root/\nread only=no\nuse chroot=no" > /tmp/rx.conf
```

`[files]` is the module name used in the rsync URL. The `path` is the only directory exposed — the sender can't access anything outside it.

**Example:**
```bash
echo -e "[files]\npath=/mnt/c/Users/Zura/Videos/\nread only=no\nuse chroot=no" > /tmp/rx.conf
```

## 3. Start the rsync daemon on the receiver

```bash
sudo rsync --daemon --no-detach --config=/tmp/rx.conf --port=<PORT> --log-file=/dev/stdout
```

## 4. Send files from the sender

```bash
rsync -av --progress --inplace --size-only /path/to/source/ rsync://RECEIVER_IP:<PORT>/files/
```

**Example:**
```bash
rsync -av --progress --inplace --size-only /mnt/c/Users/Zura/Videos/OBS/ rsync://192.168.1.45:<PORT>/files/OBS/
```

- `--inplace` avoids temp file creation, often required when syncing to `/mnt/c/` on WSL2
- `--size-only` skips files that already match by size (use instead of timestamp comparison on Windows filesystems)
- Trailing slashes on paths are significant — they mean "the contents of this directory"

## 5. Disable the firewall rule on receiver after transfer is complete

```powershell
Disable-NetFirewallRule -DisplayName "rsync-temp"
```

## Notes

- The daemon runs in the foreground and exits when you Ctrl+C it
- `failed to set times` warnings are harmless on DrvFs (`/mnt/c/`); add `--omit-dir-times --no-times` to suppress them
- To fix timestamp support permanently on the receiver, add `options = "metadata"` under `[automount]` in `/etc/wsl.conf` and run `wsl --shutdown` from Windows
- To see if the port is accessible, run this on sender and you should instantly see a success message: `nc -zv RECEIVER_IP 8730`
