
#### 4. The Daily Workflow (The "Sync" Loop)

This is where the manual part comes in, as iOS doesn't allow background Git pushes easily without automation shortcuts.

- **To Pull (Get changes from Mint):**
    
    1. Open **Working Copy**.
    2. Tap your repo > **Pull**.
    3. Wait for it to finish.
    4. Switch to **Obsidian**; the changes should appear instantly.
	
- **To Push (Send changes to Mint):**
    
    1. Make edits in **Obsidian**.
    2. Open **Working Copy**.
    3. Tap your repo > **Commit** (enter a message) > **Push**.
    4. Now your Linux Mint machine can pull the changes.
### Automation Tip (Optional but Recommended)

Since you are on iOS, you can use the **Shortcuts**app to automate the "Pull" or "Push" process so you don't have to open Working Copy every time.

- Create a Shortcut that runs "Run Script in Working Copy" (if you have the integration enabled) or simply opens the app to the specific repo.
- Some users set up a "Today Widget" in Working Copy to quickly pull/push without opening the full app.

### Important Caveats for Linux Mint Users

- **Line Endings:** Ensure your Git config on Mint handles line endings correctly (`core.autocrlf`), though Obsidian usually handles this well. Since you are on Linux (LF) and iOS (LF), this is rarely an issue compared to Windows/Mac mixes.
- **Conflicts:** If you edit the same file on Mint and iOS simultaneously without syncing in between, you will get a merge conflict. You'll have to resolve it manually on Mint (since resolving conflicts on iOS via a text editor is painful).
- **SSH Keys:** If you use SSH for your remote, you must import your private key into **Working Copy**(Settings > SSH Keys) so it can authenticate.