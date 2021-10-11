# setup-environment
Ubuntu 20.04.3 LTS  
VirtualBox 6.1.26

# ubuntu-mozc-hiragana-default
ubuntuを起動時にmozcのデフォルトをhiraganaにする

```
sudo apt update
sudo apt upgrade -y
sudo apt install build-essential devscripts -y
sudo apt build-dep ibus-mozc -y
sudo apt install debhelper -y
apt source ibus-mozc
```
ソースパッケージをインストール後
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
