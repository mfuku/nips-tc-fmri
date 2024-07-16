# macOS native 環境での 準備方法

- 本セクションでは、Lin4Neuro を使わずに、macOS native 環境でのセットアップ方法を記載します。Apple Silicon でも対応可能です。ただし、この方法でのセットアップのサポートは限られることをご了承ください(個人個人で環境がかなり異なるためです)。

## 前提条件

- CPUは Intel でも Apple Silicon でも問いません
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
- https://github.com/rordenlab/MRIcroGL/releases/download/v1.2.20220720/MRIcroGL_macOS.dmg
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

この結果が、v1.0.20220720 と表示されれば大丈夫です

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
/usr/bin/python3 ./fslinstaller.py 
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

#### ANTsのインストール
- ターミナルに以下をコピペします

- intel macの場合

    ```
    cd ~/Downloads
    curl -O https://www.nemotos.net/l4n-abis/macOS_2023/ANTS_maci64.zip
    unzip ANTS_maci64.zip -d ~/
    grep '$HOME/ANTS/install/bin' ~/.zprofile > /dev/null
    if [ $? -eq 1 ]; then
      echo "" >> ~/.zprofile
      echo "#ANTs" >> ~/.zprofile
      echo 'export ANTSPATH=$HOME/ANTS/install/bin' >> ~/.zprofile
      echo 'export PATH=$PATH:$ANTSPATH' >> ~/.zprofile
    fi

    ```
- Apple Siliconの場合

    ```
    cd ~/Downloads
    curl -O https://www.nemotos.net/l4n-abis/macOS_2023/ANTS_arm64.zip
    unzip ANTS_arm64.zip -d ~/
    grep '$HOME/ANTS/install/bin' ~/.zprofile > /dev/null
    if [ $? -eq 1 ]; then
      echo "" >> ~/.zprofile
      echo "#ANTs" >> ~/.zprofile
      echo 'export ANTSPATH=$HOME/ANTS/install/bin' >> ~/.zprofile
      echo 'export PATH=$PATH:$ANTSPATH' >> ~/.zprofile
    fi

    ```




#### 確認
- "ANTs is installed" "Please close and re-run the terminal to reflect PATH setting" と出たら、ターミナルを一度閉じて、再度ターミナルを開きます

- ターミナルから以下を実行します

```
ANTS
```

- "call ANTS or ANTS --help" と出れば大丈夫です


### SPM12: Matlab をお持ちの場合
- SPM12 はMatlabを持っているか持っていないかでインストールの方法が変わります。Matlab をお持ちでない方は、次の "SPM12: Matlab をお持ちでない場合" に従ってセットアップをしてください

- Matlab R2023b 以降、Apple siliconにnative対応しましたが、ときにバグがあるようです。Matlab R2022b の環境で構築しています。

### SPM12のインストール
- GitHub経由が便利です
- ホームディレクトリの下に spm12 をインストールすることとします

    ```
    cd #ホームディレクトリに移動します
    git clone https://github.com/spm/spm12.git

    ```

- SPMはmacOSのセキュリティで実行できないことがあるため、この問題を回避するために、ターミナルから以下を実行します

    ```
    sudo xattr -r -d com.apple.quarantine ~/spm12
    sudo find ~/spm12 -name '*.mexmaci64' -exec spctl --add {} \;

    ```

- この後、Matlabのパス設定で、/Users/ご自分のユーザ名/spm12 を指定してください

#### SPM12の確認
- Matlab から

    ```
    spm
    ```

とタイプし、SPMが起動すればOKです


### SPM12: Matlab をお持ちでない場合

- コース用に SPM12 を Matlab がなくても動作するようにスタンドアロン版を作成しました。Intel macでもApple siliconでも動作します。以下に従ってセットアップを行ってください

### Matlab Runtime R2022b の入手

- ターミナルに以下を入力し、Matlab Runtime R2022b を入手します。Intel Mac, Apple Silicon ともに共通です。

    ```
    cd ~/Downloads
    curl -O https://ssd.mathworks.com/supportfiles/downloads/R2022b/Release/7/deployment_files/installer/complete/maci64/MATLAB_Runtime_R2022b_Update_7_maci64.dmg.zip
    unzip MATLAB_Runtime_R2022b_Update_7_maci64.dmg.zip

    ```

- ダウンロードフォルダにある MATLAB_Runtime_R2022b_Update_7_maci64.dmg をダブルクリックします

- InstallForMacOSX をダブルクリックします。**インストール先はデフォルトのまま変更しないでください**

### SPM12 standalone のインストール

- 以下で SPM12 standalone を入手し、/Applications の下にインストールします。

    ```
    cd ~/Downloads
    curl -O https://www.nemotos.net/l4n-abis/macOS_2023/spm12_maci64.zip
    unzip spm12_maci64.zip -d /Applications/
    echo "" >> ~/.zprofile
    echo "# Alias for SPM12" >> ~/.zprofile
    echo "alias spm='/Applications/spm12_standalone/run_spm12.sh /Applications/MATLAB/MATLAB_Runtime/R2022b'" >> ~/.zprofile

    ```

- 一度ターミナルを閉じます。

### SPM12 standalone の確認

- GUIとコマンドラインのどちらからも起動できます

- GUI の場合は、アプリケーションの中にある spm12_standalone の中の spm をダブルクリックしてください

- コマンドラインの場合は、ターミナルを起動した後、spm と入力すればSPMが起動します。ただ、初回は起動するまでに数分時間がかかるため、焦らずにお待ちください


    ```
    spm
    ```



とタイプし、SPMが起動すればOKです

