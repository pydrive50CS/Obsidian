```bash
#!/usr/bin/env bash
set -Eeuo pipefail

#######################################
# CONFIG
#######################################
APPS_DIR="$HOME/Applications"
DESKTOP_DIR="$HOME/.local/share/applications"
ICON_DIR="$HOME/Applications/Downloads/icons"
LOG_FILE="$HOME/.local/share/appimage-installer.log"

#######################################
# COLORS
#######################################
RED='\033[0;31m'
YELLOW='\033[1;33m'
GREEN='\033[0;32m'
NC='\033[0m' # No Color

#######################################
# LOGGING
#######################################
log() {
    local level="$1"; shift
    local color="$1"
    local msg="$2"
    printf "[%s] [%s] %s\n" "$(date '+%F %T')" "$level" "$msg" | tee -a "$LOG_FILE"
}

info()  { printf "${GREEN}[%s] [INFO] %s${NC}\n" "$(date '+%F %T')" "$*" | tee -a "$LOG_FILE"; }
warn()  { printf "${YELLOW}[%s] [WARN] %s${NC}\n" "$(date '+%F %T')" "$*" | tee -a "$LOG_FILE"; }
error() { printf "${RED}[%s] [ERROR] %s${NC}\n" "$(date '+%F %T')" "$*" | tee -a "$LOG_FILE"; }

die() {
    error "$@"
    exit 1
}

#######################################
# TRAPS
#######################################
trap 'die "Script failed at line $LINENO."' ERR
trap 'info "Interrupted by user"; exit 130' INT

#######################################
# VALIDATE REQUIRED COMMANDS
#######################################
command -v update-desktop-database >/dev/null 2>&1 \
    || die "update-desktop-database not found (install desktop-file-utils)"

#######################################
# PREPARE DIRECTORIES
#######################################
mkdir -p "$APPS_DIR" "$DESKTOP_DIR" || die "Failed to create directories"

#######################################
# FIND APPIMAGES
#######################################
shopt -s nullglob
APP_IMAGES=( *.AppImage )
shopt -u nullglob

(( ${#APP_IMAGES[@]} > 0 )) || die "No AppImages found in current directory"

info "Found AppImages:"
for i in "${!APP_IMAGES[@]}"; do
    printf "  %d) %s\n" "$((i+1))" "${APP_IMAGES[$i]}"
done

#######################################
# USER INPUT
#######################################
read -rp "Enter AppImage numbers to install (space-separated): " selection
[[ -n "$selection" ]] || die "No selection provided"

#######################################
# PROCESS SELECTION
#######################################
for raw_index in $selection; do

    [[ "$raw_index" =~ ^[0-9]+$ ]] || {
        warn "Invalid selection: '$raw_index'"
        continue
    }

    index=$((raw_index - 1))

    (( index >= 0 && index < ${#APP_IMAGES[@]} )) || {
        warn "Selection out of range: $raw_index"
        continue
    }

    APP_IMAGE="${APP_IMAGES[$index]}"
    BASE_NAME="${APP_IMAGE%.AppImage}"

    ###################################
    # NAME & VERSION EXTRACTION
    ###################################
    APP_NAME="$(sed -E 's/[-_ ]v?[0-9]+(\.[0-9]+)*.*$//' <<< "$BASE_NAME" \
                | tr '[:upper:]' '[:lower:]')"
    DESKTOP_NAME="$(tr '[:lower:]' '[:upper:]' <<< ${APP_NAME:0:1})${APP_NAME:1}"
    VERSION="$(grep -oE '[0-9]+(\.[0-9]+)+' <<< "$BASE_NAME" | head -n1 || true)"

    [[ -n "$APP_NAME" ]] || {
        warn "Unable to determine app name: $APP_IMAGE"
        continue
    }

    TARGET_APP="$APPS_DIR/$APP_IMAGE"
    DESKTOP_FILE="$DESKTOP_DIR/${APP_NAME}.desktop"

    ###################################
    # MOVE APPIMAGE
    ###################################
    if [[ -e "$TARGET_APP" ]]; then
        warn "AppImage already exists: $TARGET_APP (skipped move)"
    else
        mv "$APP_IMAGE" "$TARGET_APP"
        chmod +x "$TARGET_APP"
        info "Installed AppImage → $TARGET_APP"
    fi

    ###################################
    # ICON HANDLING
    ###################################
    ICON_PATH="$ICON_DIR/${APP_NAME}.png"
    APP_ICON_PATH="$HOME/.local/share/icons/hicolor/128x128/apps/${APP_NAME}.png"
    cp $ICON_PATH $APP_ICON_PATH

    if [[ -f "$ICON_PATH" ]]; then
        info "Using icon: $ICON_PATH"
    else
        warn "Icon not found for $APP_NAME"
        ICON_PATH=""
    fi

    ###################################
    # DESKTOP ENTRY
    ###################################
    cat > "$DESKTOP_FILE" <<EOF
[Desktop Entry]
Type=Application
Name=$DESKTOP_NAME
Comment=$APP_NAME AppImage${VERSION:+ (Version $VERSION)}
Exec=$TARGET_APP
Icon=$APP_ICON_PATH
Terminal=false
Categories=Utility;
Version=${VERSION:-unknown}
EOF

    chmod +x "$DESKTOP_FILE"
    info "Desktop entry created: $DESKTOP_FILE"

done

#######################################
# REFRESH DESKTOP DATABASE
#######################################
update-desktop-database "$DESKTOP_DIR" \
    && info "Desktop database updated"

info "Installation completed successfully"

```