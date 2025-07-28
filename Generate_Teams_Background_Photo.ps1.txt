################################################################################
# Generate Teams Background Photo
# Description: Generate custom background images for Microsoft Teams
# Author : Aviel Amitay
# GitHub : https://github.com/Aviel-Amitay
# Modified: May 19 2025
################################################################################


# Load .NET image library
try {
    Add-Type -AssemblyName System.Drawing
    Write-Host "[INFO] .NET System.Drawing loaded successfully." -ForegroundColor Green
} catch {
    Write-Error "[ERROR] Failed to load System.Drawing assembly. Must run in 64-bit PowerShell."
    exit 1
}

# Paths
$sourceFolder = "C:\1"
$destinationFolder = "C:\TeamsBackgroundsPrepared"

# Verify source folder
if (-not (Test-Path $sourceFolder)) {
    Write-Error "[ERROR] Source folder '$sourceFolder' not found."
    exit 1
}

# Create destination folder if not exists
if (-not (Test-Path $destinationFolder)) {
    New-Item -Path $destinationFolder -ItemType Directory -Force | Out-Null
    Write-Host "[INFO] Created destination folder: $destinationFolder" -ForegroundColor Cyan
}

# Collect all images
$images = Get-ChildItem -Path "$sourceFolder\*" -File | Where-Object {
    $_.Extension -match '\.(jpg|jpeg|png)$'
}

if ($images.Count -eq 0) {
    Write-Warning "[WARNING] No image files found in $sourceFolder"
    exit 0
}

Write-Host "[INFO] Found $($images.Count) image(s) to process..." -ForegroundColor Yellow

# Generate a base GUID
$baseGuid = [System.Guid]::NewGuid().ToByteArray()
Write-Host "[INFO] Base GUID (bytes): $([System.BitConverter]::ToString($baseGuid))" -ForegroundColor Yellow

# Function to modify the base GUID and return a new one
function Get-ModifiedGuid {
    param (
        [byte[]]$baseBytes,
        [int]$increment
    )

    # Clone array
    $newBytes = [byte[]]::new(16)
    $baseBytes.CopyTo($newBytes, 0)

    # Modify only the first byte to keep the structure consistent
    $newBytes[0] = ($newBytes[0] + $increment) % 256

    # Return the new GUID
    return [System.Guid]::new($newBytes).ToString()
}

# Start counter
$counter = 0

foreach ($image in $images) {
    Write-Host "`n[INFO] Processing file: $($image.Name)" -ForegroundColor Magenta

    # Generate slightly unique GUID for each image
    $guid = Get-ModifiedGuid -baseBytes $baseGuid -increment $counter
    Write-Host "[INFO] Generated GUID: $guid" -ForegroundColor Cyan

    # Build file names
    $mainName = "$guid.png"
    $thumbName = "${guid}_thumb.png"
    $mainPath = Join-Path $destinationFolder $mainName
    $thumbPath = Join-Path $destinationFolder $thumbName

    try {
        # Load image
        $img = [System.Drawing.Image]::FromFile($image.FullName)
        Write-Host "[OK] Loaded image: $($image.FullName)" -ForegroundColor Green

        # Save main image
        $img.Save($mainPath, [System.Drawing.Imaging.ImageFormat]::Png)
        Write-Host "[OK] Saved: $mainName" -ForegroundColor Green

        # Save thumbnail (same content, different name)
        $img.Save($thumbPath, [System.Drawing.Imaging.ImageFormat]::Png)
        Write-Host "[OK] Saved: $thumbName" -ForegroundColor Green

        $img.Dispose()
    } catch {
        Write-Warning "[ERROR] Failed to process image: $($_.Exception.Message)"
    }

    $counter++
}

Write-Host "`n✅ Done! All processed images saved to $destinationFolder" -ForegroundColor Cyan
