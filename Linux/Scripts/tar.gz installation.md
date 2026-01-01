``` bash
#!/bin/bash
# ====================================================================
# 🌍 Universal TAR.GZ App Installer (with automatic icon detection)
# Usage: sudo ./install-tar-app.sh <AppName>
# Example: sudo ./install-tar-app.sh anydesk
# ====================================================================

set -e

APP_NAME="$1"

if [ -z "$APP_NAME" ]; then
  echo "❌ Usage: $0 <AppName>"
  exit 1
fi

# --------------------------------------------------------------------
# Define paths
# --------------------------------------------------------------------
SCRIPT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"
ICON_DIR="$SCRIPT_DIR/icons"
DOWNLOAD_DIR="$HOME/Applications/Downloads"
INSTALL_DIR="/opt/$APP_NAME"
BIN_PATH="/usr/bin/$APP_NAME"
DESKTOP_FILE="$HOME/.local/share/applications/$APP_NAME.desktop"

# --------------------------------------------------------------------
# Step 1: Find TAR.GZ file
# --------------------------------------------------------------------
echo "🔍 Searching for $APP_NAME*.tar.gz in $DOWNLOAD_DIR ..."
TAR_FILE=$(find "$DOWNLOAD_DIR" -type f -iname "${APP_NAME}*.tar.gz" | sort | tail -n 1)

if [ -z "$TAR_FILE" ]; then
  echo "❌ No ${APP_NAME}*.tar.gz found in $DOWNLOAD_DIR"
  exit 1
fi

echo "📦 Found archive: $TAR_FILE"
echo "🧹 Removing old installation..."
sudo rm -rf "$INSTALL_DIR"

# --------------------------------------------------------------------
# Step 2: Extract and detect folder
# --------------------------------------------------------------------
echo "📂 Extracting to /tmp ..."
tar -xzf "$TAR_FILE" -C /tmp

EXTRACTED_DIR=$(tar -tzf "$TAR_FILE" | head -1 | cut -d/ -f1)
FULL_PATH="/tmp/$EXTRACTED_DIR"

if [ ! -d "$FULL_PATH" ]; then
  echo "❌ Extraction failed or unexpected structure. Check archive contents."
  exit 1
fi

echo "🚚 Moving extracted folder to $INSTALL_DIR ..."
sudo mv "$FULL_PATH" "$INSTALL_DIR"

# --------------------------------------------------------------------
# Step 3: Find binary and create symlink
# --------------------------------------------------------------------
APP_BINARY=$(find "$INSTALL_DIR" -type f -perm /111 | head -n 1)

if [ -z "$APP_BINARY" ]; then
  echo "⚠️  No executable found. Please verify manually inside $INSTALL_DIR."
else
  echo "🔗 Linking binary to $BIN_PATH ..."
  sudo ln -sf "$APP_BINARY" "$BIN_PATH"
fi

sudo chmod +x "$INSTALL_DIR"/* || true

# --------------------------------------------------------------------
# Step 4: Auto-detect icon
# --------------------------------------------------------------------
echo "🖼️ Searching for icon for $APP_NAME in $ICON_DIR ..."
ICON_FILE=""
for ext in png jpg jpeg svg ico; do
  if [ -f "$ICON_DIR/${APP_NAME,,}.$ext" ]; then
    ICON_FILE="$ICON_DIR/${APP_NAME,,}.$ext"
    break
  fi
done

if [ -n "$ICON_FILE" ]; then
  echo "✅ Found icon: $ICON_FILE"
  sudo cp "$ICON_FILE" "$INSTALL_DIR/${APP_NAME,,}.png"
else
  echo "⚠️ No custom icon found in $ICON_DIR. Continuing without it."
fi

# --------------------------------------------------------------------
# Step 5: Create desktop launcher
# --------------------------------------------------------------------
mkdir -p "$(dirname "$DESKTOP_FILE")"

cat > "$DESKTOP_FILE" <<EOF
[Desktop Entry]
Name=${APP_NAME^}
Comment=${APP_NAME^} Application
Exec=$BIN_PATH
Icon=$INSTALL_DIR/${APP_NAME,,}.png
Terminal=false
Type=Application
Categories=Utility;Network;
EOF

echo "✅ $APP_NAME installed successfully!"
echo "➡️  Run it by typing '$APP_NAME' or from your application menu."

```