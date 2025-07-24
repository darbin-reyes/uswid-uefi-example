llvm-objdump -s --section=.sbom fwupdx64.efi
llvm-objdump -s --section=.sbom XmlWriter.efi

uswid --load sbom.ini --save ./fwupdx64.efi --cc clang --objcopy llvm-objcopy
uswid --load D:\dev\ws\uswid-uefi-example\ucode\intel-ucode-06-03-02.json --save D:\dev\ws\ptl0\Build\PantherLakeBoardPkg\DEBUG_VS2019\X64\XmlWriter.efi --cc clang --objcopy llvm-objcopy


for .ini valid contents see parachuted\secret.ini

llvm-objdump -s --section=.sbom BIOS_PTL_U_P_H_Internal_3242.02_Dispatch_VS_DEBUG_Prod.efi

D:\dev\ws\python-uswid\uswid\format_spdx.py

D:\dev\ws\python-uswid\uswid\cli.py->def _detect_format(filepath: str) -> Optional[Any]:
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
