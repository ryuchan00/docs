## 二次元配列を引数に渡すには

```c
// 二次元配列の[]は最初の配列にしか適応できない
int sum_all1(int xlength, int arr[][3]) {
}

// 解決策:全て引数として渡してしまえば良い
int sum_all2(int size1, int size2, int arr[size1][size2]) {
}

// 呼び出す例としてはこんな感じ
int main(void) {
  int arr[][3] = {{1,2,3}, {4,5,6}, {7,8,9}};
  printf("%d", sum_all2(3, 3, arr));
}

// ちなみに配列の形が固定で決まっているのであれば次のような例でも問題ない
int sum_all3(int n, int arr[2][3]) {
}
```

[C言語の引数に多次元配列を渡す - Qiita](https://qiita.com/Hiraku/items/babed27bc1d750c2e12d)

