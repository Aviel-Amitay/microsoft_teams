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

>>>>>>> 11da32a (Update README)
