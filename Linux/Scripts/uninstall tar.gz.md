``` bash
#!/bin/bash
# ====================================================================
# 🌍 Universal TAR.GZ App Uninstaller
# Usage: sudo ./uninstall-tar-app.sh <AppName>
# Example: sudo ./uninstall-tar-app.sh linwood
# ====================================================================

set -e

APP_NAME="$1"

if [ -z "$APP_NAME" ]; then
  echo "❌ Usage: $0 <AppName>"
  exit 1
fi

INSTALL_DIR="/opt/$APP_NAME"
BIN_PATH="/usr/bin/$APP_NAME"
DESKTOP_FILE="$HOME/.local/share/applications/$APP_NAME.desktop"
TMP_EXTRACT_DIR="$HOME/.tmp_extract_$APP_NAME"

# --------------------------------------------------------------------
# Step 1: Remove installation folder
# --------------------------------------------------------------------
if [ -d "$INSTALL_DIR" ]; then
  echo "🧹 Removing installation folder $INSTALL_DIR ..."
  sudo rm -rf "$INSTALL_DIR"
else
  echo "ℹ️ Installation folder $INSTALL_DIR not found, skipping."
fi

# --------------------------------------------------------------------
# Step 2: Remove symlink in /usr/bin
# --------------------------------------------------------------------
if [ -L "$BIN_PATH" ] || [ -f "$BIN_PATH" ]; then
  echo "🧹 Removing symlink $BIN_PATH ..."
  sudo rm -f "$BIN_PATH"
else
  echo "ℹ️ Symlink $BIN_PATH not found, skipping."
fi

# --------------------------------------------------------------------
# Step 3: Remove desktop launcher
# --------------------------------------------------------------------
if [ -f "$DESKTOP_FILE" ]; then
  echo "🧹 Removing desktop launcher $DESKTOP_FILE ..."
  rm -f "$DESKTOP_FILE"
else
  echo "ℹ️ Desktop launcher $DESKTOP_FILE not found, skipping."
fi

# --------------------------------------------------------------------
# Step 4: Remove temporary extraction folder
# --------------------------------------------------------------------
if [ -d "$TMP_EXTRACT_DIR" ]; then
  echo "🧹 Removing temporary folder $TMP_EXTRACT_DIR ..."
  rm -rf "$TMP_EXTRACT_DIR"
else
  echo "ℹ️ Temporary folder $TMP_EXTRACT_DIR not found, skipping."
fi

echo "✅ $APP_NAME has been completely uninstalled!"

```