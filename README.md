# setup-environment
Ubuntu 20.04.3 LTS  
VirtualBox 6.1.26

# ubuntu-mozc-hiragana-default
ubuntu mozc inputmodeのデフォルトをhiraganaにする

#PREINSTALL IBUS-MOZC
´´´
$ sudo apt install ibus-mozc 
$ ibus restart 
$ gsettings set org.gnome.desktop.input-sources sources "[('xkb', 'us'), ('ibus', 'mozc-jp')]"
´´´

# setup
ターミナルにて

```
sudo apt update
sudo apt upgrade -y
sudo apt install build-essential devscripts -y
sudo apt build-dep ibus-mozc -y
```
この時に以下のエラーが出てきた場合（出て来なかった場合は、続きの2番目をそのまま行う）
```
E: You must pu some 'deb-src' URIs in your sources.list
```
```
software-properties-gtk
```
上記をterminalにて入力し、Source codeの箇所を下記の様な状態にします。
![Screenshot from 2021-10-12 15-40-56](https://user-images.githubusercontent.com/85086276/136905160-c11fca73-712f-4b4d-a849-38442cba5b63.png)
続き、
```
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
その後訂正を適用してインストールをする
```
sudo dpkg -i ../mozc*.deb ../ibus-mozc*.deb
```

### References
https://dakusui.hatenablog.com/entry/2017/09/24/160400  
https://forum.zorin.com/t/ibus-mozc-how-to-change-default-input-mode-of-ibus-mozc-to-hiragana/5071
