### go with pact

[@katsutomu](https://github.com/katsutomu)

---

### 手順

1. deamon用のバイナリを用意する
2. pact用のテスト書く
3. テスト実行

### Consumerテストコード
```
val pact = ConsumerPactBuilder.consumer("AndroidApp")
        .hasPactWith("ToiletAPI")
        .uponReceiving("FirstTest")
        .path("/lines")
        .method("GET")
        .willRespondWith()
        .status(200)
        .body(        "{\n" +
                "  \"lines\": [\n" +
                "    {\n" +
                "      \"id\": 1,\n" +
                "      \"name\": \"銀座\",\n" +
                "      \"name_kana\": \"ぎんざ\",\n" +
                "      \"mark\": \"G\",\n" +
                "      \"color_code\": \"f39700\",\n" +
                "    }\n" +
                "  ]\n" +
                "}"
        )
        .toPact()
val config = MockProviderConfig.createDefault()
val result = runConsumerTest(pact, config, object : PactTestRun {
    override fun run(mockServer: MockServer) {
        val expectedResponse = HashMap<String, Any>()
        val lines = ArrayList<HashMap<String,Any>>()
        val line = HashMap<String, Any>()
        line.put("id", 1)
        line.put("name", "銀座")
        line.put("name_kana", "ぎんざ")
        line.put("mark", "G")
        line.put("color_code", "f39700")
        lines.add(line)
        expectedResponse.put("lines", lines)
        Assert.assertEquals(expectedResponse,
                ConsumerClient(mockServer.getUrl()).getAsMap("/lines"))
    }
})
Assert.assertEquals(PactVerificationResult.Ok, result);
```
@[1](サーバーを起動して)
@[2](pact deamonを介するしてテスト)
@[3](ここでConsumerが期待する内容が書かれたファイルを指定している)
@[4](ここでConsumerが期待する内容が書かれたファイルを指定している)
@[5](ここでConsumerが期待する内容が書かれたファイルを指定している)
@[6](ここでConsumerが期待する内容が書かれたファイルを指定している)
@[7](ここでConsumerが期待する内容が書かれたファイルを指定している)
@[8-18](ここでConsumerが期待する内容が書かれたファイルを指定している)

### Providerテストコード
```
func TestProvider(t *testing.T) {

  // Create Pact connecting to local Daemon
  pact := &dsl.Pact{
    Port:     6666, // Ensure this port matches the daemon port!
    Consumer: "MyConsumer",
    Provider: "MyProvider",
  }

  // Start provider API in the background
  go startServer()

  // Verify the Provider with local Pact Files
  pact.VerifyProvider(t, types.VerifyRequest{
    ProviderBaseURL:        "http://localhost:8000",
    PactURLs:               []string{filepath.ToSlash(fmt.Sprintf("%s/myconsumer-myprovider.json", pactDir))},
    ProviderStatesSetupURL: "http://localhost:8000/setup",
  })
```
@[9](サーバーを起動して)
@[11-17](pact deamonを介するしてテスト)
@[14](ここでConsumerが期待する内容が書かれたファイルを指定している)


### PactBroker
