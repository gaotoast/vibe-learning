# API 設計（Page Tracker - PoC 版）

## 概要

この API 設計は、Next.js Web アプリケーションのための Supabase 連携関数を定義します。PoC 版では Supabase の REST API と JavaScript クライアントライブラリを使用し、PostgreSQL データベースとの効率的な連携を実現します。

## データアクセス API

### 認証関連（Supabase Auth）

- `registerUser(email, password)` - 新規ユーザー登録
- `loginUser(email, password)` - ユーザーログイン
- `logoutUser()` - ログアウト処理
- `getCurrentSession()` - 現在のセッション情報取得
- `resetPassword(email)` - パスワードリセット用メール送信

### 書籍関連（基本的な CRUD 操作）

- `createBook(bookData)` - 書籍データの新規作成
- `getBooks(filters)` - 条件指定による書籍一覧取得
- `getBookById(id)` - 個別書籍データの取得
- `updateBookProgress(id, page)` - 読書進捗の更新
- `deleteBook(id)` - 書籍データの削除

### 統計・履歴関連

- `recordReadingActivity(bookId, pagesRead)` - 読書活動の記録
- `getReadingStats()` - 全体的な読書統計データ取得
- `getDailyProgress(startDate, endDate)` - 期間指定による日次進捗データ取得
- `getMonthlyProgress(year, month)` - 月間読書統計の取得

## リクエストとレスポンス例

### createBook(bookData)

**リクエスト**

```javascript
import { supabase } from "../lib/supabaseClient";

// 書籍データ作成関数
async function createBook(bookData) {
  try {
    const { data, error } = await supabase
      .from("books")
      .insert([
        {
          title: bookData.title,
          author: bookData.author,
          total_pages: bookData.totalPages,
          current_page: bookData.currentPage || 0,
          user_id: bookData.userId,
          start_date: new Date(),
          last_update_date: new Date(),
          memo: bookData.memo,
        },
      ])
      .select();

    if (error) throw error;
    return { success: true, book: data[0] };
  } catch (error) {
    console.error("書籍追加エラー:", error.message);
    return { success: false, error: error.message };
  }
}

// 使用例
const newBook = {
  title: "Webデザインの基本原則",
  author: "鈴木一郎",
  totalPages: 286,
  currentPage: 0,
  userId: "9f8e7d6c-5b4a-3f2e-1d0c-9b8a7f6e5d4c",
  memo: "UI/UX設計の参考文献",
};

const result = await createBook(newBook);
console.log(result);
```

**レスポンス**

```javascript
// 成功時
{
  success: true,
  book: {
    id: "e7c8a8c5-1d4f-4b3c-9c8b-8c7f1a2b3c4d",
    title: "Webデザインの基本原則",
    author: "鈴木一郎",
    total_pages: 286,
    current_page: 0,
    is_completed: false,
    start_date: "2024-06-03T10:15:00.000Z",
    last_update_date: "2024-06-03T10:15:00.000Z",
    user_id: "9f8e7d6c-5b4a-3f2e-1d0c-9b8a7f6e5d4c",
    memo: "UI/UX設計の参考文献"
  }
}

// エラー時
{
  success: false,
  error: "タイトルは必須項目です"
}
```

### updateBookProgress(id, page)

**リクエスト**

```javascript
import { supabase } from "../lib/supabaseClient";

// 読書進捗更新関数
async function updateBookProgress(id, currentPage) {
  try {
    // 書籍の総ページ数を取得
    const { data: book, error: fetchError } = await supabase
      .from("books")
      .select("total_pages")
      .eq("id", id)
      .single();

    if (fetchError) throw fetchError;

    // 完読フラグの設定
    const isCompleted = currentPage >= book.total_pages;

    // 進捗の更新
    const { data, error } = await supabase
      .from("books")
      .update({
        current_page: currentPage,
        is_completed: isCompleted,
        last_update_date: new Date(),
      })
      .eq("id", id)
      .select();

    if (error) throw error;

    // 読書履歴の記録（前回からの差分）
    await recordReadingHistory(id, currentPage - (book.current_page || 0));

    return { success: true, book: data[0] };
  } catch (error) {
    console.error("進捗更新エラー:", error.message);
    return { success: false, error: error.message };
  }
}

// 読書履歴記録関数 (補助関数)
async function recordReadingHistory(bookId, pagesRead) {
  if (pagesRead <= 0) return; // 進捗がない場合は記録しない

  try {
    const { data: userData } = await supabase.auth.getUser();

    await supabase.from("reading_history").insert([
      {
        book_id: bookId,
        user_id: userData.user.id,
        pages_read: pagesRead,
        read_date: new Date().toISOString().split("T")[0], // YYYY-MM-DD
        created_at: new Date(),
      },
    ]);
  } catch (error) {
    console.error("読書履歴記録エラー:", error.message);
  }
}

// 使用例
const result = await updateBookProgress(
  "e7c8a8c5-1d4f-4b3c-9c8b-8c7f1a2b3c4d",
  120
);
console.log(result);
```

**レスポンス**

```javascript
// 成功時
{
  success: true,
  book: {
    id: "e7c8a8c5-1d4f-4b3c-9c8b-8c7f1a2b3c4d",
    title: "Webデザインの基本原則",
    author: "鈴木一郎",
    total_pages: 286,
    current_page: 120,
    is_completed: false,
    start_date: "2024-06-03T10:15:00.000Z",
    last_update_date: "2024-06-03T14:30:00.000Z",
    user_id: "9f8e7d6c-5b4a-3f2e-1d0c-9b8a7f6e5d4c"
  }
}
```

### getReadingStats()

**リクエスト**

```javascript
import { supabase } from "../lib/supabaseClient";

// 読書統計取得関数
async function getReadingStats() {
  try {
    const { data: userData } = await supabase.auth.getUser();
    if (!userData.user) throw new Error("ユーザー認証が必要です");

    const userId = userData.user.id;

    // 書籍の統計情報を取得
    const { data: books, error } = await supabase
      .from("books")
      .select("*")
      .eq("user_id", userId);

    if (error) throw error;

    // 統計データの計算
    const totalBooks = books.length;
    const completedBooks = books.filter((book) => book.is_completed).length;
    const inProgressBooks = totalBooks - completedBooks;

    const totalPages = books.reduce((sum, book) => sum + book.total_pages, 0);
    const readPages = books.reduce((sum, book) => sum + book.current_page, 0);

    // 読書履歴から日別データを取得
    const today = new Date().toISOString().split("T")[0];
    const monthAgo = new Date();
    monthAgo.setDate(monthAgo.getDate() - 30);
    const monthAgoStr = monthAgo.toISOString().split("T")[0];

    const { data: dailyData, error: historyError } = await supabase
      .from("reading_history")
      .select("read_date, pages_read")
      .eq("user_id", userId)
      .gte("read_date", monthAgoStr)
      .lte("read_date", today)
      .order("read_date");

    if (historyError) throw historyError;

    // 日別データの集計
    const dailyProgress = dailyData.reduce((acc, item) => {
      if (!acc[item.read_date]) {
        acc[item.read_date] = 0;
      }
      acc[item.read_date] += item.pages_read;
      return acc;
    }, {});

    return {
      success: true,
      stats: {
        totalBooks,
        completedBooks,
        inProgressBooks,
        totalPages,
        readPages,
        completionRate:
          totalPages > 0 ? ((readPages / totalPages) * 100).toFixed(1) : 0,
        dailyProgress,
      },
    };
  } catch (error) {
    console.error("統計取得エラー:", error.message);
    return { success: false, error: error.message };
  }
}

// 使用例
const result = await getReadingStats();
console.log(result);
```

**レスポンス**

```javascript
// 成功時
{
  success: true,
  stats: {
    totalBooks: 5,
    completedBooks: 2,
    inProgressBooks: 3,
    totalPages: 1420,
    readPages: 678,
    completionRate: "47.7",
    dailyProgress: {
      "2024-05-01": 12,
      "2024-05-03": 25,
      "2024-05-05": 30,
      "2024-05-08": 15,
      "2024-05-10": 45,
      "2024-06-01": 20,
      "2024-06-03": 35
    }
  }
}
```

## フロントエンドフック（React Hooks）

### useBooks

ユーザーの書籍一覧を取得・管理するカスタムフック

```javascript
import { useState, useEffect } from "react";
import { supabase } from "../lib/supabaseClient";

export function useBooks(filter = "all") {
  const [books, setBooks] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchBooks = async () => {
      try {
        setLoading(true);

        const { data: userData } = await supabase.auth.getUser();
        if (!userData.user) {
          setError("認証が必要です");
          return;
        }

        let query = supabase
          .from("books")
          .select("*")
          .eq("user_id", userData.user.id)
          .order("last_update_date", { ascending: false });

        // フィルター適用
        if (filter === "reading") {
          query = query.eq("is_completed", false);
        } else if (filter === "completed") {
          query = query.eq("is_completed", true);
        }

        const { data, error } = await query;

        if (error) throw error;

        setBooks(data);
        setError(null);
      } catch (err) {
        console.error("書籍取得エラー:", err);
        setError(err.message);
      } finally {
        setLoading(false);
      }
    };

    fetchBooks();
  }, [filter]);

  return { books, loading, error };
}
```

### useStats

読書統計を取得するカスタムフック

```javascript
import { useState, useEffect } from "react";
import { getReadingStats } from "../services/bookService";

export function useStats() {
  const [stats, setStats] = useState({
    totalBooks: 0,
    completedBooks: 0,
    inProgressBooks: 0,
    totalPages: 0,
    readPages: 0,
    completionRate: 0,
    dailyProgress: {},
  });
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchStats = async () => {
      try {
        setLoading(true);
        const result = await getReadingStats();

        if (!result.success) {
          throw new Error(result.error);
        }

        setStats(result.stats);
        setError(null);
      } catch (err) {
        console.error("統計取得エラー:", err);
        setError(err.message);
      } finally {
        setLoading(false);
      }
    };

    fetchStats();
  }, []);

  return { stats, loading, error };
}
```

message: "書籍が正常に追加されました"
}

````

### updateProgress(bookId, currentPage)

**リクエスト**

```javascript
// 読書進捗を更新
const bookId = "book_001";
const currentPage = 45;

try {
  const result = await updateProgress(bookId, currentPage);
  console.log("進捗更新成功:", result);
} catch (error) {
  console.log("進捗更新失敗:", error.message);
}
````

**レスポンス**

```javascript
// 成功時
{
  success: true,
  currentPage: 45,
  progressPercent: 22.5, // 45/200 * 100
  message: "読書進捗が更新されました"
}
```

### getReadingStats(userId)

**リクエスト**

```javascript
// 読書統計を取得
const userId = getCurrentUser().uid;

try {
  const stats = await getReadingStats(userId);
  console.log("統計データ:", stats);
} catch (error) {
  console.log("統計取得失敗:", error.message);
}
```

**レスポンス**

```javascript
// 成功時
{
  totalBooks: 3,        // 登録書籍数
  completedBooks: 1,    // 完読書籍数
  totalPages: 89,       // 総読了ページ数
  booksInProgress: 2    // 読書中の書籍数
}
```

## エラーハンドリング（初心者向け）

### 基本的なエラーパターン

```javascript
// 認証エラー
{
  success: false,
  error: "auth/user-not-found",
  message: "ユーザーが見つかりません"
}

// ネットワークエラー
{
  success: false,
  error: "network-error",
  message: "インターネット接続を確認してください"
}

// データ不足エラー
{
  success: false,
  error: "validation-error",
  message: "書籍タイトルが入力されていません"
}
```

### エラーハンドリングの実装例

```javascript
// 基本的なエラーハンドリング
async function handleAddBook(bookData) {
  try {
    const result = await addBook(bookData);
    if (result.success) {
      alert("書籍が追加されました！");
    }
  } catch (error) {
    console.error("エラー:", error);
    alert("書籍の追加に失敗しました: " + error.message);
  }
}
```

## ローカルストレージの活用（初心者向け）

### 基本的な使用例

```javascript
// ユーザー設定の保存
function saveUserSettings(settings) {
  localStorage.setItem("userSettings", JSON.stringify(settings));
}

// ユーザー設定の取得
function getUserSettings() {
  const settings = localStorage.getItem("userSettings");
  return settings ? JSON.parse(settings) : null;
}

// 一時的なデータの保存（書きかけのメモなど）
function saveTempData(key, data) {
  sessionStorage.setItem(key, JSON.stringify(data));
}
```

## Firebase 設定（Windows 環境向け）

### プロジェクト設定ファイル例

```javascript
// firebase-config.js（初心者向けシンプル版）
import { initializeApp } from "firebase/app";
import { getFirestore } from "firebase/firestore";
import { getAuth } from "firebase/auth";

// Firebase 設定（実際の値は Firebase Console から取得）
const firebaseConfig = {
  apiKey: "your-api-key",
  authDomain: "your-project.firebaseapp.com",
  projectId: "your-project-id",
  storageBucket: "your-project.appspot.com",
  messagingSenderId: "123456789",
  appId: "your-app-id",
};

// Firebase 初期化
const app = initializeApp(firebaseConfig);
export const db = getFirestore(app);
export const auth = getAuth(app);
```

## API 使用時の注意点（初心者向け）

### セキュリティ

- **API キーは公開リポジトリにコミットしない**
- **環境変数（.env ファイル）を活用**
- **Firestore セキュリティルールを適切に設定**

### パフォーマンス

- **必要なデータのみ取得**（全件取得を避ける）
- **ローカルキャッシュの活用**
- **ページング機能の実装**（大量データ対応）

### エラー対応

- **try-catch で確実にエラーハンドリング**
- **ユーザーにわかりやすいエラーメッセージ表示**
- **console.log でデバッグ情報を出力**

<!-- Generated by Copilot -->
