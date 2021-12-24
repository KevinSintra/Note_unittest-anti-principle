{%hackmd DfWYF9cYREebVNN1eEOz-w %}

單元測試的藝術 :kissing_closed_eyes:
======

###### tags: `unittest` `書` `單元測試的藝術`

> ***不知自己不知道, 那你會以為你知道.***

此篇前言
------
其它章節:
[round 2=> link](https://hackmd.io/@GoldxTree/H1v35SGPF)
[round 3=> link](https://hackmd.io/@GoldxTree/Sk7ih7fdF)

此篇程式碼範例用跟書中不同語言跟工具做實現, 主要想摸點新玩意:v:, 讓大腦活動活動. 內容多數都會帶入個人觀點去做紀錄.

- 環境:
    - 語言: Python 3.9.7
    - 測試工具: pytest-6.2.5
    - plugins: mock-3.6.1
        

### chepter1 單元測試基礎
- 虛設常式(Stub)的定義
- 透過 Stub 來重構程式碼
- 克服程式碼中的封裝問題
- 探討使用虛設常式的最佳實踐

:::info
**定義單元測試**
* 呼叫公開的方法回傳一個結果
* 呼叫方法前後, 系統可見的狀態或行為發生變化. 變化不須透過私有狀態就能取得
* 呼叫不受測試所控制的第三方系統
* ==單元測試的範圍可以小到一個方法, 大到多個類別.==
::::

### 小結:
* 寫優秀的測試
* 不好的測試讓大家不想使用, 會讓單元測試變得沒意義.
* [原則整理再另篇文章](https://hackmd.io/@GoldxTree/rJ-gzEdEF)
* [專案](https://github.com/KevinSintra/chapter1_simpleTest/commits/master)
------ 

### chapter2 第一個單元測試
- 好的命名
- 使用測試框架
- 了解一個工作單元的三種輸出類型

:::info
**測試方法命名**
* 開頭為測試方法的名稱
* 定義越清楚越好
* 若需測試系統狀態, 狀態改變前後須用不同的命名來協助辨識
:::

### 小結:
* 善用框架的特性, 來協助單元測試的簡潔與可讀性
* 保持簡單的單元測試
* 使用下列的結構以達到好的命名 =>```[unitOfWork]_[Scenario]_[ExpectedBehavior]```
* 使用簡單工廠達到重複使用程式, 例如初始化...等
* 盡量不要在單元測試中使用 setup、tearDown, 因為有可能會讓可讀性更差
* [專案](https://github.com/KevinSintra/unittestArt_chapter2/commits/master)

------

### chapter 3 透過虛設常式解決依賴問題
- 虛設常式(Stub)的定義
- 透過虛設常式來重構程式碼
- 克服程式碼中的封裝問題
- 探討使用虛設常式的最佳實踐

### 小結:
* 透過依賴反轉與依賴注入, 隔離依賴外部環境的邏輯進而達到可測試的目的.
* 依賴注入可透過 建構式注入、屬性注入、方法注入 三種，可依照情境下去設計
* 實現依賴反轉與依賴注入, ==會加深物件互動的層次而導致程式碼較難離解.== **但換來可隔離與可單元測試, 帶來的價值通常遠超過於被犧牲的.**
* 因被測試類別或者依賴==原本就有繼承介面或基底類別時==, 考量額外使用**依賴反轉 與 依賴注入**去繼承額外類別或介面可能會換來過高的複雜度, **可改直接繼承被測試類別, 搭配 extract、virtrul method、override 來達成可測試的目的.**
* [專案](https://github.com/KevinSintra/unittestArt_chapter3/commits/master)

------

### chapter 4 使用模擬物件驗證互動
- 定義 interaction testing(互動測試)
- 瞭解模擬物件
- 區分 fake(假物件)、mock(模擬物件)、stub(虛設常式)
- 模擬物件的最佳實踐

:::info
* **interaction testing**
  * 針對一個物件如何向其它物件互動的測試
  * 互動測試通常會增加測試的複雜度
  * 測試需透過**模擬物件**來驗證被測試物件是否如期與該物件互動

</br>

* **假物件:** 通常用來描述一個 **虛設常式** 或 **模擬物件**
* **虛設常式:** 測試最後是對 **被測試物件** 進行==驗證==
* **模擬物件:** 測試最後須對 **假物件** 進行==驗證==
* **單元測試情境為三種:** 回傳值驗證、屬性驗證、模擬物件互動驗證
:::

:::info
**單元測試要點**
* 一個單元測試只測試**一種行為**
* 一個單元測最多應該就只有**一個互動驗證(模擬物件)**
    ```若有多個互動驗證, 通常代表著不只在測試一種行為.```
* 一個單元測試應該只測試**一種情境**
    ```多種情境代表該測試可能複雜與脆弱, 導致調整程式後頻繁進而修改測試. ```
* 避免將物件鏈做成假物件(connStr = globalUtl.config.DBConfig.ConnStr), 如果不需對物件鏈去做驗證可將程式重構, 並使用虛設常式去覆寫該方法:
    ```python=20
    connStr = getConnectionString()
    def getConnectionString(): # 測試時使用 虛設常式 複寫該方法
        return globalUtl.config.DBConfig.ConnStr
    ```
:::

### 小結:
* 假物件可區分為**虛設常式物件**與**模擬物件**兩種類型
* 假物件**同時為**「虛設常式」與「模擬物件」的**情形較少見**
* **虛設常式**是用來**模擬情境**, 不會為測試執行失敗的主因. 通常是去滿足**被測試類別**需要的**回傳值**與**例外**
* **模擬物件**通常是用來**驗證互動**, 並**不是驗證回傳值**
* 單元測試**情境**包含: **回傳值驗證**(被測試目標)、**屬性驗證**(被測試目標)、**模擬物件互動驗證**(模擬物件會提供屬性協助測試驗證)
* **一個單元測試**應只測試**一種行為**
* **一個單元測試**應只測試**一種情境**
* 單元測試**過於複雜**時, 應思考是否為**測試標的太多** 或 **被測試目標方法過於複雜==是否應重構==**
* [專案](https://github.com/KevinSintra/unittestArt_chapter4/commits/master)
------

### chapter 5 隔離模擬框架
- 瞭解模擬框架
- 使用框架建立虛設常式和模擬物件
- 探討虛設常式和模擬物件的進階使用案例
- 模擬框架使用時常犯的錯誤

:::info
**使用隔離框架的缺點**
* 更容易建立假物件
* 更容易驗證參數
* 測試程式可讀性變差
    * 因建立假物件較輕鬆, 可能導致過多的建立. 而忘了去思考**測試目標是否太多**或**被測試方法是否需要重構**
* 一個測試中有多個模擬
    * 依循好的測試實踐原則, 一個測試一次只測一件事.
    * 一個測試中有兩個模擬物件, 代表測試中有很多結果需驗證
    * 若在命名測試時, 不知如何命名或方法名稱過長時. 應思考「**測試目標是否太多**或**被測試方法是否需要重構**」
* 過度的指定互動測試(模擬物件), 如何避免?
    * 盡量使用非嚴格的模擬物件(下章介紹): ==有助於被測試目標中私有方法時常變動的情境.==
    * 盡量使用虛設常式, 而非模擬物件.
    * 理解虛設常式與模擬物件的定義
:::

### 小結
* 測試盡量選擇**回傳值**或**系統狀態改變**的驗證
* 超過5%的測試方法使用模擬物件, 可能就代表過度指定了.
* 測試程式開始變得難懂, 意味著需要簡化事情
* [專案](https://github.com/KevinSintra/unittestArt_chapter5/commits/master)

------

### chapter 6

主要是在說明 .net 那些遺留的程式碼, 或者不可測試的程式碼. 可利用不受限框架去協助測試. 目的是為了重構以前沒有的測試的程式.

------

### chapter 7 測試階層和組織
- 在自動化的每日建置中執行單元測試(跳過)
- 使用持續整合來進行自動化測試(跳過)
- 在解決方案的組織測試
- 探討測試類別的繼承模式

:::info
**提示:bulb:**
* 通常一個類別對應一個測試類別
* 讀測試的人花在定位測試程式上的時間, 比理解測試程式還多的話. 就需要考慮別的組織方式.
* 將測試對應到明確工作單元入口
:::

:::info
**測試搭配物件導向:**
* 三種基本模式:
    * 抽象測試基礎結構類別
    * 測試類別模板
    * 抽象測試驅動類別
* 使用三種基本模式, 用到的重構技術:
    * 重構類別階層
    * 使用泛型
:::

## 為應用程式建立API

**==為應用程式建立API==主要陳述將物件導向的觀念, 融入單元測試中. (物件導向可參考[物件導向個人心得2](https://hackmd.io/@GoldxTree/ryxspYQWt)**

以下只呈現測試相關程式碼

### 1.抽象測試基礎結構類別模式 (檔名: test_ApiExample1.py)
#### 測試沒有遵循 DRY
```python=10
class TestAnalyzer:
    def test_analyzer_filenameTooShort_logCalled(self, mocker):
        logger = mocker.patch('test_ApiExample1.ILogger')  # 模擬物件
        mocker_method = mocker.patch.object(logger, 'log')
        LoggingFacility.set_logger(logger)

        analyzer = LogAnalyzer()
        analyzer.analyzer('a.txt')

        assert mocker_method.called == True

    def teardown_method(self):
        LoggingFacility.set_logger(None)


class TestConfigurationManager:
    def test_configured_logCalled(self, mocker):
        logger = mocker.patch('test_ApiExample1.ILogger') # 模擬物件
        mocker_method = mocker.patch.object(logger, 'log')
        LoggingFacility.set_logger(logger)

        manager = ConfigurationManager()
        manager.configured('api1.json')

        assert mocker_method.called == True
        
    def teardown_method(self):
        LoggingFacility.set_logger(None)
```
#### 重構後
```python=10
class BaseTestsClass:
    """基底類別供測試使用"""

    def fake_logger(self, mocker):
        """
        不是所有測試方法都會用到 LoggingFacility, 所以讓他們各自使用
        """
        logger = mocker.patch('test_ApiExample1.ILogger')  # 模擬物件
        return logger

    def teardown_method(self):
        LoggingFacility.set_logger(None)


class TestAnalyzer(BaseTestsClass):
    def test_analyzer_filenameTooShort_logCalled(self, mocker):
        logger = super().fake_logger(mocker)
        mocker_method = mocker.patch.object(logger, 'log')
        LoggingFacility.set_logger(logger)
        ...


class TestConfigurationManager(BaseTestsClass):
    def test_configured_logCalled(self, mocker):
        logger = super().fake_logger(mocker)
        mocker_method = mocker.patch.object(logger, 'log')
        LoggingFacility.set_logger(logger)
        ...
```

### 2.測試模板類別模式
- **簡易說明:** 
    - 該情境是去解析不同來源的文字內容, 透過抽象去規範各個不同來源對應的實作類, 應該實作的方法有哪些. 
    - 接下來就要去測試它囉, 以下的測試程式有用設計模式(大致上是基底類別去跑測試方法, 子類別只依規格去做設定)

#### 被測試程式碼的類別圖 =>
![](https://i.imgur.com/K9Zut3g.jpg?1)

#### 三個測試目標類別, 要有三個測試類別去執行. 礙於篇幅過長所以重構前只放一個測試類
``` python=5
class TestStandarStringParser:
    def test_getStringVersionFromHeader_singleDigit_found(self):
        input = 'header;version=1;\n'
        parser = StandardStringParser(input)
        version_name = parser.get_string_version_from_header()
        assert '1' == version_name

    def test_getStringVersionFromHeader_withMiorVersion_found(self):
        input = 'header;version=1.1;\n'
        parser = StandardStringParser(input)
        version_name = parser.get_string_version_from_header()
        assert '1.1' == version_name

    def test_getStringVersionFromHeader_withRevisionVersion_found(self):
        input = 'header;version=1.1.1;\n'
        parser = StandardStringParser(input)
        version_name = parser.get_string_version_from_header()
        assert '1.1.1' == version_name
```

#### 重構後 
    note:
    重構也可以用 pytest 參數化的方法達成, 就不用搞繼承關係了.
    (不知道繼承與參數化的方式, 哪種比較容易閱讀.)

``` python=5
class BaseStringParser(metaclass=abc.ABCMeta):
    def expected_singl_digit(self) -> str:
        return '1'

    def expected_with_minor(self) -> str:
        return '1.1'

    def expected_with_revision(self) -> str:
        return '1.1.1'

    @abc.abstractmethod
    def get_parser(input: str) -> IStringParser:
        pass

    @abc.abstractmethod
    def header_version_single_digit() -> str:
        pass

    @abc.abstractmethod
    def header_version_minor() -> str:
        pass

    @abc.abstractmethod
    def header_version_revision() -> str:
        pass

    def test_getStringVersionFromHeader_singleDigit_found(self):
        input = self.header_version_single_digit()
        parser = self.get_parser(input)
        version_name = parser.get_string_version_from_header()
        assert self.expected_singl_digit() == version_name

    def test_getStringVersionFromHeader_withMiorVersion_found(self):
        input = self.header_version_minor()
        parser = self.get_parser(input)
        version_name = parser.get_string_version_from_header()
        assert self.expected_with_minor() == version_name

    def test_getStringVersionFromHeader_withRevisionVersion_found(self):
        input = self.header_version_revision()
        parser = self.get_parser(input)
        version_name = parser.get_string_version_from_header()
        assert self.expected_with_revision() == version_name


class TestStandarStringParse(BaseStringParser):
    def get_parser(self, input: str) -> IStringParser:
        return StandarStringParse(input)

    def header_version_single_digit(self):
        return f'header;version={super().expected_singl_digit()};\n'

    def header_version_minor(self):
        return f'header;version={super().expected_with_minor()};\n'

    def header_version_revision(self):
        return f'header;version={super().expected_with_revision()};\n'
``` 

### 小結:
* 把共用的 測試API, 介紹給團隊. 如果不這樣做就會重複開發, 導致浪費時間成本
* 測試可以搭配物件導向
* 避免使用物件導向導致過於複雜, 導致測試的可讀性太差. 
* [專案](https://github.com/KevinSintra/unittestArt_chapter7/commits/master)
    * 抽象測試基礎結構類別模式
    * 測試模板類別模式

------