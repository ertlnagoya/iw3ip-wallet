# iw3ip-wallet

IW3IP プラットフォーム向けのスマートフォン SSI ウォレット。

Sphereon の [mobile-wallet](https://github.com/Sphereon-Opensource/mobile-wallet)（Apache License 2.0, React Native）をフォークして構築する。

## 上流との関係

- **Upstream:** https://github.com/Sphereon-Opensource/mobile-wallet
- **ライセンス:** Apache License 2.0
- 上流リポジトリの `LICENSE` および `NOTICE`（存在する場合）は削除・改変せず、そのまま保持する。
- IW3IP 独自の追加・変更は本 `README.iw3ip.md` および `iw3ip/*` ブランチ側のコミットで明示する。
- 変更ファイルには Apache-2.0 の要件に従い、必要に応じて変更履歴を残す。

## リモート構成

```
origin    https://github.com/ertlnagoya/iw3ip-wallet.git
upstream  https://github.com/Sphereon-Opensource/mobile-wallet.git
```

## ブランチポリシー

- `develop` — upstream の `develop` を追従する。IW3IP 側からは直接コミットしない。
- `iw3ip/*` — IW3IP 向けの変更はすべてこのプレフィックスのブランチで行う。
  - 例: `iw3ip/phase2-consent-vc` — Phase 2 の同意 VC 対応
- PR は `ertlnagoya/iw3ip-wallet` の `develop` ブランチをベースに作成する。
- upstream への force push は行わない。upstream リポジトリには一切触らない。

### upstream 追従手順

```bash
git fetch upstream
git checkout develop
git merge --ff-only upstream/develop
git push origin develop
```

## Bundle ID / Package

- iOS bundle identifier: `jp.ac.nagoya-u.ertl.iw3ip.wallet`
- Android application ID: `jp.ac.nagoya_u.ertl.iw3ip.wallet`（Android はパッケージ名にハイフンが使えないためアンダースコア）
- 上流の Sphereon 版 ID と衝突しないように、派生ビルドは必ず上記 ID で署名する。
- 現状 `app.json` には上記を設定済み。ネイティブプロジェクト（`ios/`, `android/`）への反映は `expo prebuild --clean` の実行が必要。

## ローカル初回セットアップ

`app.json` の設定をネイティブプロジェクトに反映するには次を実行する。

```bash
# 既存の native dir を再生成（注意: ios/ と android/ の手動変更が消える）
rm -rf ios android
npx expo prebuild --clean
```

その後、IW3IP 側で用意した署名鍵と EAS project を `eas.json` / `app.json` の `extra.eas.projectId` に追加して `eas build` を実行する。

## 関連ドキュメント

- IW3IP ハンズオン: [docs/hands-on/ha-ssi-wallet.md](https://github.com/ertlnagoya/iw3ip.github.io/blob/main/docs/hands-on/ha-ssi-wallet.md)
