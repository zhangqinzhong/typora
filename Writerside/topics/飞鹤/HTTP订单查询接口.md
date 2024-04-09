com.alibaba.rainbow.core.manager.rpc.order.OrderAbilityManager#setMerchantId

微信群聊沟通，确认rest接口请求方式

接口MSE路由名称：foreign-order-select-page

```yaml
	# 星妈会鉴权
- name: rainbow-foreign-order-selectpage
    # token:eyJhbGciOiJSUzI1NiIsImtpZCI6IjRhNzY1MjBhLTM4ZGUtNDBjYS04ZDBmLWQ1M2U2NTEwMDY3MyJ9.eyJqdGkiOiJQTFVlVGJaWjNUQTZ6MnRXUFJoQU5RIiwiaWF0IjoxNjc4MDg3MjQ0LCJleHAiOjE5OTM0NDcyNDQsIm5iZiI6MTY3ODA4NzE4NCwic3ViIjoiaHR0cDovL2FsaWJhYmFjbG91ZC5jb20iLCJhdWQiOiIiLCJpc3MiOiJodHRwOi8vYWxpYmFiYWNsb3VkLmNvbSIsInVzZXJJZCI6IjEyMTMyMzQiLCJlbWFpbCI6InVzZXJFbWFpbEB5b3VhcHAuY29tIn0.ftgu4UBCmiYPuaVkzTqDWblVbh7Cu_Vtb7lnJ2B0xwZtK6BOR8iFDqVyZ7PzEnylOQlxMfQnnap0eOKZBqAPKU8ks1Go97kNgnbq0ICgVe_QlOEsirvPE99NUcMCXwP44ZjGaasHkZPDt7OyWJXYq67hQQQlTs-9pscTPkJQsCqfR7xCVfMrb_o6goJJBNLU8RYmPnCHvXWv-zfibMQKkC-UjDWjW4w30p6xH85QowsDSlPhDwdb1QjCCeQ1Vj0qx6XZozdXNogghZAK7_jAVefIElhQwlhGvT_tasv4hEGQ4oG8kHiPIOOoBY6yko_OktmA4IH4UnyyfMe5oql0LA
    issuer: http://alibabacloud.com
    jwks: |
      {
        "keys": [
          {
            "kty": "RSA",
            "kid": "4a76520a-38de-40ca-8d0f-d53e65100673",
            "e": "AQAB",
            "alg": "RS256",
            "n": "mtzEcPODe65Eba8otEVT731J-82k6xIObSwerBDYrtIFiSrXLZ2hK_q0K2jd0qOwCsxYNwpJ2KtO_h01lAkjZ25hjVAnwagSw7sKhBeN-21lpoDaBiDYMcE6mFTJJF7iB77fgYN0rHLy8tvF1ltAc9uaINsUQxdhUal_SZBo8NOqISg7lgy-2bExBHIUKg9whSaqNU37bkAqjuZKxUVRD1-6xg9b5wEbvBAACZLivHAWsMXVRkQF0cteRmnR9HN1sMaTyzvEbo1YE-Q0rUS2S5KUMDer2-uZ0XdTp3EHA7Cd8nx_MG0aHl9s_9DzGKNUc2n1GgPJbHyTS48EuzOaow",
            "use": "sig"
          }
        ]
      }
    from_headers:
      - name: "jwt-assertion"
        value_prefix: ""
```

配置方式为1个路由2个鉴权。



```yaml


# 订单查询接口 一个路由 2个鉴权
  - _match_route_:
      - foreign-order-select-page
    allow:
      - rainbow-foreign-order-selectpage
      - c-87729573-6ec9-4d2c-9d99-45371d11dca0
```

