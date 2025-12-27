#bash #nirealtimelinux

## Script

Save the following script to `/home/lvuser/monitor_crio.sh`. It uses [[top]] for CPU monitoring and [[free]] for memory usage along with [[grep]] and [[awk]] for parsing the data.

```bash
#!/bin/bash

# Path to USB mount
START_TIME=$(date +"%Y-%m-%d_%H-%M-%S")
USB_PATH="/media/sda1" # or /u
LOG_FILE="$USB_PATH/crio_stats_$START_TIME.csv"
exec >> "$LOG_FILE" 2>&1 # send stderr and stdout to the log file

echo "[$(date)] Starting monitor script"

# Ensure USB is mounted
if [ ! -d "$USB_PATH" ]; then
  echo "USB drive not found at $USB_PATH"
  exit 1
fi

# Log header
echo "Timestamp, CPU Load (%), Mem Used (MB), Mem Free (MB), Uptime (s)" >> "$LOG_FILE"

while true; do
  TIMESTAMP=$(date +"%Y-%m-%d %H:%M:%S")

  # CPU usage (1-second average)
  # run top once, get the CPU line, and return 100 minus the idle cpu percentage
  CPU_LOAD=$(top -bn1 | grep "CPU:" --max-count=1 | awk '{print 100 - $4}') 

  # Memory usage
  # get the free and used memory statss from the Mem line
  MEM_STATS=$(free -m | awk '/Mem:/ {print $3","$4}')

  # Uptime in seconds
  # get the first column from /proc/uptime
  UPTIME=$(cat /proc/uptime | awk '{print int($1)}')

  # Log line
  echo "$TIMESTAMP, $CPU_LOAD, $MEM_STATS, $UPTIME" >> "$LOG_FILE"

  # Wait before next log (e.g., 60 seconds)
  sleep 60
done

```

Make the script executable

```bash
chmod +x /home/lvuser/monitor_crio.sh
```

and run it in the background

```bash
./monitor_crio.sh &
```
