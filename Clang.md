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

## printfの出力方法

[フォーマット指定子一覧](https://www.k-cube.co.jp/wakaba/server/format.html)

## gccの実行方法

MacOSの場合、brewでも入れているけどMacOSのバージョンをあげないといけなさそうな雰囲気を感じたので、XCodeで入れたやつを使用している。

```sh
/usr/bin/gcc
```

## コマンドライン引数

コマンドラインより引数を受け取った時に処理する方法

```c
int main(int argc, char *argv[]) {
  int n = 100;
  
  // argcは引数の数、argv[0]は必ずあるので2以上かどうかで引数が与えられたか確認している
  if (argc >= 2)
    // argv[0]は、実行ファイルのパス名
    n = atoi(argv[1]);
}
```

[コマンドライン引数](http://www9.plala.or.jp/sgwr-t/c/sec11-4.html)

## C言語でヒストグラムを作る

実際にはExcelで作成したが、C言語のみで完結する方法もある。

[メジアン(中央値)、範囲(レンジ)、ヒストグラムを求める](http://cgengo.sakura.ne.jp/arg04.html)

## 文字列を引数にとる

いっつも忘れるのでメモ

```c
void hoge(char *filename) {
  printf("%d\n", filename);
}

void main(void) {
  char filename[100];
  hoge(filename);
}
```

## フォルダを作成する

MacとWindowsは違うので注意

Mac

```c
#include <sys/stat.h>

const char *dir = "./data";

    if (mkdir(dir, 0777) == 0)
        printf("ディレクトリ%sを作成しました。\n", dir);
    else
        printf("ディレクトリ%sを作成できませんでした。\n", dir);
```

[C言語 - sys/stat - mkdir()](https://www.loose-info.com/main/memolist/c/lib_sys_stat_mkdir.html)

[【C言語/C++】フォルダ作成を行う - めめんと](https://mementoo.info/archives/787)

[C言語関数辞典 - fprintf](http://www.c-tipsref.com/reference/stdio/fprintf.html)

## 文字列連結

3つ以上は `sprintf` を使用する

```c
// 第一引数は、結果を格納する場所
sprintf(filename, "%s%d%s", "data/data", j + 1, ".csv");
```

[文字列の連結 | 誰のためでもないMemo](http://windyrings.jugem.jp/?eid=801)

## ファイルに出力する

`fprintf` メソッドを使用する

```c
#include <stdio.h> 
#include <stdlib.h>

int main() {
  FILE *outputfile;         // 出力ストリーム
  
  outputfile = fopen("d.txt", "w");  // ファイルを書き込み用にオープン(開く)
  if (outputfile == NULL) {          // オープンに失敗した場合
    printf("cannot open\n");         // エラーメッセージを出して
    exit(1);                         // 異常終了
  }
  
  fprintf(outputfile, "My name is Enokida Yuuichirou.\n"); // ファイルに書く

  fclose(outputfile);          // ファイルをクローズ(閉じる)
  return 0;
}
```

[ファイルへの出力](https://ylb.jp/2006b/proc/fileio/fileoutput.html)

## 簡単な構造体


```c
#include <stdio.h>

struct Physical
{
    int number;
    int height;
    int weight;
};

int main(void)
{
    struct Physical pysical[10] =
    {
        {1,168,60}
    };
    printf("%d %d %d\n", pysical[0].number, pysical[0].height, pysical[0].weight);
    // 1 168 60
    return 0;
}

```
