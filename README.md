# GARMENT ISOLATER

Windows background removal for clothing and accessory images, with an offline mask editor and an optional File Explorer **Remove background** action.

This repository contains the source and small Explorer adapter from the working **1.1.1** manual-tools release. First-run setup downloads the pinned processing engine, dependencies and models. Processing then runs locally, offline, without an account or API key.

## Start here

1. Download or clone the repository, and extract it to a permanent folder. Do not launch from inside a ZIP. Windows 10/11 **x64** is required.
2. Double-click **Setup Windows.cmd**. Internet access is needed for this first setup. Let the window finish; it checks the downloads before reporting success.
3. Double-click **Test Tools.cmd** for a small synthetic-image self-test.
4. Double-click **Background Remover.cmd**, add your images, choose an output folder, and click **Remove backgrounds**.

Keep the folder together after setup. The `runtime/` and `models/` folders contain downloaded local dependencies. Running setup again verifies completed downloads and can finish an interrupted setup.

## What each tool does

- **Background Remover.cmd** opens the batch window. Detailed/U2NET is the default; Quick/U2NETP is the smaller model.
- **Mask Editor.cmd** opens the local browser editor. Use current Chrome or Edge.
- **Install Remove Background Menu.cmd** adds the optional action for the current Windows user after setup. Select images in File Explorer, then use **Shift + right-click > Remove background**, or **Show more options > Remove background**.
- **Queued Background Jobs.cmd** reopens the Explorer job queue.
- **Uninstall Remove Background Menu.cmd** removes the menu registration belonging to this tool copy.

The Explorer adapter passes the full selection through Windows `IDataObject`/`IDropTarget`, supporting batches of 100+ files without placing every filename on one command line. Images from different folders are grouped by source folder. Explorer results are saved beside the selected images.

If another copy already owns the menu registration, use that copy's uninstaller before registering this one. Keep an installed tool in the same location; uninstall its menu before moving it.

## Outputs and preservation

Every run creates a fresh `GARMENT_BATCH_...` folder containing:

- `Sources PNG/`: oriented source PNGs for comparison and editing.
- `Masks/`: native-size grayscale coverage masks.
- `Cutouts/`: native-size transparent PNGs.
- Processing receipts and integrity records.

Original files are preserved. Output basenames are preserved. Two inputs with the same output basename are rejected before a batch starts. Existing transparency is preserved by default; the window offers explicit re-segmentation if needed. Coverage is applied to source alpha once. Compatible RGB pixels and colour profiles are retained at native size.

This tool keeps the source proportions and dimensions. Use your chosen resizer separately for enlargement. PNG export does not restore detail absent from a source.

## Correct a mask

1. Open **Mask Editor.cmd**.
2. Load the matching image from `Sources PNG/` and its PNG from `Masks/`.
3. Paint **Keep** to restore source material or **Remove** to clear retained background. Use zoom, undo and contrasting backgrounds to inspect the result.
4. Export the grayscale PNG mask.
5. In the remover, choose **Apply saved mask**, then select the matching original image and your saved mask. A fresh batch preserves the original colours and profiles.

The editor can also export a transparent PNG directly using browser-decoded colours. Keep requires an intact source: a coverage mask cannot recover pixels that are absent from an already-damaged cutout.

## Known limits

Automatic masks are proposals, not a guarantee that every garment detail is isolated correctly. Check pale fabric, fur, lace, thin chains, straps, handles, detached accessories and internal openings. A general foreground model can retain stands or background between trouser legs, and can miss narrow parts. The mask editor is included for correction.

If setup or processing reports an error, keep the window open and read the message. Completed outputs remain available. Let slow operations finish before starting the same job again.

If Windows Controlled Folder Access blocks an output folder, choose a writable folder or review Windows Security's allowed-app setting for this copy's verified `runtime/node.exe`. Setup and the tool do not change Windows Security settings.

Batch receipts record the paths chosen for that run. Moving image files preserves their contents but does not rewrite paths in those receipts.

## Repository contents

`Background Remover.ps1`, `helpers/`, `scripts/manual_batch.cjs`, `scripts/output_paths.cjs` and `mask-editor.html` are copied from the current installed release. `ExplorerIntegration/source/` contains the native adapter source and build command. The small tested DLL is supplied for users who do not have a C++ compiler.

Pinned setup: Node **24.19.0**, sharp **0.35.5**, onnxruntime-node **1.30.0**, and the checked U2NET/U2NETP model releases. See `DEPENDENCIES.json` for download and integrity details. The source ZIP excludes the large runtime binaries, model weights, personal images and generated batches.

For the browser upload steps, see [GITHUB-UPLOAD.md](GITHUB-UPLOAD.md). Third-party licence texts and source references are in `licences/`.

The fresh-extraction setup and synthetic-image checks are recorded in [VERIFICATION.md](VERIFICATION.md).
