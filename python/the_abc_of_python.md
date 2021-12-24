{%hackmd DfWYF9cYREebVNN1eEOz-w %}

Python 初碰面 :kissing_heart:
======

###### tags: `python` `入門`

> ***不知自己不知道, 那你會以為你知道.***

### 元組(Tuple)

:::info
**Tuple 是==唯獨的==且還可用於建立==多型別==的資料集合**
:::

- 建立 Tuple => 於 () 符號中輸入資料, 並且以逗號區隔
- 存取 Tuples 元素的方法:
    - 使用 [] 符號並傳入索引值(從0開始計算)來進行存取
    - 取特定範圍的Tuples(元組)元素, 使用 [:] 符號並傳入索引值 ![](https://i.imgur.com/QtiTV3A.png)
- 尋找 Tuples 元素的方法:
    :::info
    取得 Tuples 元素索引值前, 需先判斷元素是否存在, 否則當元素不存在時, 會發生 ValuesError 的例外:
    ```python=16
    if 100 in Tuples
        print(Tuples.index(100))
    else
        print("100 not in Tuples.")
    ```
    :::
    - 使用 index(), 取對應元素的 index 位置 => Tuples.index(30)
    - 使用 count() 取對應的總數 => Tuples.count(50)

------

### 函式 *arfgs, **kwargs 運算子

- 單參接收多筆輸入值 ***arfgs:**
![](https://i.imgur.com/naFp6C3.png)

- 單參接收多筆資料並打包成字典(Dictionary) ***arfgs:**
![](https://i.imgur.com/Ilu3HXH.png)

------

### module vs. package:

- python 只要將檔案獨立, 名稱即為該檔案名稱(name of module). 使用方式如下:
    ```python=1
    # 引用整個模組
    import post
    import about
    p = post.Post()
    p.add_post("Python Programming")
    author = about.get_author()
    email = about.get_email()
    print(p.titles)  #執行結果：['Python Programming']
    print(author)  #執行結果：Mike
    print(email)  #執行結果：learncodewithmike@gmail.com
    ```

- 將數個 module 抽出後放在資料夾內, 名稱即為資料夾名稱(name of package). 使用方式如下:
    ```python=1
    # 從套件中引用模組
    import blog.post
    import blog.about
    p = blog.post.Post()
    p.add_post("Python Programming")
    author = blog.about.get_author()
    email = blog.about.get_email()
    ```

:::info
使用第三方套件(Package)時, 注意以下要點 :+1:
- PyPI 套件庫位於: https://pypi.org
- 通常都透過 pip 管理第三方套件
- 透過 pip 下載的第三方套件, 是供全域使用的.
- 如果有專案需要不同版本地三方套件需求, 目前需額外透過 pipenv 使用虛擬環境方式來達成.
- Pipfile 及 Pipfile.lock 檔案
    - Pipfile: 專案相關資訊, 套件安裝 ...等資訊紀錄於此
    - Pipfile.lock: 相依套件的資訊記錄於此, 且會紀錄 migarate 相關資訊.
:::
------

### lambda & Comprehension:rocket: 

***==可運用在任何可疊代的物件(Iterable Object), 一行程式碼即可完成多行的任務==***