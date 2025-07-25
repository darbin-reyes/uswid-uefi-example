
# build the .o and .efi PE file here using the compiler
e = executable('CryptoDxe.efi', sources: ['CryptoDxe.c'])

# build the uSWID intermediate file with the binary hash --
# which we would not need to do if we embedded coSWID instead...
conf = configuration_data()
conf.set('FILENAME', e.full_path())
ini_supplemental = configure_file(
  input : 'supplemental.ini',
  output : 'supplemental.ini',
  configuration : conf,
)

# convert the EDK2 inf file to an uSWID ini SBOM file
env = environment()
env.set('SRCDIR', meson.current_source_dir())
env.set('BUILDDIR', meson.current_build_dir())
env.set('TREEHASH', source_version)
ini_from_inf = custom_target('CryptoDxe.ini',
  input: 'CryptoDxe.inf',
  output: 'CryptoDxe.ini',
  command: [inf2ini, '@INPUT@', '@OUTPUT@'],
  env: env,
)

# build the uSWID intermediate file
sbom_built += custom_target('CryptoDxe.uswid',
  input: [ini_from_inf, ini_supplemental, ini_toplevel],
  output: 'CryptoDxe.uswid',
  command: [
    uswid,
    '--verbose',
    '--load', '@INPUT0@', '@INPUT1@', '@INPUT2@',
    '--save', '@OUTPUT@',
  ],
)
