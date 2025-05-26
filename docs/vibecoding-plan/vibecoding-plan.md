# VibeCoding 計画書 - 読書進捗管理アプリ（PoC 版）

## 1. プロジェクト概要

### プロジェクト名

BookProgress（仮）- ReactNative 版読書進捗管理アプリ

### 目的

技術書や資格試験のテキスト読書の進捗を視覚化し、読書の継続をサポートするモバイルアプリケーションを開発する。本 PoC では、最小限の機能セットで基本的なユーザー体験を検証する。

### 主要機能（PoC 版）

- ユーザー認証（Firebase Authentication）
- 書籍の登録と進捗管理（Firestore）
- 視覚的なプログレスバーによる進捗表示
- 累計読了ページ数の表示
- クラウドデータ同期とオフラインサポート

## 2. 開発スケジュール（6 月 13 日締切）

### 週 1: 環境構築・学習・基本設計（5 月 22 日〜5 月 28 日）

- 開発環境のセットアップ
- ReactNative/Expo の基礎学習
- Firebase プロジェクト作成と接続設定
- 画面構成の決定
- Firestore データモデル設計

### 週 2: UI 実装・基本機能（5 月 29 日〜6 月 4 日）

- タブナビゲーション実装
- 書籍一覧画面実装
- 書籍追加機能実装
- プログレスバー表示

### 週 3: 完成・テスト（6 月 5 日〜6 月 13 日）

- 統計画面実装
- デザイン調整
- バグ修正
- 動作確認とデモ準備

## 3. 初心者向け段階的タスク

### 週 1: 環境構築・学習・基本設計

- [1 日目] 開発環境のセットアップ

  - [ ] Node.js と npm のインストール
  - [ ] Expo のインストール
  - [ ] ReactNative プロジェクトの作成
  - [ ] GitHub リポジトリの作成と初回コミット
  - [ ] Expo アプリをスマートフォンにインストール

- [2 日目] ReactNative の基礎学習

  - [ ] ReactNative のコア要素を理解（View, Text, Button 等）
  - [ ] 公式チュートリアルの実施
  - [ ] サンプルアプリの作成

- [3-4 日目] アプリ構造とデータモデル設計

  - [ ] 必要な画面の洗い出し（認証画面を含む）
  - [ ] ナビゲーション構造の決定
  - [ ] Firebase プロジェクト作成とセットアップ
  - [ ] Firestore データモデル設計
  - [ ] Firebase Authentication 設定

- [5-7 日目] 基本的な UI コンポーネント実装
  - [ ] タブナビゲーションの実装
  - [ ] 基本的なスタイリングの学習
  - [ ] プロトタイプ画面の作成

### 週 2: UI 実装・基本機能

- [8-9 日目] 書籍一覧画面

  - [ ] 書籍リストの表示
  - [ ] FlatList の使い方学習
  - [ ] 各書籍のプログレスバー表示
  - [ ] 「+」ボタンの設置

- [10-12 日目] 書籍追加機能

  - [ ] 入力フォーム UI 実装
  - [ ] 入力バリデーション
  - [ ] データ保存処理
  - [ ] 一覧への反映

- [13-14 日目] 進捗更新機能
  - [ ] 詳細/編集画面実装
  - [ ] 進捗更新処理
  - [ ] プログレスバーの更新動作確認

### 週 3: 完成・テスト

- [15-17 日目] 統計画面実装

  - [ ] 累計読了ページ数表示
  - [ ] 簡単なカード表示機能実装
  - [ ] レイアウト調整

- [18-20 日目] 最終調整
  - [ ] UI デザインの洗練
  - [ ] バグ修正
  - [ ] リアルデータでの動作確認
  - [ ] デモ準備
- [21 日目] 発表準備
  - [ ] スクリーンショット作成
  - [ ] デモ動画作成
  - [ ] PoC の成果まとめ

## 4. 技術スタック（簡易版）

### 開発基盤

- **言語**: JavaScript（TypeScript の導入は任意）
- **フレームワーク**: React Native
- **開発環境**: Expo (Go Workflow)

### 主要ライブラリ

- **ナビゲーション**: React Navigation
- **ローカル保存**: AsyncStorage
- **UI コンポーネント**: Expo 標準コンポーネント

### 開発ツール

- **IDE**: Visual Studio Code
- **バージョン管理**: Git + GitHub
- **パッケージ管理**: npm
- **デバッグ**: Expo DevTools

## 5. 簡易開発環境セットアップガイド

### 前提条件

- Windows PC + iPhone の環境で開発
- 初心者向けに最小限のセットアップを行う

### 手順（初心者向けステップバイステップ）

1. Node.js のインストール

   ```powershell
   # ブラウザからNode.jsの公式サイトにアクセス
   # LTS版をダウンロードしてインストール
   ```

2. プロジェクト作成

   ```powershell
   npx create-expo-app BookProgressApp
   cd BookProgressApp
   ```

3. 必要なライブラリのインストール

   ```powershell
   npm install @react-navigation/native @react-navigation/bottom-tabs @react-navigation/stack
   npm install @react-native-async-storage/async-storage
   npm install firebase
   npm install react-native-firebase
   npx expo install react-native-screens react-native-safe-area-context
   ```

4. アプリ開始

   ```powershell
   npx expo start
   ```

5. iPhone でテスト
   - iPhone に Expo Go アプリをインストール
   - QR コードをスキャンして実機で確認

## 6. 初心者向け学習リソース

### ReactNative 基礎

- [React Native 公式ドキュメント](https://reactnative.dev/docs/getting-started)
- [Expo 公式ドキュメント](https://docs.expo.dev/)
- [React Navigation 入門](https://reactnavigation.org/docs/getting-started)

### Firebase 学習

- [Firebase 公式ドキュメント](https://firebase.google.com/docs)
- [Firestore データモデル設計](https://firebase.google.com/docs/firestore/manage-data/structure-data)
- [Firebase Authentication](https://firebase.google.com/docs/auth)

### チュートリアル

- [YouTube: React Native for Beginners](https://www.youtube.com/watch?v=0-S5a0eXPoc)
- [Firebase + React Native チュートリアル](https://firebase.google.com/docs/web/setup)
- [Expo Snack: オンラインエディタ](https://snack.expo.dev/)
- [AsyncStorage 利用方法](https://react-native-async-storage.github.io/async-storage/docs/usage)

### サンプルコード

**Firebase 初期化**

```javascript
// src/services/firebase.js
import { initializeApp } from "firebase/app";
import { getAuth } from "firebase/auth";
import { getFirestore } from "firebase/firestore";

const firebaseConfig = {
  apiKey: "XXXXXXXXXXXXXXXXXXXXXXXXX",
  authDomain: "bookprogress-app.firebaseapp.com",
  projectId: "bookprogress-app",
  storageBucket: "bookprogress-app.appspot.com",
  messagingSenderId: "XXXXXXXXXXXX",
  appId: "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
};

// Firebaseの初期化
const app = initializeApp(firebaseConfig);
export const auth = getAuth(app);
export const db = getFirestore(app);
```

**基本的な画面レイアウト**

```javascript
import React, { useState, useEffect } from "react";
import { View, Text, StyleSheet, FlatList } from "react-native";
import { collection, query, where, getDocs } from "firebase/firestore";
import { db, auth } from "../services/firebase";

export default function BookListScreen() {
  const [books, setBooks] = useState([]);
  const userId = auth.currentUser?.uid;

  useEffect(() => {
    const fetchBooks = async () => {
      if (userId) {
        const q = query(collection(db, "books"), where("userId", "==", userId));
        const querySnapshot = await getDocs(q);
        const booksList = [];
        querySnapshot.forEach((doc) => {
          booksList.push({ id: doc.id, ...doc.data() });
        });
        setBooks(booksList);
      }
    };

    fetchBooks();
  }, [userId]);

  return (
    <View style={styles.container}>
      <Text style={styles.title}>書籍一覧</Text>
      <FlatList
        data={books}
        keyExtractor={(item) => item.id}
        renderItem={({ item }) => (
          <View style={styles.bookItem}>
            <Text style={styles.bookTitle}>{item.title}</Text>
            {/* プログレスバー等のコンテンツを追加 */}
          </View>
        )}
      />
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    padding: 20,
    backgroundColor: "#fff",
  },
  title: {
    fontSize: 24,
    fontWeight: "bold",
    marginBottom: 20,
  },
});
```

## 7. シンプル化したプロジェクト構造

```
BookProgressApp/
├── App.js                # アプリのエントリーポイント
├── assets/               # 画像やフォント
├── src/
│   ├── screens/          # アプリの各画面
│   │   ├── auth/         # 認証関連画面
│   │   │   ├── LoginScreen.js
│   │   │   └── RegisterScreen.js
│   │   ├── BookListScreen.js
│   │   ├── AddBookScreen.js
│   │   ├── BookDetailScreen.js
│   │   └── StatsScreen.js
│   ├── components/       # 再利用可能なコンポーネント
│   │   ├── BookItem.js
│   │   └── ProgressBar.js
│   ├── navigation/       # ナビゲーション設定
│   │   └── AppNavigator.js
│   ├── services/         # Firebase関連サービス
│   │   ├── firebase.js   # Firebase初期設定
│   │   ├── auth.js       # 認証サービス
│   │   └── firestore.js  # データベースサービス
│   └── utils/            # ユーティリティ関数
│       └── storage.js    # AsyncStorage（オフライン対応用）
├── firebase.json        # Firebase設定
├── package.json
└── README.md
```

## 8. テスト計画（簡易版）

### 手動テスト項目

- 書籍追加機能の確認

  - [ ] タイトル、著者、ページ数を入力して登録できるか
  - [ ] 必須項目のバリデーションが機能するか
  - [ ] 登録後、一覧に表示されるか

- 進捗更新機能の確認

  - [ ] 進捗更新画面に正しい情報が表示されるか
  - [ ] ページ数を更新できるか
  - [ ] プログレスバーが正しく更新されるか

- データ保存確認

  - [ ] Firebase へのデータ保存が正しく行われるか
  - [ ] ログアウト・ログイン後もデータが正しく取得できるか
  - [ ] 複数の書籍データが正しく保存されるか
  - [ ] オフラインでも操作できるか

- UI/UX 確認
  - [ ] 画面遷移がスムーズか
  - [ ] 表示が崩れる箇所はないか
  - [ ] ユーザー操作のフィードバックは適切か

## 9. PoC 評価基準

### 成功指標

1. **機能完成度**: 主要機能（書籍登録、進捗管理、統計表示）が動作すること
2. **使用感**: 直感的な操作で機能を利用できること
3. **安定性**: 基本的な操作でクラッシュしないこと
4. **データ保存**: データが正しく保存・読み込みされること

### PoC での検証ポイント

- 読書進捗の視覚化による学習モチベーション向上効果
- 操作の簡便さ（頻繁な記録更新がストレスにならないか）
- 統計情報の有用性（モチベーション維持に役立つか）

## 10. リスクと対策（初心者向け）

### 想定されるリスク

1. **学習曲線**: ReactNative の知識習得に時間がかかる
2. **機能の複雑さ**: 期間内に全機能を実装できない
3. **開発環境**: 環境構築でのトラブル

### 対策

1. **学習曲線対策**:

   - 基本的なコンポーネントから段階的に学習
   - 簡単なサンプルアプリから始める
   - コピー＆ペーストで動くコードから理解を深める

2. **機能制限対策**:

   - MVP (Minimum Viable Product)の考え方で最小機能に絞る
   - 難しい機能は後回しにする
   - 必要に応じて UI の簡素化を行う

3. **環境対策**:
   - Expo の標準機能を最大限活用
   - 複雑なネイティブ機能の利用を避ける
   - 必要に応じてオンラインのエディタ(Expo Snack)を利用

## 11. PoC 後の展望

### 短期展望（次の 1 ヶ月）

- ユーザーフィードバックの収集と分析
- UI の改良と使いやすさの向上
- バグ修正とコードのリファクタリング

### 中期展望（3〜6 ヶ月）

- Firebase 機能の拡張（Cloud Functions、Analytics）
- プッシュ通知機能の追加
- より高度な統計・グラフ表示

### 長期展望（6 ヶ月〜）

- App Store での正式公開
- 有料機能やサブスクリプションモデルの検討
- コミュニティ機能などの付加価値の創出

<!-- Generated by Copilot -->
