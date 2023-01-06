#### Androidのテスト

AndroidのKotorのテストを書いてみる。

### セットアップ
```
// build.gradle
testImplementation 'junit:junit:4.12'
```
シンプルな単体テストだけ行うなら必須はこれだけ

### テストの実行
AndroidStudioの場合は、テストファイル(ExampleUnitTes)を開いて</br>
TestClass横の ▷ を押すと実行
```
  src/
    test/
      java/
        net.da_vinsi_studio.kotor/ 
          ExampleUnitTest
```
コマンド実行も可能
```
./gradlew test
```
テスト実行後はレポートが吐き出されるようだ
```
  app/
    build/
      reports/
        tests/ 
          testDevelopDebugUnitTest
          testDevelopReleaseUnitTest
          ...
```
