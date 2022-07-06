# M1 MacでJavaの環境構築をする 2022年5月

## 環境

- macOS Monterey 12.3.1
- VSCode
- openjdk 18.0.1 2022-04-19
- javac 18.0.1

## 本記事で達成すること

1. 「M1 Mac」に
1. 「OpenJDK 18」を入れて
1. 「VSCode」で開発できるようにする

## 本記事の構成

1. Javaの環境構築を行う
1. VSCodeの設定を整える

## 1. M1 MacにJavaの環境を構築する

### 1-1 JDKインストール

- [azulのサイト](https://www.azul.com/downloads/?os=macos&architecture=arm-64-bit&package=jdk)からダウンロード
- 画面を下にスクロールしていくと、参照画像のボタンからインストールすることができる
![JDKインストール画面](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fff4f941d-e39a-4a5a-91d7-45d6cee60fb7%2FJDK.png?id=3ff0319d-96cf-48ce-a93e-ddd4ec146e4f&table=block&spaceId=746a82b8-e627-4f8e-86d3-12788b947a52&width=2000&userId=67c4617e-b3ef-4dc8-ba24-817a58bd5a2f&cache=v2)

### 1-2 環境変数の設定

- ターミナルで下記のコマンドを実行

```zsh
cd
echo JAVA_HOME=/Library/Java/JavaVirtualMachines/zulu-18.jdk/Contents/Home >> .zshrc
source .zshrc
echo $JAVA_HOME

echo PATH=$PATH:$JAVA_HOME/bin >> .zshrc
source .zshrc
echo $PATH
```

- **確認コマンド**を入力する

```zsh
❯ java --version
openjdk 18.0.1 2022-04-19
OpenJDK Runtime Environment Temurin-18.0.1+10 (build 18.0.1+10)
OpenJDK 64-Bit Server VM Temurin-18.0.1+10 (build 18.0.1+10, mixed mode)

❯ javac --version
javac 18.0.1
```

## 2. VSCodeの設定を整える

### 2-1 拡張機能：Extension Pack for Java をインストールする
- [ココ！](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-pack)からインストールすることができる

### 2-2 setting.jsonに追記する

- vscode上で`command + ,` を入力し、設定画面を表示する
- 以下の画像から**setting.json**を開く
![vscの設定画面](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fe0b98f1f-b13d-45c1-bca8-c15bb65728ce%2Fjava_vsc.png?id=7b78d236-3b5f-4198-9dc1-0906f8ab5fb0&table=block&spaceId=746a82b8-e627-4f8e-86d3-12788b947a52&width=2000&userId=67c4617e-b3ef-4dc8-ba24-817a58bd5a2f&cache=v2)

- setting.jsonに`  "java.jdt.ls.java.home": "/Library/Java/JavaVirtualMachines/zulu-18.jdk/Contents/Home"` を追記する
![setting.jsonに追記](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F08a1898f-423e-4c26-ae9c-feb7c9b29950%2Fsettings_json__schoo.png?id=a5eece62-e7d2-4146-b714-2e55fce06f91&table=block&spaceId=746a82b8-e627-4f8e-86d3-12788b947a52&width=2000&userId=67c4617e-b3ef-4dc8-ba24-817a58bd5a2f&cache=v2)

## おまけ：assertsを有効化する

1. launch.jsonを作成する
 ![launch.jsonの作成](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F68525302-b457-4229-9441-69f1a7109029%2F92203646-39d4e900-eeb4-11ea-837c-d707b6fde7ba.png?id=7f75b96d-f6bc-4b42-a525-fc86284ab118&table=block&spaceId=746a82b8-e627-4f8e-86d3-12788b947a52&width=2000&userId=67c4617e-b3ef-4dc8-ba24-817a58bd5a2f&cache=v2)
1. launch.jsonに`  "java.debug.settings.vmArgs": "-ea"
`を追記する
![launch.jsonに追記](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F942ae02e-7508-4044-b5f4-25a16fcb8084%2Flaunch_json__schoo.png?id=923b7826-30d9-4a21-a5d7-292293efb224&table=block&spaceId=746a82b8-e627-4f8e-86d3-12788b947a52&width=2000&userId=67c4617e-b3ef-4dc8-ba24-817a58bd5a2f&cache=v2)

## 参考にさせていただいた記事

https://zenn.dev/osuzuki/articles/b41dc7be15e2b5

https://qiita.com/suke_masa/items/f9af0fb84ad9447ae961#%E7%92%B0%E5%A2%83%E5%A4%89%E6%95%B0%E3%81%AE%E8%A8%AD%E5%AE%9A

https://codeaid.jp/vscode-debug/

