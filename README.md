# Daz Studio CLI Character Generator (Genesis 9)

An automated character generation pipeline for Daz Studio running on Linux via Wine. This project uses Daz Script (QtScript) to bypass encryption hurdles and automate the dialing of unique character combinations.

## 🛠 Troubleshooting History & Lessons Learned

### 1. The Encryption Wall (`.dsf.enc`)
- **Issue:** Daz Connect assets are encrypted on disk. Standard CLI tools (Python/Bash) cannot read morph data directly.
- **Solution:** Switched to internal Daz Scripting (`.dsa`), allowing the Daz engine to decrypt and read assets at runtime.

### 2. Path Mapping (Wine/Linux)
- **Issue:** Confusion between Linux paths (`/home/...`) and Wine paths (`C:/Users/...`).
- **Solution:** Utilized Daz's internal Virtual File System (e.g., `/People/Genesis 9/...`) which abstracts the physical location and searches across all mapped Content Directories.

### 3. API Discrepancies
- **Issue:** `oContentMgr.saveScene` returned `undefined`.
- **Solution:** Corrected the API call to `App.getAssetIOMgr().doSave(filename)`, which is the proper method for saving presets in Daz Studio 4.x.

### 4. Modeal Popups & CLI Hanging
- **Issue:** Wine execution hung indefinitely.
- **Cause:** Daz Studio "Modal" dialogs (Update Available, Login) block script execution.
- **Solution:** Manual intervention required to check "Do not show again" on all GUI prompts before automation can run headless.

## ⚠️ Known Issues (As of May 21, 2026)

### Resource Error (Skin Application)
- **Current Symptom:** The first character in a loop generates and saves successfully. However, subsequent characters trigger a "Resource Error" stating the skin `.duf` cannot be found, even though the path is correct.
- **Hypothesis:** `Scene.clear()` or the save process may be resetting the current working directory or clearing the library pointers in memory. 

## 🚀 How to Run
1. Open Daz Studio.
2. Go to `File -> Open Script`.
3. Select `ultimate_gen.dsa`.
4. Click **Execute**.

## 📁 Output
Characters are saved to: `C:/users/wgoton/Documents/DAZ 3D/Studio/My Library/Scenes/`
