```bash
#!/usr/bin/env bash

  

# Set your source and destination folders

SOURCE_DIR="/mnt/immich/upload/d4701910-c576-4834-a88d-4c201795589d"

DEST_DIR="/mnt/uploads"

  

# Create destination if it doesn't exist

mkdir -p "$DEST_DIR"

  

# File extensions to match (photos + videos)

EXTS=("jpg" "jpeg" "png" "gif" "bmp" "tiff" "heic"

      "mp4" "mov" "avi" "mkv" "wmv" "flv" "webm")

  

# Build find command

FIND_CMD=(find "$SOURCE_DIR" -type f \( )

for ext in "${EXTS[@]}"; do

    FIND_CMD+=(-iname "*.$ext" -o)

done

unset 'FIND_CMD[${#FIND_CMD[@]}-1]' # remove last -o

FIND_CMD+=(\))

  

# Run find and move files

"${FIND_CMD[@]}" | while IFS= read -r file; do

    base=$(basename "$file")

    dest="$DEST_DIR/$base"

  

    # Avoid overwriting by renaming

    counter=1

    while [[ -e "$dest" ]]; do

        name="${base%.*}"

        ext="${base##*.}"

        dest="$DEST_DIR/${name}_$counter.$ext"

        ((counter++))

    done

  

    echo "Moving: $file -> $dest"

    mv "$file" "$dest"

done
```