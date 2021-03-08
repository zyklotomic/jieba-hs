# jieba-hs
![Haskell CI](https://github.com/zyklotomic/jieba-hs/workflows/Haskell%20CI/badge.svg)

TODO:
  - Serious performance issues, see Main.hs for benchmarking program, do some profiling.
  - Most likely due to personal ineptitude with lists and O(n^2) concat

In Beta!

「結巴-hs」是「[結巴](https://github.com/fxsjy/jieba)」中文分詞的Haskell版本。

"Jieba-hs" is an implementation of the "[Jieba](https://github.com/fxsjy/jieba)"
word segmentation library for Chinese in Haskell.

## 使用 Usage
jieba-hs的字典格式與jieba的一模一樣 ([原版](https://github.com/fxsjy/jieba/tree/master/extra_dict))。
字典存進`data/*`裏。HMM Model看`hmm.model`, 從[cppjieba](https://github.com/yanyiwu/cppjieba)借的
但稍微改了一些。

The format of the dictionaries are the same as jieba, see the
([original](https://github.com/fxsjy/jieba/tree/master/extra_dict)).
The HMM Model is borrowed from [cppjieba](https://github.com/yanyiwu/cppjieba) with a few
slight modifications

```haskell
import System.IO
import Dictionary
import Jieba
import Data.List (intercalate)

main :: IO ()
main = do
    contents <- hGetContents =<< openFile "dict.txt.small" ReadMode
    let dict = dictFromContents contents
    let hmmd <- readHmmDict "data/hmm.model"
    let snt = "他来到了网易杭研大厦"
    let result = cutNoHMM dict snt
    let result' = cutHMM dict hmmd snt
    putStrLn $ intercalate "/" result
    putStrLn $ intercalate "/" result
```
```
*Main> main
他/来到/了/网易/杭/研/大厦
他/来到/了/网易/杭研/大厦
```

## TODO
- [ ] TF-IDF 的字典
- [x] POS Tagging
- [ ] 查看`Data.Map.Strict` 是否已經使用了 Trie樹。~~字典後備數據結構從 Map 換成 Trie樹~~
- [ ] 解析器例外處理/應不應該使用Parsec？
- [ ] Github Actions 裏加hlint和cabal tests
- [ ] 英語版本的 README
