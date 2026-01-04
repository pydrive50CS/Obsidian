
---
## The 6 “Always”
1. **Always know what you’re changing**  
   Read the config. Understand defaults. Know the “blast radius”.
2. **Always be able to roll back**  
   Backups and version control (git) are non-negotiable.
3. **Always log your work**  
   Keep a sysadmin journal: a simple `docs/CHANGELOG.md`.
4. **Always test in a safe place first**  
   Use containers/VMs before touching your daily driver.
5. **Always make work reproducible**  
   Scripts + dotfiles + notes = your future safety.
6. **Always assume your future self is a stranger**  
   Write notes that make sense months later.

## The 6 “Never”
1. **Never run mystery commands as root**  
   Especially `curl ... | sh`, random one-liners, unknown AUR scripts.
2. **Never edit “important systems” without a backup and exit plan**
3. **Never disable security to “make it work”**  
   Fix the cause, don’t remove guardrails.
4. **Never change multiple things at once**  
   One change → test → commit.
5. **Never ignore logs**  
   “It doesn’t work” isn’t a symptom. Logs are the symptom.
6. **Never treat automation as optional**  
   Manual steps become future outages.

---
