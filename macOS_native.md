# macOS native 環境での 準備方法

- 本セクションでは、Lin4Neuro を使わずに、macOS native 環境でのセットアップ方法を記載します。Apple M1 でも対応可能です。ただし、この方法でのセットアップのサポートは限られることをご了承ください(個人個人で環境がかなり異なるためです)。このインストラクションを読んでわからないことが多い方は、ご自身でのセットアップは難しいとお考えいただき、Lin4Neuroでの受講としてください

## 前提条件

- CPUは Intel でも Apple M1 でも問いません
- ターミナルはデフォルトの zsh を使用することとします

## インストールが必要なソフトウェア
- git
- MRIcroGL
- XQuartz
- FSL
- MRtrix3
- ANTs
- Matlab
- SPM


### git

#### インストール
- Command line tools for Xcode のインストールにより git を使うことが可能となります

```
xcode-select --install
```

#### 確認
- ターミナルから以下をタイプしていただき、バージョンが出力されれば大丈夫です

```
git --version
```


### MRIcroGL

#### インストール
- MRIcroGL は以下のリンクからインストーラーを入手できます
- https://github.com/rordenlab/MRIcroGL/releases/download/v1.2.20211006/MRIcroGL_macOS_which2.dmg
- インストール後、以下のコマンドを実行し、.zprofileに設定を書き込みます。bashの方は.bash_profileに置き換えてください

```
echo '' >> ~/.zprofile
echo '#MRIcroGL' >> ~/.zprofile
echo 'PATH=$PATH:/Applications/MRIcroGL.app/Contents/Resources' >> ~/.zprofile
```

#### 確認
- 一度ターミナルを終了し、ターミナルを再度起動した後に、以下をタイプしてください

```
dcm2niix --version
```

この結果が、v1.0.20211006 と表示されれば大丈夫です

### XQuartz
- XQuartz は FSL の実行のために必要です

#### インストール
- 以下からインストーラーを入手し、実行します
- https://github.com/XQuartz/XQuartz/releases/download/XQuartz-2.8.1/XQuartz-2.8.1.dmg

#### 確認
- FSLが実行されればXQuartzもきちんとインストールされるのでここでは確認しません

### FSL (SPMコースでは不要です)
#### 事前準備
- FSLはインストーラーがpython3に対応しましたが、そのためにxcode-selectをインストールする必要があります。上でgitのインストールをしている場合は既に入っています。まだの方は、gitのところをご確認ください

#### インストール
- 以下をターミナルから実行し、fslinstaller.pyを入手し、実行します

```
cd ~/Downloads
curl -O https://fsl.fmrib.ox.ac.uk/fsldownloads/fslinstaller.py
cd ~/Downloads
python ./fslinstaller.py 
```

- インストール完了後、FSLの設定は .profile に記載されるのですが、zshではうまく読み込まれないことがあるので、.zprofileにも記載します

- ターミナルから以下を実行します

```
echo '' >> ~/.zprofile
echo '# FSL Setup' >> ~/.zprofile
echo 'FSLDIR=/usr/local/fsl' >> ~/.zprofile
echo 'PATH=${FSLDIR}/bin:${PATH}' >> ~/.zprofile
echo 'export FSLDIR PATH' >> ~/.zprofile
echo '. ${FSLDIR}/etc/fslconf/fsl.sh' >> ~/.zprofile
```

- これが終わったら一度ターミナルを終了し、再びターミナルを起動します

#### 確認
- ターミナルから以下をタイプします
```
fsl
```

これでFSLが立ち上がればOKです

### MRtrix3 (SPMコースでは不要です)
#### インストール
- ターミナルから以下を実行します

```
sudo bash -c "$(curl -fsSL https://raw.githubusercontent.com/MRtrix3/macos-installer/master/install)"
```

#### 確認
- ターミナルから以下を実行します

```
mrview
```

- MRViewが起動すれば大丈夫です

### ANTs (SPMコースでは不要です)
#### インストール
- ターミナルから以下を実行します

```
cd ~/Downloads
curl -O https://raw.githubusercontent.com/kytk/shell-scripts/master/ANTs_installer_macOS.sh
chmod 755 ANTs_installer_macOS.sh
./ANTs_installer_macOS.sh
```
#### 確認
- "ANTs is installed" "Please close and re-run the terminal to reflect PATH setting" と出たら、ターミナルを一度閉じて、再度ターミナルを開きます

- ターミナルから以下を実行します

```
ANTS
```

- "call ANTS or ANTS --help" と出れば大丈夫です


### Matlab
- Matlabは各自購入してください。Baseだけで大丈夫です。必要なバージョンは以下のリンクが参考になります
- https://jp.mathworks.com/support/requirements/previous-releases.html

### SPM
#### インストール
- GitHub経由が便利です
- ホームディレクトリの下に git というディレクトリを作成し、その下に spm12 をインストールすることとします

```
cd ~
mkdir git #なければ作成
git clone https://github.com/spm/spm12.git
```

- さらにターミナルから以下を実行します

```
sudo xattr -r -d com.apple.quarantine ~/git/spm12
sudo find ~/git/spm12 -name '*.mexmaci64' -exec spctl --add {} \;
```

- この後、Matlabのパス設定で、~/git/spm12 を指定してください

#### 確認
- Matlab から

```
spm
```

とタイプし、SPMが起動すればOKです

