# Flatpak VSCodium Insiders

## Issues
Please open issues under: https://github.com/flathub/com.vscodium.codium-insiders/issues

## FAQ

### About the access to the host filesystem
Note that VSCodium Insiders is granted *full access to your host directories*
[(but at certain grade is still sandboxed)](https://docs.flatpak.org/en/latest/sandbox-permissions.html#filesystem-access).

You can use `flatpak override` to locally adjust this if you prefer to a more strong
hardened sandbox for VSCodium Insiders file system access:
```bash
flatpak override --user com.vscodium.codium-insiders --nofilesystem=host
```

Now manually grant accesss to the folder(s) you want to work in
```bash
flatpak override --user com.vscodium.codium-insiders --filesystem=~/src
```

> Remember: this version is running inside a _container_ and is therefore __not able__
to access SDKs on your host system!

### To execute commands on the host system, run inside the sandbox:

  `$ flatpak-spawn --host <COMMAND>`

  or

  `$ host-spawn <COMMAND>`

  - Most users seem to report a better experience with `host-spawn`

Note that this runs the COMMAND without any further host-side confirmation.
If you want to prevent such full host access from inside the sandbox, you can use `flatpak override` as follows:
```
flatpak override --user com.vscodium.codium --no-talk-name=org.freedesktop.Flatpak
```

### Where is my X extension? AKA modify product.json

Since are serveral ways to achieve this the better is to use [vsix-manager](https://open-vsx.org/extension/zokugun/vsix-manager)

### Host Shell

To make the Integrated Terminal automatically use the host system's shell,
you can add one of the following configurations for flatpak-spawn or host-spawn to the settings of vscodium:

`flatpak-spawn`

```json
  {
    "terminal.integrated.defaultProfile.linux": "bash",
    "terminal.integrated.profiles.linux": {
      "bash": {
        "path": "/usr/bin/flatpak-spawn",
        "args": ["--host", "--env=TERM=xterm-256color", "bash"],
        "icon": "terminal-bash",
        "overrideName": true
      }
    },
  }
```

`host-spawn`

```json
  {
    "terminal.integrated.defaultProfile.linux": "bash",
    "terminal.integrated.profiles.linux": {
      "bash": {
        "path": "/app/bin/host-spawn",
        "args": ["bash"],
        "icon": "terminal-bash",
        "overrideName": true
      }
    },
  }
```

### SDKs

This flatpak provides a standard development environment (gcc, python, etc).
To see what's available:

```bash
  $ flatpak run --command=sh com.vscodium.codium-insiders
  $ ls /usr/bin (shared runtime)
  $ ls /app/bin (bundled with this flatpak)
```
To get support for additional languages, you have to install SDK extensions, e.g.

```bash
  $ flatpak install flathub org.freedesktop.Sdk.Extension.dotnet
  $ flatpak install flathub org.freedesktop.Sdk.Extension.golang
  $ FLATPAK_ENABLE_SDK_EXT=dotnet,golang flatpak run com.vscodium.codium-insiders
```
You can use

```bash
  $ flatpak search <TEXT>
```
to find others.

### Run flatpak codium from host terminal

If you want to run `codium /path/to/file` from the host terminal just add this
to your shell's rc file

```bash
alias codium="flatpak run com.vscodium.codium-insiders "
```

then reload sources, now you could try:

```bash
$ codium /path/to/
# or
$ FLATPAK_ENABLE_SDK_EXT=dotnet,golang codium /path/to/
```

### Git LFS support

To use Git LFS with VSCodium's integrated Git support, you will need the `com.visualstudio.code.tool.git-lfs` addon.

```bash
  $ flatpak install flathub com.visualstudio.code.tool.git-lfs
```

You might need to reinstall the Git LFS hooks in each repo with `git lfs install`.

## Related Documentation

- https://github.com/VSCodium/vscodium/blob/master/DOCS.md
- https://code.visualstudio.com/docs#vscode
