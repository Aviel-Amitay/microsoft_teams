################################################################################
# Distribution Teams Photo
# Description: Distribute profile photos for Microsoft Teams users
# Author : Aviel Amitay
# GitHub : https://github.com/Aviel-Amitay
# Modified: May 19 2025
################################################################################


$sourceFolder = "C:\TeamsBackgroundsPrepared"

if (-not (Test-Path $sourceFolder)) {
    Write-Error "[ERROR] Source folder '$sourceFolder' does not exist."
    exit 1
}

# Collect PNG files to deploy
$backgroundImages = Get-ChildItem -Path $sourceFolder -Filter *.png -File
if ($backgroundImages.Count -eq 0) {
    Write-Warning "[WARNING] No background images found in $sourceFolder"
    exit 0
}

Write-Host "[INFO] Found $($backgroundImages.Count) image(s) to distribute." -ForegroundColor Yellow

# Get all user profiles from C:\Users, excluding system ones
$userProfiles = Get-ChildItem 'C:\Users' -Directory | Where-Object {
    $_.Name -notin @('Default', 'Default User', 'Public', 'All Users')
}

foreach ($profile in $userProfiles) {
    $userName = $profile.Name
    $localAppData = Join-Path $profile.FullName "AppData\Local"
    $teamsUploadsPath = Join-Path $localAppData "Packages\MSTeams_8wekyb3d8bbwe\LocalCache\Microsoft\MSTeams\Backgrounds\Uploads"

    Write-Host "`n[INFO] Processing user: $userName" -ForegroundColor Cyan

    if (-not (Test-Path $teamsUploadsPath)) {
        Write-Warning "[WARNING] Uploads path not found for user '$userName'. Creating it..."
        try {
            New-Item -Path $teamsUploadsPath -ItemType Directory -Force | Out-Null
        } catch {
            Write-Error "[ERROR] Failed to create folder for user '$userName': $_"
            continue
        }
    }

    foreach ($file in $backgroundImages) {
        $destFile = Join-Path $teamsUploadsPath $file.Name
        try {
            Copy-Item -Path $file.FullName -Destination $destFile -Force
            Write-Host "[OK] Copied $($file.Name) to $userName's Teams folder" -ForegroundColor Green
        } catch {
            Write-Warning "[ERROR] Failed to copy '$($file.Name)' to '$teamsUploadsPath': $_"
        }
    }
}

Write-Host "`n✅ Done! Background images distributed to all user Teams folders." -ForegroundColor Green
