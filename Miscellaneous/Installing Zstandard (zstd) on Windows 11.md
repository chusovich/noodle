## Step 1: Download Prebuilt Binary
1. Go to the [Zstandard GitHub Releases](https://github.com/facebook/zstd/releases) page.
2. Find the latest release (e.g., `v1.5.5` or newer).
3. Download the Windows binary ZIP file, such as:
   - `zstd-v1.5.5-win64.zip` (for 64-bit systems)

## Step 2: Extract Files
1. Extract the contents of the ZIP file to a folder on your system.
   - Example: `C:\Tools\zstd`
2. Inside the folder, you'll see `zstd.exe` and other related files.

## Step 3: (Optional) Add Zstandard to System PATH
1. Press `Win + S` and search for:
   - **"Edit environment variables for your account"**
2. Under **"User variables"**, select `Path` and click **Edit**.
3. Click **New**, and add the path to your Zstandard folder: `C:\Tools\zstd`
4. Click **OK** to close all dialogs.

## Step 4: Verify Installation

Open **Command Prompt** or **PowerShell**, and run:

```bash
zstd --version
```

Expected output:

```
*** zstd command line interface 64-bits v1.5.5, by Yann Collet ***
```

## Step 5: Basic Usage Example

Compress a file:

```bash
zstd file.txt
```

This creates `file.txt.zst`.

Decompress:

```bash
zstd -d file.txt.zst
```
