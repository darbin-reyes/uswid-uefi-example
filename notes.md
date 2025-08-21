llvm-objdump -s --section=.sbom fwupdx64.efi
llvm-objdump -s --section=.sbom XmlWriter.efi

uswid --load sbom.ini --save ./fwupdx64.efi --cc clang --objcopy llvm-objcopy
uswid --load D:\dev\ws\uswid-uefi-example\ucode\intel-ucode-06-03-02.json --save D:\dev\ws\ptl0\Build\PantherLakeBoardPkg\DEBUG_VS2019\X64\XmlWriter.efi --cc clang --objcopy llvm-objcopy

uswid --load intel-ucode-06-03-02.copy.json --save XmlWriter.efi --cc clang --objcopy llvm-objcopy

for .ini valid contents see parachuted\secret.ini

llvm-objdump -s --section=.sbom BIOS_PTL_U_P_H_Internal_3242.02_Dispatch_VS_DEBUG_Prod.efi

D:\dev\ws\python-uswid\uswid\format_spdx.py

D:\dev\ws\python-uswid\uswid\cli.py->

```python
def _detect_format(filepath: str) -> Optional[Any]:
    if filepath.endswith("bom.json") or filepath.endswith("cdx.json"):
        return uSwidFormatCycloneDX()
    if filepath.endswith("spdx.json"):
        return uSwidFormatSpdx()
    ext = filepath.rsplit(".", maxsplit=1)[-1].lower()
    if ext in ["exe", "efi", "o"]:
        return uSwidFormatPe()
    if ext in ["uswid", "raw", "bin"]:
        return uSwidFormatUswid()
    if ext in ["coswid", "cbor"]:
        return uSwidFormatCoswid()
    if ext == "ini":
        return uSwidFormatIni()
    if ext == "inf":
        return uSwidFormatInf()
    if ext == "xml":
        return uSwidFormatSwid()
    if ext == "json":
        return uSwidFormatGoswid()
    if ext == "pc":
        return uSwidFormatPkgconfig()
    return None
```


##############


To add an SBOM (Software Bill of Materials) to a UEFI BIOS .rom file, you typically follow these steps:

1. Generate the SBOM
Use a tool like syft, CycloneDX, or SPDX tools to generate an SBOM file (usually in JSON or XML format) for your firmware build.
2. Convert SBOM to a UEFI Section or FFS File
Use EDK2 tools to wrap the SBOM file into a UEFI section or FFS (Firmware File System) file:
Use GenSec to create a RAW or user interface section:
Optionally, use GenFfs to create an FFS file:
Replace <GUID> with a unique GUID for your SBOM.
3. Insert the SBOM Section/FFS into the BIOS Image
Use a tool like FMMT or UEFITool to insert the SBOM section or FFS file into the appropriate place in the .rom file.
Example with FMMT:



FMMT -a BIOS.rom <GUID> SbomSection.ffs BIOS_with_Sbom.rom


https://github.com/spdx/spdx-examples/tree/master/software/example1