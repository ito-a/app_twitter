### Flutterのテストやってみた
初学者が３日間だけFlutterテストの基本を学んでみた。

#### テストの種類
- ユニット
  - Classごとの単体テスト 
  - apiの疎通等もモックを使ってテストできる
- ウィジェット
  - 文字通りWidgetのテスト
  - マッチャーを使って特定の文字やボタンが描画されているかをテストする
- インテグレーション
  - 統合テスト 

#### テストファイルの命名
テストをしたいファイル名の後ろに’_test’ を付けて命名する必要があるらしい
```
  lib/
    user.dart
  test/
    user_test.dart
```
```
  lib/
    model/
       user.dart
  test/
   model/
    user_test.dart
```

#### テストの実行
```
下記コマンドで全てのテストを実行
  flutter test
  
特定のテストファイルを実行
  flutter test test/XXXXXX_test.dart
```
### ユニットテスト
<details>
<summary>超基本編</summary>

```
// テスト対象のClass
class Counter {
  int value = 0;
  void increment() => value++;
  void decrement() => value--;
}
```
```
// テストコード
import 'package:flutter_test/flutter_test.dart';
import 'package:sample_app_test/counter.dart';

void main() {
  group('Counter', () {
    test('最初は０から始まるか', () {
      expect(Counter().value, 0);
    });

    test('Counterクラスのincrementを実行したときのvalueは１か', () {
      final counter = Counter();

      counter.increment();

      expect(counter.value, 1);
    });

    test('Counterクラスのdecrementedを実行したときのvalueは-１か', () {
      final counter = Counter();

      counter.decrement();

      expect(counter.value, -1);
    });
  });
}
```
解説
- groupは複数のテストを書くときに囲む。１つのテストだけなら下記表記でも可
```
  test('Counterクラスのincrementを実行したときのvalueは１か', () {
    final counter = Counter();
    counter.increment();
    expect(counter.value, 1);
  });
```

</details>

<details>
<summary>非同期編(もうちょっと検証した方がいい)</summary>

```
@JsonSerializable(fieldRename: FieldRename.snake)
class User {
  User({
    this.userId,
    this.id,
    this.title,
  });

  factory User.fromJson(Map<String, dynamic> json) => _$UserFromJson(json);
  String? userId;
  int? id;
  String? title;

  Map<String, dynamic> toJson() => _$UserToJson(this);
}
```
```
@RestApi()
abstract class ApiClient {
  factory ApiClient(Dio dio, {required String baseUrl}) = _ApiClient;

  @GET('/albums/1')
  Future<User> getUsers();
}
```
```
abstract class TestRepository {
  Future<User> getUsers();
}

class TestImpl with TestRepository {
  TestImpl({
    required this.homeClient,
  });

  ApiClient homeClient;

  @override
  Future<User> getUsers() async {
    homeClient = ApiClient(AppDio.getInstance(), baseUrl: 'https://jsonplaceholder.typicode.com');
    return await homeClient.getUsers();
  }
}
```
```
// テストコード
class MockHomeClient extends Mock implements ApiClient {}

void main() {
  late TestImpl dataSource;

  final User tArticle = User.fromJson('article.json'.toFixture());
  final User tArticles = tArticle;

  late MockHomeClient homeClient;

  setUp(() async {
    registerFallbackValue(Uri());
    homeClient = MockHomeClient();
    dataSource = TestImpl(homeClient: homeClient);
  });

  group('get users', () {
    test(
      'should return Users when the response is 200 (success)',
      () async {
        when(
          () => homeClient.getUsers(),
        ).thenAnswer(
          (_) async => tArticles,
        );
        final response = await dataSource.getUsers();
        expect(response.title, 'quidem molestiae enim');
      },
    );
  });
}
```
解説
- テストライブラリとして有名なのはmockito。mocktailも登場してる(初心者にはmocktailがオススメとのこと)<br>

</details>

### ウィジェットテスト

<details>
<summary>超基本編</summary>

```
// テスト対象のWidget
void main() {
  runApp(const MyApp(
    title: 'Example',
  ));
}

class MyApp extends StatefulWidget {
  const MyApp({
    super.key,
    required this.title,
  });

  final String title;

  @override
  State<MyApp> createState() => _MyAppState();
}

class _MyAppState extends State<MyApp> {
...
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
    theme: ThemeData(
    primarySwatch: Colors.blue,
    ),
    home: Scaffold(
    appBar: AppBar(
    title: Text(widget.title),
  ),
...
```
```
// テストコード
import 'package:flutter_test/flutter_test.dart';
import 'package:sample_app_test/main.dart';

void main() {
testWidgets('ツールバーに「T」が表示される', (WidgetTester tester) async {
  await tester.pumpWidget(const MyApp(title: 'T'));
  final titleFinder = find.text('T');
  expect(titleFinder, findsOneWidget);
});
}
```
解説
- 今回はマッチャー(findsOneWidget)を利用してますが、色々あるみたいです。ちょっと調べた。
```
- findsOneWidget: ウィジェットが１つ見つかる
- findsNothing: ウィジェットが見つからない
- findsWidgets: 1つ以上のウィジェットが見つかった
- findsNWidgets: 特定の数のウィジェットが見つかった
- findsAtLeastNWidgets:少なくとも特定の数のウィジェットを見つけた
```
</details>

#### インテグレーションテスト
<details>
<summary>超基本編</summary>
テストの準備

1. ライブラリ導入
```
dev_dependencies:
  integration_test:
    sdk: flutter
```

2. テストファイルの作成
```
  integration_test/
    app_test.dart
```

3. テストコード
```
// FLutterのサンプルアプリをテストする
void main() {
  IntegrationTestWidgetsFlutterBinding.ensureInitialized();

  group('end-to-end test', () {
    testWidgets('tap on the floating action button, verify counter', (tester) async {
      app.main();
      await tester.pumpAndSettle();

      // Verify the counter starts at 0.
      expect(find.text('0'), findsOneWidget);

      // Finds the floating action button to tap on.
      final Finder fab = find.byTooltip('Increment');

      // Emulate a tap on the floating action button.
      await tester.tap(fab);

      // Trigger a frame.
      await tester.pumpAndSettle();

      // Verify the counter increments by 1.
      expect(find.text('1'), findsOneWidget);
    });
  });
}
```
エミュレータで実際に動かしているようなテストができる。</br>
ただし、３つのテストの中で最も実行速度が遅い。
</details>