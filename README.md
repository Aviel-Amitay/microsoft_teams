<<<<<<< HEAD
# microsoft_teams
PowerShell scripts to generate and distribute Microsoft Teams background and thumbnail images to all local user profiles.
=======
# 🖼️ Teams Background Generator & Distributor

This PowerShell toolkit helps automate the creation and deployment of Microsoft Teams background images for all users on a machine.

It includes:

- ✅ A generator for Teams-compatible `.png` + `_thumb.png` image pairs
- ✅ A distributor to copy the images into each user's Teams profile

---

## 📁 Scripts Included

### `1_generate_teams_backgrounds.ps1`

Generates properly named Teams background files:

- Scans all `.jpg`, `.jpeg`, or `.png` images in a source folder (e.g. `C:\1`)
- Converts each to `.png` format
- For each image, generates:
  - `GUID.png`
  - `GUID_thumb.png`
- Output is saved to: `C:\TeamsBackgroundsPrepared`

📝 *Example Output:*  
	- 8dc93b47-484f-461c-8710-39c79f8e9fa0.png  
	- 8dc93b47-484f-461c-8710-39c79f8e9fa0_thumb.png  
	- 8dc93b48-484f-461c-8710-39c79f8e9fa0.png  
	- 8dc93b48-484f-461c-8710-39c79f8e9fa0_thumb.png  
  

---

### `2_distribute_backgrounds_to_users.ps1`

Copies the generated files to each local user's Teams background folder:

- Enumerates all user profiles under `C:\Users`
- Copies images to:

%LOCALAPPDATA%\Packages\MSTeams_8wekyb3d8bbwe\LocalCache\Microsoft\MSTeams\Backgrounds\Uploads

- Automatically creates the folder if it doesn't exist

---

## 🔧 Setup & Usage

### Step 1: Generate the Backgrounds
```powershell
.\1_generate_teams_backgrounds.ps1


Ensure C:\1 contains your source images.

Step 2: Distribute to Users
Run as Administrator:

.\2_distribute_backgrounds_to_users.ps1

✅ Requirements
PowerShell (64-bit)

Admin privileges (for cross-user access)

Microsoft Teams installed from the Microsoft Store (Package name: MSTeams_8wekyb3d8bbwe)

📌 Use Case
Perfect for:

Corporate laptops/desktops

Training labs and classrooms

Shared workstations

📄 License
MIT License. Use and modify freely with attribution.

🤝 Contributing
Pull requests welcome. Please fork the repo and submit a PR with changes.


---

Let me know if you want to include screenshots, logo branding, or configuration options in the README as well.
