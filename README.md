# TelemedicineApp (source + OHIF redirect + GitHub Actions)

This package contains the TeleMedMvc source and a minimal OHIF redirect page (wwwroot/ohif/index.html).
It also includes a GitHub Actions workflow that builds a self-contained Windows single-file executable.

Important: I can't compile the Windows EXE inside this chat environment. Use the GitHub Action (recommended) or publish locally.

## Steps to get TelemedicineApp.exe via GitHub Actions
1. Create a new GitHub repository and push the contents of this folder to the repository's `main` branch.
2. Open the repository on GitHub, go to Actions -> "Publish Windows EXE", and click "Run workflow".
3. After the workflow completes, download the artifact named `TelemedicineApp-windows`. It contains the published EXE and supporting files.
4. Unzip the artifact, place the `ohif` folder next to the EXE if not included, and run `TeleMedMvc.exe` on the Windows machine that hosts Orthanc.

## Or publish locally on Windows
Requirements: .NET 8 SDK installed.
```powershell
cd TeleMedMvc
dotnet restore
dotnet publish -c Release -r win-x64 --self-contained true /p:PublishSingleFile=true -o publish
```
Find the EXE in `TeleMedMvc/publish`.

## Configuration
- Orthanc base URL is in `TeleMedMvc/appsettings.json` (default: http://localhost:8042/).
- Login credentials: username `nexo`, password `nexo1234` (hardcoded).
- Mapping DB: `mapping.db` auto-created next to EXE when run.

## Notes about OHIF
This package uses a lightweight `wwwroot/ohif/index.html` that redirects to the public OHIF viewer (https://viewer.ohif.org) with `serverUrl` set to your local Orthanc DICOMweb endpoint.
If you want a fully offline OHIF bundled, you must build OHIF locally and copy its build output into `wwwroot/ohif` before publishing.