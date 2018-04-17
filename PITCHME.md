### go with pact

[@katsutomu](https://github.com/katsutomu)

---

### PACTはなにか
- Consumer Driven Contruct Testのimplements
- 消費者と提供者間の破壊的変更を検知する
- いいかんじにする

---


### 手順

1. deamon用のバイナリを用意する
2. pact用のテスト書く
3. テスト実行

---


### 要するに

これをこう

---

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
                "    }\n" +
                "  ]\n" +
                "}"
        )
        .toPact()
val config = MockProviderConfig.createDefault()
val result = runConsumerTest(pact, config, object : PactTestRun {
    override fun run(mockServer: MockServer) {

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

---

```
{
    "provider": {
        "name": "ToiletAPI"
    },
    "consumer": {
        "name": "AndroidApp"
    },
    "interactions": [
        {
            "description": "test lines",
            "request": {
                "method": "GET",
                "path": "/lines"
            },
            "response": {
                "status": 200,
                "body": {
                    "lines": [
                        {
                            "id": 1,
                            "name": "ginza",
                            "name_kana": "ginza",
                            "mark": "G",
                            "color_code": "f39700",
                            "sort_number": 0
                        },
                        {
                            "id": 2,
                            "name": "marunouchi",
                            "name_kana": "marunouchi",
                            "mark": "M",
                            "color_code": "e60012",
                            "sort_number": 0
                        }
                    ]
                }
            }
        },
        {
            "description": "test stopStations",
            "request": {
                "method": "GET",
                "path": "/lines/1/stop_stations"
            },
            "response": {
                "status": 200,
                "body": {
                    "stop_stations": [
                        
                    ]
                }
            }
        }
    ],
    "metadata": {
        "pact-specification": {
            "version": "2.0.0"
        },
        "pact-jvm": {
            "version": "3.5.0"
        }
    }
}
```

---

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
@[](サーバーを起動して)
@[11-17](pact deamonを介するしてテスト)
@[14](ここでConsumerが期待する内容が書かれたファイルを指定している)

---

### PactBroker
