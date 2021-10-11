# setup-environment
Ubuntu 20.04.3 LTS  
VirtualBox 6.1.26

# ubuntu-mozc-hiragana-default
ubuntu mozc inputmodeのデフォルトをhiraganaにする

# setup
ターミナルにて

```
sudo apt update
sudo apt upgrade -y
sudo apt install build-essential devscripts -y
sudo apt build-dep ibus-mozc -y
sudo apt install debhelper -y
apt source ibus-mozc
```
ソースパッケージをインストール後訂正を行う
```
vim $(find . -name property_handler.cc)
```
```
#if IBUS_CHECK_VERSION(1, 5, 0)
const bool kActivatedOnLaunch = false;
#else
const bool kActivatedOnLaunch = true;
#endif  // IBus>=1.5.0
```
この箇所を以下の様に訂正する
```
#if IBUS_CHECK_VERSION(1, 5, 0)
const bool kActivatedOnLaunch = true;
#else
const bool kActivatedOnLaunch = true;
#endif  // IBus>=1.5.0
```
訂正後
```
cd mozc-2.23.2815.102+dfsg
dpkg-buildpackage -us -uc -b
```
その後インストールをする
```
sudo dpkg -i ../mozc*.deb ../ibus-mozc*.deb
```

### References
https://dakusui.hatenablog.com/entry/2017/09/24/160400
https://forum.zorin.com/t/ibus-mozc-how-to-change-default-input-mode-of-ibus-mozc-to-hiragana/5071
