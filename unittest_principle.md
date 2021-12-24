unittest principle
======

###### tags: `unittest` `入門`

### 原則

- Having unit tests without integration tests：只有單元測試，沒有整合測試

- Having integration tests without unit tests: 只有整合測試，沒有單元測試

- Having the wrong kind of tests: 測試的種類分佈不符合專案特性
    - 有個通則但不是絕對, 重點是將心力放在能為專案帶來更多價值的測試上
        - command line app: unit > integration
        - web app: UI > integration > unit
        - API: integration > unit

- Testing the wrong functionality: 花太多時間在測試非核心功能(思考是否要寫測試)

- Testing internal implementation: 測試綁定在功能的內部實作上(意思是針對行為去測試, 不是那些未公開的方法) 

- Paying excessive attention to test coverage: 過度追求測試覆蓋率

- PBCNT（Percent of Bugs that Create New tests）：有多少實際上遇過的bug 確實地轉成測試了？（anti-pattern 10）
- PTVB（Percent of Tests that Verify Behavior and not implementation）：有多少測試是針對行為而非內部實作？（anti-pattern 5）
- PTD（Percent of Tests that are Deterministic to total tests）：有多少測試，失敗時能真正代表程式碼出錯了？（anti-pattern 7）

- Having flaky or slow tests：測試結果不穩定或太耗時

- Running tests manually: 需要手動介入測試

- Not converting production bugs to tests: 上線時遇到的bug沒有納入測試

------

### 優秀單元測試的定義
* 很容易被實現
* 執行速度很快
* 執行結果是一致的
* 能完全掌控被測試的單元
* 測試失敗時能簡單清楚呈現期望為何? 發生的原因在哪?
* 團隊中的任何一個都可以執行, 並且得到相同結果