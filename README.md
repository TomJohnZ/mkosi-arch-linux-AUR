# To add additional AUR packages
- Create the following directory structure:
```bash
mkosi.images/aur-pkg-install/mkosi.conf.d/<package-name>/PKGBUILDS
```
- Create an empty `mkosi.conf` file in the newly created <package-name> directory:
```bash
touch mkosi.images/aur-pkg-install/mkosi.conf.d/<package-name>/mkosi.conf
```
- Recursively download all target and dependency PKGBUILDs into the
  newly created directory:
```bash
cd mkosi.images/aur-pkg-install/mkosi.conf.d/package name>/PKGBUILDS
readarray -t dependencies < <(paru -Po <package-name> | grep -E "^(|TARGET)AUR") && for index in "${!dependencies[@]}"; do dependency="${dependencies[$index]}"; paru -G $(echo $dependency | cut -d" " -f3) && mv {,$(printf "%02d" $index)-}"$(echo $dependency | cut -d" " -f4)"; done
```
- Note: If you are going to commit, ensure any `.SRCINFO` files are
  committed:
```bash
  git add -f mkosi.images/aur-pkg-install/mkosi.conf.d/package-name>/PKGBUILDS/*/.SRCINFO
```
  and remove all the `.git` directories
```bash
rm -rf mkosi.images/aur-pkg-install/mkosi.conf.d/package-name>/PKGBUILDS/*/.git/
```
