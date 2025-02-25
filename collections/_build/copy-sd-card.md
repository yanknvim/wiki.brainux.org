---
title: SD カードへのコピー
tags:
  - Linux
  - 自力ビルド
excerpt: ""
---


# 注意

***この手順を実行すると、SD カードの内容が消去されます。必ずバックアップをしてから実行しましょう。***


# 前提

- [U-Boot のビルド](/u-boot/u-boot-build/)にてU-Bootをビルドしていること
- [Linux のビルド](/linux/linux-build/)にてLinuxをビルドしていること


# パーティションを区切る

先頭に 100MB くらいの FAT32 のパーティションを作り、残りを ext4 のパーティションにします。


## GPartedのインストール

以下のコマンドで GParted をインストールします。

```sh
sudo apt install gparted
```


## パーティションを作成する

Linux をインストールする SD カードを PC に挿入して GParted を起動します。


### GParted の起動

![GParted起動画面](/assets/images/Launch-GParted.png)

GPartedの右上のメニューから SD カード を選びます。容量で選ぶとわかりやすいです。


### パーティションの削除

![パーティションの削除](/assets/images/partition-delete.png)

***この手順を実行すると、SD カードの内容が消去されます。必ずバックアップをしてから実行しましょう。***

パーティションを右クリックしたあと "削除" をクリックしてパーティションを削除します。


### パーティションの作成

まず、FAT32 のパーティションを作成します。 "未割り当て" のパーティションを右クリックして "新規" をクリックします。

![新規パーティションの作成のダイアログ](/assets/images/create-partition-fat32.png)

上図のようなダイアログが表示されたら

- 新しいサイズ: 100MiB 程度
- ファイルシステム: fat32

に設定して "追加" をクリックし、パーティションを作成します。

次に、ext4のパーティションを作成します。

- 新しいサイズ: 残りの容量いっぱいまで
- ファイルシステム: ext4

に設定して "追加" をクリックし、パーティションを作成します。


# 必要なファイルをコピーする

1. `buildbrain`ディレクトリの中から以下ファイルをSDカードのFAT32パーティションへコピーします

    - `linux-brain/arch/arm/boot/zImage`
    - `linux-brain/arch/arm/boot/dts/imx28-[機種名].dts`

2. ターミナルを起動して `cd buildbrain` で `buildbrain` ディレクトリに入ります

以下のコマンドを実行します。

```sh
sudo cp -ar ./brainux/* /SDカードの/２つ目の/パーティション/
```

`/SDカードの/２つ目の/パーティション/` は適宜読み替えてください。
