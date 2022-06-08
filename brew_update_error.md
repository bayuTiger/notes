## 状況
先程`brew update`しようとしたら以下のエラーが発生しました

```zsh
❯ brew update
fatal: unable to access 'https://github.com/Homebrew/homebrew-core/': Could not resolve host: github.com
fatal: unable to access 'https://github.com/Homebrew/brew/': Could not resolve host: github.com
fatal: unable to access 'https://github.com/Homebrew/homebrew-core/': Could not resolve host: github.com
fatal: unable to access 'https://github.com/Homebrew/brew/': Could not resolve host: github.com
Error: Fetching /opt/homebrew/Library/Taps/homebrew/homebrew-core failed!
Fetching /opt/homebrew failed!
```

## 環境

- Homebrew 3.4.10
- macOS Monterey v12.3.1

## 解決

以下の２つのコマンドのうち、１つを実行すれば解決します

```zsh
brew cleanup
brew upgrade
```
:::note warn
理由は後述しますが`brew upgrade`がオススメです！
:::

## 説明
自分は最初「キャッシュが原因かな？」と考えたので`brew cleanup`を実行したのですが、そのとき以下の表示がされました
```zsh
Warning: Skipping git: most recent version 2.36.1 not installed
Removing: /Users/takayamatoranosuke/Library/Caches/Homebrew/git--2.36.0... (15.5MB)
==> This operation has freed approximately 15.5MB of disk space.
```

この表示から「git のバージョンが上がったからアクセスできなかったんだ！」と分かりました

その後`brew update`を実行し、以下の表示を確認できました

```zsh
❯ brew update
Updated 1 tap (homebrew/core).
==> New Formulae

~~省略

You have 1 outdated formula installed.
You can upgrade it with brew upgrade
or list it with brew outdated.
```

先程のcleanupから「gitがアップデートされてない！」ということが判明したので、次に`brew upgrade`を実行しました

```zsh
❯ brew upgrade
==> Upgrading 1 outdated package:
git 2.36.0 -> 2.36.1
省略~~
```

無事gitのバージョンアップが完了しました！

このことから`brew cleanup`より`brew upgrade`の方をオススメする理由は、結局gitのアップデートをかけるためにはupgradeしなといけないからです

今後もgitのバージョンアップは継続されていくと思うので、ここに対応を記録しておきます

