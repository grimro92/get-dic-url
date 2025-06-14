<script setup lang="ts">
import { ref, onMounted } from 'vue'; // refとonMountedをインポート

// バックエンドからのメッセージを保持するリアクティブな変数
const backendMessage = ref<string>(''); // 型をstringに指定

// コンポーネントがマウントされた後に実行される処理
onMounted(async () => {
  try {
    // バックエンドの /api/hello エンドポイントにリクエストを送信
    const response = await fetch('/api/hello');

    // レスポンスが正常か確認
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }

    // レスポンスボディをテキストとして取得
    const message = await response.text();

    // 取得したメッセージをリアクティブ変数にセット
    backendMessage.value = message;

  } catch (error) {
    // エラーが発生した場合の処理
    console.error("Failed to fetch from backend:", error);
    backendMessage.value = "Failed to load message from backend."; // エラーメッセージを表示
  }
});
</script>

<template>
  <div>
    <h1>ふじもんのホームページ</h1>
    <p>バックエンドからのメッセージ: <strong>{{ backendMessage }}</strong></p>
  </div>
</template>

<style scoped>
/* スタイルは今回はシンプルに */
h1 {
  color: #42b883;
  /* Vueのテーマカラー */
}

strong {
  color: #35495e;
  /* Vueのテーマカラー */
}
</style>