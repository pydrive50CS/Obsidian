#!/bin/bash

# ==========================================
# Obsidian Vault Git Auto-Sync Script
# ==========================================
# Author: en1gma
# Description: Automatically sync Obsidian Vault to Git repository.
# Logs actions with timestamps and colors. Saves logs to .gitlogs file.
# Keeps only last 3 months of logs.
# ==========================================

# ----------------------------
# CONFIGURATION
# ----------------------------
VAULT_DIR="$HOME/Applications/AppData/Obsidian"
SYNC_INTERVAL=600   # in seconds
LOG_FILE="$VAULT_DIR/.gitlogs"

# Automatically find Obsidian AppImage
OBSIDIAN_APPIMAGE=$(ls "$HOME/Applications"/Obsidian-*.AppImage 2>/dev/null | head -n1)

# ----------------------------
# COLORS FOR LOGS
# ----------------------------
RED="\033[0;31m"
GREEN="\033[0;32m"
YELLOW="\033[1;33m"
BLUE="\033[0;34m"
RESET="\033[0m"

log() {
    local type="$1"
    local message="$2"
    local color

    case "$type" in
        INFO) color="$BLUE";;
        SUCCESS) color="$GREEN";;
        WARNING) color="$YELLOW";;
        ERROR) color="$RED";;
        *) color="$RESET";;
    esac

    # Only log git-related actions
    if [[ "$type" == "INFO" && "$message" == "No changes detected to sync" ]]; then
        echo -e "${color}[$(date '+%Y-%m-%d %H:%M:%S')] [$type] $message${RESET}"
    else
        echo -e "${color}[$(date '+%Y-%m-%d %H:%M:%S')] [$type] $message${RESET}"
        echo "[$(date '+%Y-%m-%d %H:%M:%S')] [$type] $message" >> "$LOG_FILE"
        prune_logs
    fi
}

# ----------------------------
# PRUNE LOGS (keep last 3 months)
# ----------------------------
prune_logs() {
    if [[ -f "$LOG_FILE" ]]; then
        # Keep only entries from last 3 months
        local cutoff_date
        cutoff_date=$(date --date="3 months ago" '+%Y-%m-%d')
        awk -v cutoff="$cutoff_date" '$0 ~ /^\[([0-9]{4}-[0-9]{2}-[0-9]{2})/ {
            split($1,date,"[\\[\\]]");
            if(date[2] >= cutoff) print $0
        }' "$LOG_FILE" > "${LOG_FILE}.tmp" && mv "${LOG_FILE}.tmp" "$LOG_FILE"
    fi
}

# ----------------------------
# PRE-CHECKS
# ----------------------------
if [[ -z "$OBSIDIAN_APPIMAGE" ]]; then
    log ERROR "No Obsidian AppImage found in ~/Applications"
    exit 1
fi

chmod +x "$OBSIDIAN_APPIMAGE" || { log ERROR "Failed to make AppImage executable: $OBSIDIAN_APPIMAGE"; exit 1; }

if [[ ! -d "$VAULT_DIR" ]]; then
    log ERROR "Vault directory does not exist: $VAULT_DIR"
    exit 1
fi

cd "$VAULT_DIR" || { log ERROR "Cannot cd into vault directory"; exit 1; }

# Initialize git logs file if not present
touch "$LOG_FILE"

# ----------------------------
# GIT SYNC FUNCTION
# ----------------------------
sync_repo() {
    if ! git rev-parse --is-inside-work-tree >/dev/null 2>&1; then
        log ERROR "Directory is not a git repository: $VAULT_DIR"
        return 1
    fi

        if [[ -n $(git status --porcelain | grep -v ".gitlogs") ]]; then
        changed_files=$(git status --porcelain | awk '{print $2}')
        git add .

        local commit_message="Auto-sync: $(date '+%Y-%m-%d %H:%M:%S') | Files changed:\n$changed_files"

        if git commit -m "$(echo -e "$commit_message")" >/dev/null 2>&1; then
            log SUCCESS "Committed changes: $commit_message"
            if git push >/dev/null 2>&1; then
                log SUCCESS "Pushed changes to remote repository"
            else
                log WARNING "Failed to push changes to remote. Check network or remote repo."
            fi
        else
            log WARNING "Nothing to commit or commit failed"
        fi
    else
        log INFO "No changes detected to sync"
    fi
}

# ----------------------------
# BACKGROUND SYNC LOOP
# ----------------------------
background_sync() {
    while true; do
        sync_repo
        sleep "$SYNC_INTERVAL"
    done
}

# Run sync in background
background_sync &
SYNC_PID=$!
log INFO "Background sync started with PID $SYNC_PID"

# ----------------------------
# LAUNCH OBSIDIAN
# ----------------------------
log INFO "Launching Obsidian..."
$OBSIDIAN_APPIMAGE

# ----------------------------
# CLEANUP AFTER EXIT
# ----------------------------
log INFO "Obsidian exited. Cleaning up background sync..."
kill "$SYNC_PID" 2>/dev/null
wait "$SYNC_PID" 2>/dev/null

# Final sync only if changes exist
if [[ -n $(git status --porcelain) ]]; then
    sync_repo
else
    log INFO "No final changes to commit"
fi

log INFO "Script exiting."
