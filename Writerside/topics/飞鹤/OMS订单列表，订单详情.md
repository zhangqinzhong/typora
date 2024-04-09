# OMS订单列表

纪元：retailforce_oms

API：trade_list_order_at_page

纪元无复杂代码逻辑。直接调用oms接口返回数据。

oms接口Code Reference：com.alibaba.oms.portal.api.trade.OrderPortalService#listAtPage

调用中台接口获取数据：

com.alibaba.cz.trade.normal.vieworder.api.QueryOrderViewAbilityService#queryOrderListView



子订单价格：

originPrice：取自PriceViewDTO的originPrice

realMoney：shouldPayPrice / quantity （应付金额除以数量）

realPoint：AssetInfoViewDTO对象中的assetAmount / quantity（资产金额除以数量）（取的集合中assetType = POINT的第一个元素）



订单价格：

totalMoney：取自orderTotalPrice

freight：取自postFee

totalPoint：取自assetInfoViewDTOS集合的assetType = POINT的第一个元素的assetAmount字段。



# OMS订单详情

c0a8a96f16774863515807739d08c8



纪元：retailforce_oms

API：trade_query_order_detail

oms接口Code Reference：com.alibaba.oms.portal.api.trade.OrderPortalService#queryDetail

调用中台接口获取数据：

com.alibaba.cz.trade.normal.vieworder.api.QueryOrderViewAbilityService#queryOrderView





```json
{
    "businessFeatures": {},
    "ext": {},
    "header": {
        "date": 1677486351741,
        "rpcId": "0.2.2",
        "traceId": "c0a8a96f16774863515807739d08c8"
    },
    "platformFeatures": {},
    "result": {
        "actualPaidFee": 105000,
        "afterRefundStatus": 9,
        "bizCode": "com.feihe2.threemall",
        "buyerDeleteStatus": 0,
        "buyerId": "U114548832050766276",
        "buyerName": "天猫默认会员",
        "buyerRateStatus": 2,
        "channelCode": "002",
        "channelName": "自营天猫渠道",
        "deliveryStatus": 3,
        "deliveryTypeId": 1,
        "deliveryTypeName": "快递邮寄",
        "discountFee": 82000,
        "endTime": 1668133355000,
        "extStatus": 0,
        "features": {
            "useBbq": "false",
            "bc": "normal_trade_base_line",
            "thirdOrderInfo": "{\"payment\":\"78300\",\"paid_coupon_fee\":\"0\",\"expandcard_info\":[{\"basic_price_used\":\"0\",\"expand_price\":\"0\",\"basic_price\":\"0\",\"expand_price_used\":\"0\"}],\"discount_fee\":\"26700\",\"coupon_fee\":\"0\"}",
            "checkAndPay": "0",
            "dst": "2022-11-02 16:33:09",
            "encRecPhone": "*******5955",
            "payTime": "1666951421000",
            "platform": "0103",
            "tid": "1717229201603514863",
            "hold": "0",
            "activityTypeList": "",
            "payMethod": "ONLINE_PAYMENT",
            "cuanhuoStatus": "1",
            "ptos": "86400",
            "encRecAddress": "津*街道**路锦**园**号**单元***",
            "flowId": "212",
            "warehouseIds": "464",
            "oaid": "1D2d8x5nRZT9Uic7Vr1ZNQwiam3dOlaJNA7tYMia3lNxfhs8mq6QeF8KJKibrNF9yPwhLbIVXKF",
            "traceId": "c0a8a9df16672803603661853d0029",
            "stepTradesStatus": "1",
            "channelOrderId": "1717229201603514863",
            "sysVersion": "2",
            "cuanhuoReason": "应运营要求,该渠道订单默认疑似窜货",
            "orderSource": "tmall",
            "os": "APPLET",
            "flowInst": "1|theEnd|^",
            "encRecName": "蒙**",
            "ctod": "2022-11-11 10:21:44",
            "mcType": "5",
            "oId": "92C004C22C7D3E8C629C1B82DA8242C6",
            "ptod": "2022-10-29 18:03:35",
            "frontCashier": "0",
            "orderTags": "210",
            "limitSubject": "U114548832050766276",
            "msgIds": "C0A8A90B00291BFC632703461DFF7BEE,C0A8A9D700297FDAF75103461ED56FD2,C0A8A92B002933739D3F036AAA089A6A,C0A8A9EC002929F1D36C08B39FFA50E2,C0A8A92B002933739D3F1148860B432F,C0A8A99C00297FDAF7511182C4A85133,C0A8A9EC002929F1D36C11AC4C5A635A,C0A8A9EC002929F1D36C11AC4C5A6359,C0A8A9D700297FDAF75135B9960CF148,C0A8A9D700297FDAF75135B9960DF14A",
            "adjustShippingFee": "0",
            "delayTime": "3",
            "ctos": "604800",
            "checkOut": "0"
        },
        "merchantId": "300000",
        "merchantName": "飞鹤上海电商商家",
        "orderCreateTime": 1666951415000,
        "orderLineSDOS": [
            {
                "afterRefundStatus": 7,
                "buyerId": "U114548832050766276",
                "buyerName": "天猫默认会员",
                "buyerRateStatus": 2,
                "categoryId": "225770746704967477",
                "deliveryStatus": 3,
                "deliveryTime": 1667377990000,
                "features": {
                    "oiup": "160800",
                    "bc": "normal_trade_base_line",
                    "itemType": "0",
                    "thirdOrderLineInfo": "{\"divide_order_fee\":\"105000\",\"part_mjz_discount\":\"26700\",\"price\":\"105000\",\"adjust_fee\":\"0\",\"total_fee\":\"131700\",\"discount_fee\":\"-26700\",\"recharge_fee\":\"0\",\"platform_subsidy_fee\":\"0\"}",
                    "cobc": "com.feihe2.threemall",
                    "oltags": "normal",
                    "selfGoodsType": "1",
                    "costPrice": "160800",
                    "tagPrice": "160800",
                    "oId": "3341983dcd92fc954c819bdf35d522d7",
                    "spid": "200902",
                    "originLineId": "1717229201604514863",
                    "itemAdjustFee": "55800",
                    "newExclusive": "0",
                    "reqLineOutId": "1717229201603514863@1717229201604514863@3",
                    "subOrderSource": "tmall",
                    "outerSkuId": "229756453648225427",
                    "invStrategy": "1",
                    "combinedGoodsInfos": "[{\"deliveryQty\":6,\"shouldPayFee\":105000,\"selfGoodsType\":\"1\",\"scItemId\":\"303621\",\"tagPrice\":26800,\"taxRate\":\"13\",\"qty\":6,\"name\":\"星飞帆4段儿童奶粉听装700g\",\"outerId\":\"10061018\",\"id\":\"9300000341078426276-0\",\"otherShouldPayFee\":0,\"taxType\":\"103020402\",\"deliveryStatus\":\"30\",\"ratio\":100}]",
                    "isf": "1",
                    "isCombGoods": "1"
                },
                "itemId": "277542379171841015",
                "itemTitle": "【品类日】飞鹤星飞帆4段3-6岁儿童奶粉四段700g*6罐组",
                "merchantId": "300000",
                "merchantName": "飞鹤上海电商商家",
                "orderLineCreateTime": 1666951415000,
                "orderLineModifiedTime": 1668133355000,
                "otherSharedActualPaidFee": 0,
                "otherSharedDiscountFee": 0,
                "otherSharedShouldPayFee": 0,
                "otherStrikeThroughFee": 0,
                "otherUnitFee": 0,
                "outOrderLineId": "3341983dcd92fc954c819bdf35d522d7",
                "payStatus": 6,
                "payTime": 1666951421000,
                "quantity": 1,
                "receivedTime": 1668133355000,
                "refundStatus": 7,
                "sellerRateStatus": 2,
                "sharedActualPaidFee": 105000,
                "sharedDiscountFee": 55800,
                "sharedShouldPayFee": 105000,
                "skuId": "277542379171841016",
                "skuPictUrl": "https://media.feihe.com/fhmall/feiHeMall/materials/20210423/afd0ac9a7d7ec1adf1b9933a7b5f6976.jpg",
                "skuProperty": "规格描述:6罐组;",
                "strikeThroughFee": 105000,
                "syncVersion": 4,
                "tradeOrderId": "9300000341078416276",
                "tradeOrderLineId": "9300000341078426276",
                "unitFee": 160800
            },
            {
                "afterRefundStatus": 7,
                "buyerId": "U114548832050766276",
                "buyerName": "天猫默认会员",
                "buyerRateStatus": 2,
                "categoryId": "225770746704967477",
                "deliveryStatus": 3,
                "deliveryTime": 1667528504000,
                "features": {
                    "oiup": "9200",
                    "bc": "normal_trade_base_line",
                    "itemType": "0",
                    "thirdOrderLineInfo": "{\"divide_order_fee\":\"0\",\"part_mjz_discount\":\"0\",\"price\":\"9200\",\"adjust_fee\":\"0\",\"total_fee\":\"0\",\"discount_fee\":\"9200\",\"recharge_fee\":\"0\",\"platform_subsidy_fee\":\"0\"}",
                    "cobc": "com.feihe2.threemall",
                    "oltags": "normal",
                    "selfGoodsType": "1",
                    "costPrice": "9200",
                    "tagPrice": "9200",
                    "oId": "880202de2547d8f6285c3b28fb8d0869",
                    "spid": "200902",
                    "originLineId": "1717229201605514863",
                    "itemAdjustFee": "9200",
                    "newExclusive": "0",
                    "reqLineOutId": "1717229201603514863@1717229201605514863@3",
                    "subOrderSource": "tmall",
                    "outerSkuId": "372841596850717701",
                    "invStrategy": "1",
                    "combinedGoodsInfos": "[{\"deliveryQty\":8,\"shouldPayFee\":0,\"selfGoodsType\":\"1\",\"scItemId\":\"371519187635999917\",\"tagPrice\":0,\"taxRate\":\"13\",\"qty\":8,\"name\":\"星飞帆4段儿童奶粉条包25g\",\"outerId\":\"10065002\",\"id\":\"9300000341078436276-0\",\"otherShouldPayFee\":0,\"deliveryStatus\":\"30\",\"ratio\":100}]",
                    "isf": "1",
                    "isCombGoods": "1"
                },
                "itemId": "372932626390361836",
                "itemTitle": "【赠品】飞鹤星飞帆4段3-6岁儿童奶粉四段100g*2组",
                "merchantId": "300000",
                "merchantName": "飞鹤上海电商商家",
                "orderLineCreateTime": 1666951415000,
                "orderLineModifiedTime": 1668133355000,
                "otherSharedActualPaidFee": 0,
                "otherSharedDiscountFee": 0,
                "otherSharedShouldPayFee": 0,
                "otherStrikeThroughFee": 0,
                "otherUnitFee": 0,
                "outOrderLineId": "880202de2547d8f6285c3b28fb8d0869",
                "payStatus": 6,
                "payTime": 1666951421000,
                "quantity": 1,
                "receivedTime": 1668133355000,
                "refundStatus": 7,
                "sellerRateStatus": 2,
                "sharedActualPaidFee": 0,
                "sharedDiscountFee": 9200,
                "sharedShouldPayFee": 0,
                "skuId": "372932626390361837",
                "skuPictUrl": "https://media.feihe.com/fhmall/feiHeMall/materials/20220519/2c30cc06084c411f64501e5e758cfe7b.png",
                "skuProperty": "规格描述:8罐组;",
                "strikeThroughFee": 0,
                "syncVersion": 4,
                "tradeOrderId": "9300000341078416276",
                "tradeOrderLineId": "9300000341078436276",
                "unitFee": 9200
            },
            {
                "afterRefundStatus": 7,
                "buyerId": "U114548832050766276",
                "buyerName": "天猫默认会员",
                "buyerRateStatus": 2,
                "categoryId": "224773223415674138",
                "deliveryStatus": 3,
                "deliveryTime": 1667525783000,
                "features": {
                    "oiup": "12800",
                    "bc": "normal_trade_base_line",
                    "itemType": "0",
                    "thirdOrderLineInfo": "{\"divide_order_fee\":\"0\",\"part_mjz_discount\":\"0\",\"price\":\"12800\",\"adjust_fee\":\"0\",\"total_fee\":\"0\",\"discount_fee\":\"12800\",\"recharge_fee\":\"0\",\"platform_subsidy_fee\":\"0\"}",
                    "cobc": "com.feihe2.threemall",
                    "oltags": "normal",
                    "selfGoodsType": "1",
                    "costPrice": "12800",
                    "tagPrice": "12800",
                    "outerSkuIdType": "2",
                    "oId": "1973f3e21a56706defbfa558a67fcdd7",
                    "spid": "200902",
                    "originLineId": "1717229201606514863",
                    "taxRate": "13",
                    "itemAdjustFee": "12800",
                    "newExclusive": "0",
                    "reqLineOutId": "1717229201603514863@1717229201606514863@3",
                    "subOrderSource": "tmall",
                    "outerSkuId": "D106634",
                    "invStrategy": "1",
                    "materielName": "贝迪源城市购物小推车（059）",
                    "isf": "1"
                },
                "itemId": "419332145618187979",
                "itemTitle": "【赠品】贝迪源城市购物小推车（以实物为准，单独下单不发货）",
                "merchantId": "300000",
                "merchantName": "飞鹤上海电商商家",
                "orderLineCreateTime": 1666951415000,
                "orderLineModifiedTime": 1668133355000,
                "otherSharedActualPaidFee": 0,
                "otherSharedDiscountFee": 0,
                "otherSharedShouldPayFee": 0,
                "otherStrikeThroughFee": 0,
                "otherUnitFee": 0,
                "outOrderLineId": "1973f3e21a56706defbfa558a67fcdd7",
                "payStatus": 6,
                "payTime": 1666951421000,
                "quantity": 1,
                "receivedTime": 1668133355000,
                "refundStatus": 7,
                "sellerRateStatus": 2,
                "sharedActualPaidFee": 0,
                "sharedDiscountFee": 12800,
                "sharedShouldPayFee": 0,
                "skuId": "419332145618187980",
                "skuPictUrl": "https://media.feihe.com/fhmall/feiHeMall/materials/20220927/b70eba84af2597dc643122e8a553cf73.jpg",
                "skuProperty": "规格描述:单件;",
                "strikeThroughFee": 0,
                "syncVersion": 4,
                "tradeOrderId": "9300000341078416276",
                "tradeOrderLineId": "9300000341078446276",
                "unitFee": 12800
            },
            {
                "afterRefundStatus": 7,
                "buyerId": "U114548832050766276",
                "buyerName": "天猫默认会员",
                "buyerRateStatus": 2,
                "categoryId": "224773223415674138",
                "deliveryStatus": 3,
                "deliveryTime": 1667528504000,
                "features": {
                    "oiup": "4200",
                    "bc": "normal_trade_base_line",
                    "itemType": "0",
                    "thirdOrderLineInfo": "{\"divide_order_fee\":\"0\",\"part_mjz_discount\":\"0\",\"price\":\"4200\",\"adjust_fee\":\"0\",\"total_fee\":\"0\",\"discount_fee\":\"4200\",\"recharge_fee\":\"0\",\"platform_subsidy_fee\":\"0\"}",
                    "cobc": "com.feihe2.threemall",
                    "oltags": "normal",
                    "selfGoodsType": "1",
                    "costPrice": "4200",
                    "tagPrice": "4200",
                    "outerSkuIdType": "2",
                    "oId": "e736ae4e8960612df06ce382e0fbcd48",
                    "spid": "200902",
                    "originLineId": "1717229201607514863",
                    "taxRate": "13",
                    "itemAdjustFee": "4200",
                    "newExclusive": "0",
                    "reqLineOutId": "1717229201603514863@1717229201607514863@3",
                    "subOrderSource": "tmall",
                    "outerSkuId": "D106627",
                    "invStrategy": "1",
                    "materielName": "美乐童年超可洗水擦笔（044）",
                    "isf": "1"
                },
                "itemId": "419332237644011519",
                "itemTitle": "【赠品】美乐童年超可洗水擦笔（以实物为准，单独下单不发货）",
                "merchantId": "300000",
                "merchantName": "飞鹤上海电商商家",
                "orderLineCreateTime": 1666951415000,
                "orderLineModifiedTime": 1668133355000,
                "otherSharedActualPaidFee": 0,
                "otherSharedDiscountFee": 0,
                "otherSharedShouldPayFee": 0,
                "otherStrikeThroughFee": 0,
                "otherUnitFee": 0,
                "outOrderLineId": "e736ae4e8960612df06ce382e0fbcd48",
                "payStatus": 6,
                "payTime": 1666951421000,
                "quantity": 1,
                "receivedTime": 1668133355000,
                "refundStatus": 7,
                "sellerRateStatus": 2,
                "sharedActualPaidFee": 0,
                "sharedDiscountFee": 4200,
                "sharedShouldPayFee": 0,
                "skuId": "419332237644011520",
                "skuPictUrl": "https://media.feihe.com/fhmall/feiHeMall/materials/20220927/6aa71e008fb5494bff3b11c63c87c96a.png",
                "skuProperty": "规格描述:单件;",
                "strikeThroughFee": 0,
                "syncVersion": 4,
                "tradeOrderId": "9300000341078416276",
                "tradeOrderLineId": "9300000341078456276",
                "unitFee": 4200
            }
        ],
        "orderModifiedTime": 1668133355000,
        "otherActualPaidFee": 0,
        "otherDiscountFee": 0,
        "otherProductTotalFee": 0,
        "otherShouldPayFee": 0,
        "otherStrikeThroughFee": 0,
        "outOrderId": "92C004C22C7D3E8C629C1B82DA8242C6",
        "payStatus": 6,
        "payTime": 1666951421000,
        "productTotalFee": 187000,
        "queryIndex": "{\"channelOrderId\":\"1717229201603514863\",\"districtCode\":\"210104\",\"deliveryTime\":1667377990000,\"dst\":\"2022-11-02 16:33:09\",\"itemTitle\":\"【品类日】飞鹤星飞帆4段3-6岁儿童奶粉四段700g*6罐组,【赠品】飞鹤星飞帆4段3-6岁儿童奶粉四段100g*2组,【赠品】贝迪源城市购物小推车（以实物为准，单独下单不发货）,【赠品】美乐童年超可洗水擦笔（以实物为准，单独下单不发货）\",\"ctod\":\"2022-11-11 10:21:44\",\"mcType\":\"5\",\"hold\":\"0\",\"orderTags\":\"210\",\"itemId\":\"277542379171841015,372932626390361836,419332145618187979,419332237644011519\",\"outerSkuId\":\"229756453648225427,372841596850717701,D106634,D106627\",\"activityTypeList\":\"\",\"logisticsNo\":\"SF1678143341336,SF1695567672695,SF1372935889853\",\"cuanhuoStatus\":\"1\",\"checkOut\":\"0\",\"warehouseIds\":\"464\",\"tradeOrderLineId\":\"9300000341078426276,9300000341078436276,9300000341078446276,9300000341078456276\"}",
        "refundStatus": 9,
        "sellerDeleteStatus": 0,
        "sellerRateStatus": 2,
        "shippingFee": 0,
        "shouldPayFee": 105000,
        "status": 1,
        "storeId": "200902",
        "storeName": "天猫旗舰店",
        "strikeThroughFee": 105000,
        "syncVersion": 10,
        "tradeOrderId": "9300000341078416276"
    },
    "success": true,
    "validateErrors": {}
}
```

```json
{
    "businessFeatures": {},
    "ext": {},
    "header": {
        "date": 1677486351811,
        "rpcId": "0.2.3",
        "traceId": "c0a8a96f16774863515807739d08c8"
    },
    "platformFeatures": {},
    "result": {
        "buyerViewDTO": {
            "buyerId": "U114548832050766276",
            "buyerName": "天猫默认会员",
            "nickName": "User2e52d33b6e314c24909f1fe6e8e099fca"
        },
        "deliveryInfoViewDTO": {
            "city": "沈阳市",
            "cityCode": "210100",
            "deliveryTime": "2022-11-02 16:33:10",
            "deliveryType": 1,
            "detailAddress": "辽宁省沈阳市大东区津桥街道",
            "features": {},
            "postNum": "000000",
            "province": "辽宁省",
            "provinceCode": "210000",
            "street": "大东区",
            "streetCode": "210104",
            "town": "津桥街道",
            "townCode": "津桥街道"
        },
        "merchantInfoDTO": {
            "merchantId": "300000",
            "merchantName": "飞鹤上海电商商家",
            "shopName": "天猫旗舰店",
            "storeId": "200902"
        },
        "operationViewDTOS": [],
        "orderInfoDTO": {
            "bizCode": "com.feihe2.threemall",
            "bizFeatures": {
                "stepTradesStatus": "1",
                "channelOrderId": "1717229201603514863",
                "thirdOrderInfo": "{\"payment\":\"78300\",\"paid_coupon_fee\":\"0\",\"expandcard_info\":[{\"basic_price_used\":\"0\",\"expand_price\":\"0\",\"basic_price\":\"0\",\"expand_price_used\":\"0\"}],\"discount_fee\":\"26700\",\"coupon_fee\":\"0\"}",
                "customLimitSubject": "{\"buyerId\":\"U114548832050766276\"}",
                "orderSource": "tmall",
                "encRecPhone": "*******5955",
                "payTime": "1666951421000",
                "encRecName": "蒙**",
                "tid": "1717229201603514863",
                "osr": "{\"splitInfos\":[{\"split\":false,\"splitId\":\"92C004C22C7D3E8C629C1B82DA8242C6_0\",\"splitReason\":\"delivery\"}]}",
                "orderTags": "210",
                "activityTypeList": "",
                "whid": "[]",
                "encRecAddress": "津*街道**路锦**园**号**单元***",
                "oaid": "1D2d8x5nRZT9Uic7Vr1ZNQwiam3dOlaJNA7tYMia3lNxfhs8mq6QeF8KJKibrNF9yPwhLbIVXKF"
            },
            "channelCode": "002",
            "channelName": "自营天猫渠道",
            "createTime": 1666951415000,
            "dealTime": 1668133355000,
            "endTime": 1668133355000,
            "features": {
                "useBbq": "false",
                "bc": "normal_trade_base_line",
                "thirdOrderInfo": "{\"payment\":\"78300\",\"paid_coupon_fee\":\"0\",\"expandcard_info\":[{\"basic_price_used\":\"0\",\"expand_price\":\"0\",\"basic_price\":\"0\",\"expand_price_used\":\"0\"}],\"discount_fee\":\"26700\",\"coupon_fee\":\"0\"}",
                "checkAndPay": "0",
                "dst": "2022-11-02 16:33:09",
                "encRecPhone": "*******5955",
                "payTime": "1666951421000",
                "platform": "0103",
                "tid": "1717229201603514863",
                "hold": "0",
                "activityTypeList": "",
                "payMethod": "ONLINE_PAYMENT",
                "cuanhuoStatus": "1",
                "ptos": "86400",
                "encRecAddress": "津*街道**路锦**园**号**单元***",
                "flowId": "212",
                "warehouseIds": "464",
                "oaid": "1D2d8x5nRZT9Uic7Vr1ZNQwiam3dOlaJNA7tYMia3lNxfhs8mq6QeF8KJKibrNF9yPwhLbIVXKF",
                "traceId": "c0a8a9df16672803603661853d0029",
                "stepTradesStatus": "1",
                "channelOrderId": "1717229201603514863",
                "sysVersion": "2",
                "cuanhuoReason": "应运营要求,该渠道订单默认疑似窜货",
                "orderSource": "tmall",
                "os": "APPLET",
                "flowInst": "1|theEnd|^",
                "encRecName": "蒙**",
                "ctod": "2022-11-11 10:21:44",
                "weight": "0.0",
                "mcType": "5",
                "oId": "92C004C22C7D3E8C629C1B82DA8242C6",
                "ptod": "2022-10-29 18:03:35",
                "frontCashier": "0",
                "orderTags": "210",
                "limitSubject": "U114548832050766276",
                "msgIds": "C0A8A90B00291BFC632703461DFF7BEE,C0A8A9D700297FDAF75103461ED56FD2,C0A8A92B002933739D3F036AAA089A6A,C0A8A9EC002929F1D36C08B39FFA50E2,C0A8A92B002933739D3F1148860B432F,C0A8A99C00297FDAF7511182C4A85133,C0A8A9EC002929F1D36C11AC4C5A635A,C0A8A9EC002929F1D36C11AC4C5A6359,C0A8A9D700297FDAF75135B9960CF148,C0A8A9D700297FDAF75135B9960DF14A",
                "adjustShippingFee": "0",
                "delayTime": "3",
                "ctos": "604800",
                "checkOut": "0",
                "weightUnit": ""
            },
            "orderTags": "210",
            "payTime": 1666951421000,
            "totalCount": 4,
            "tradeOrderId": "9300000341078416276"
        },
        "orderStatusViewDTO": {
            "afterRefundStatus": 9,
            "buyerDeleteStatus": 0,
            "buyerRateStatus": 2,
            "deliveryStatus": 3,
            "extStatus": 0,
            "orderStatus": "SUCCESS",
            "orderStatusStr": "已完成",
            "payStatus": 6,
            "refundStatus": 9,
            "sellerDeleteStatus": 0,
            "sellerRateStatus": 2
        },
        "orderStepViewDTO": {
            "content": [
                {
                    "key": "买家下单",
                    "value": "2022-10-28 18:03:35"
                },
                {
                    "key": "买家付款",
                    "value": "2022-10-28 18:03:41"
                },
                {
                    "key": "发货",
                    "value": "2022-11-02 16:33:09"
                },
                {
                    "key": "收货",
                    "value": "2022-11-11 10:22:35"
                },
                {
                    "key": "交易成功",
                    "value": "2022-11-11 10:22:35"
                }
            ],
            "selected": "5"
        },
        "packageViewDTOS": [
            {
                "companyCode": "shunfeng",
                "features": {
                    "outSubDeliveryDetailId": "null",
                    "subDeliveryOrderId": "930000016506034",
                    "deliveryOrderId": "9300000341078416276"
                },
                "logisticNo": "SF1678143341336",
                "logisticsName": "顺丰",
                "packageId": "null",
                "status": 3,
                "tradeOrderLineViewDTOS": [
                    {
                        "afterRefundStatus": 7,
                        "bizFeatures": {
                            "sinvidl": "[]"
                        },
                        "deliveryStatus": 3,
                        "features": {
                            "oiup": "160800",
                            "bc": "normal_trade_base_line",
                            "itemType": "0",
                            "thirdOrderLineInfo": "{\"divide_order_fee\":\"105000\",\"part_mjz_discount\":\"26700\",\"price\":\"105000\",\"adjust_fee\":\"0\",\"total_fee\":\"131700\",\"discount_fee\":\"-26700\",\"recharge_fee\":\"0\",\"platform_subsidy_fee\":\"0\"}",
                            "cobc": "com.feihe2.threemall",
                            "oltags": "normal",
                            "selfGoodsType": "1",
                            "costPrice": "160800",
                            "tagPrice": "160800",
                            "oId": "3341983dcd92fc954c819bdf35d522d7",
                            "spid": "200902",
                            "originLineId": "1717229201604514863",
                            "itemAdjustFee": "55800",
                            "newExclusive": "0",
                            "reqLineOutId": "1717229201603514863@1717229201604514863@3",
                            "subOrderSource": "tmall",
                            "outerSkuId": "229756453648225427",
                            "invStrategy": "1",
                            "combinedGoodsInfos": "[{\"deliveryQty\":6,\"shouldPayFee\":105000,\"selfGoodsType\":\"1\",\"scItemId\":\"303621\",\"tagPrice\":26800,\"taxRate\":\"13\",\"qty\":6,\"name\":\"星飞帆4段儿童奶粉听装700g\",\"outerId\":\"10061018\",\"id\":\"9300000341078426276-0\",\"otherShouldPayFee\":0,\"taxType\":\"103020402\",\"deliveryStatus\":\"30\",\"ratio\":100}]",
                            "isf": "1",
                            "isCombGoods": "1"
                        },
                        "itemViewDTO": {
                            "features": {
                                "outerSkuId": "229756453648225427"
                            },
                            "itemId": "277542379171841015",
                            "itemName": "【品类日】飞鹤星飞帆4段3-6岁儿童奶粉四段700g*6罐组",
                            "itemTitle": "【品类日】飞鹤星飞帆4段3-6岁儿童奶粉四段700g*6罐组",
                            "itemUrl": "/detail/getDetail?itemId=277542379171841015",
                            "sku": "规格描述:6罐组;",
                            "skuId": "277542379171841016",
                            "skuPicUrl": "https://media.feihe.com/fhmall/feiHeMall/materials/20210423/afd0ac9a7d7ec1adf1b9933a7b5f6976.jpg"
                        },
                        "payStatus": 6,
                        "priceViewDTO": {
                            "actualFee": 105000,
                            "assetInfoViewDTOS": [
                                {
                                    "actualAmount": 0,
                                    "assetAmount": 0,
                                    "assetDeduct": 0,
                                    "assetType": "POINT",
                                    "originAmount": 0,
                                    "strikeAmount": 0
                                }
                            ],
                            "costPrice": 160800,
                            "originPrice": 160800,
                            "shouldPayPrice": 105000,
                            "unitPrice": 105000
                        },
                        "promotionDetails": [],
                        "quantity": 1,
                        "receivedTime": 1668133355000,
                        "refundStatus": 7,
                        "reverseViewDTO": {
                            "action": true,
                            "desc": "申请退款",
                            "status": 7,
                            "tradeOrderId": "9300000341078416276",
                            "tradeOrderLineId": "9300000341078426276"
                        },
                        "status": "SUCCESS",
                        "tradeOrderLineId": "9300000341078426276"
                    }
                ]
            },
            {
                "companyCode": "shunfeng",
                "features": {
                    "outSubDeliveryDetailId": "null",
                    "subDeliveryOrderId": "930000016506034",
                    "deliveryOrderId": "9300000341078416276"
                },
                "logisticNo": "SF1695567672695",
                "logisticsName": "顺丰",
                "packageId": "null",
                "status": 3,
                "tradeOrderLineViewDTOS": [
                    {
                        "afterRefundStatus": 7,
                        "bizFeatures": {
                            "sinvidl": "[]"
                        },
                        "deliveryStatus": 3,
                        "features": {
                            "oiup": "4200",
                            "bc": "normal_trade_base_line",
                            "itemType": "0",
                            "thirdOrderLineInfo": "{\"divide_order_fee\":\"0\",\"part_mjz_discount\":\"0\",\"price\":\"4200\",\"adjust_fee\":\"0\",\"total_fee\":\"0\",\"discount_fee\":\"4200\",\"recharge_fee\":\"0\",\"platform_subsidy_fee\":\"0\"}",
                            "cobc": "com.feihe2.threemall",
                            "oltags": "normal",
                            "selfGoodsType": "1",
                            "costPrice": "4200",
                            "tagPrice": "4200",
                            "outerSkuIdType": "2",
                            "oId": "e736ae4e8960612df06ce382e0fbcd48",
                            "spid": "200902",
                            "originLineId": "1717229201607514863",
                            "taxRate": "13",
                            "itemAdjustFee": "4200",
                            "newExclusive": "0",
                            "reqLineOutId": "1717229201603514863@1717229201607514863@3",
                            "subOrderSource": "tmall",
                            "outerSkuId": "D106627",
                            "invStrategy": "1",
                            "materielName": "美乐童年超可洗水擦笔（044）",
                            "isf": "1"
                        },
                        "itemViewDTO": {
                            "features": {
                                "outerSkuId": "D106627"
                            },
                            "itemId": "419332237644011519",
                            "itemName": "【赠品】美乐童年超可洗水擦笔（以实物为准，单独下单不发货）",
                            "itemTitle": "【赠品】美乐童年超可洗水擦笔（以实物为准，单独下单不发货）",
                            "itemUrl": "/detail/getDetail?itemId=419332237644011519",
                            "sku": "规格描述:单件;",
                            "skuId": "419332237644011520",
                            "skuPicUrl": "https://media.feihe.com/fhmall/feiHeMall/materials/20220927/6aa71e008fb5494bff3b11c63c87c96a.png"
                        },
                        "payStatus": 6,
                        "priceViewDTO": {
                            "actualFee": 0,
                            "assetInfoViewDTOS": [
                                {
                                    "actualAmount": 0,
                                    "assetAmount": 0,
                                    "assetDeduct": 0,
                                    "assetType": "POINT",
                                    "originAmount": 0,
                                    "strikeAmount": 0
                                }
                            ],
                            "costPrice": 4200,
                            "originPrice": 4200,
                            "shouldPayPrice": 0,
                            "unitPrice": 0
                        },
                        "promotionDetails": [],
                        "quantity": 1,
                        "receivedTime": 1668133355000,
                        "refundStatus": 7,
                        "reverseViewDTO": {
                            "action": true,
                            "desc": "申请退款",
                            "status": 7,
                            "tradeOrderId": "9300000341078416276",
                            "tradeOrderLineId": "9300000341078456276"
                        },
                        "status": "SUCCESS",
                        "tradeOrderLineId": "9300000341078456276"
                    },
                    {
                        "afterRefundStatus": 7,
                        "bizFeatures": {
                            "sinvidl": "[]"
                        },
                        "deliveryStatus": 3,
                        "features": {
                            "oiup": "9200",
                            "bc": "normal_trade_base_line",
                            "itemType": "0",
                            "thirdOrderLineInfo": "{\"divide_order_fee\":\"0\",\"part_mjz_discount\":\"0\",\"price\":\"9200\",\"adjust_fee\":\"0\",\"total_fee\":\"0\",\"discount_fee\":\"9200\",\"recharge_fee\":\"0\",\"platform_subsidy_fee\":\"0\"}",
                            "cobc": "com.feihe2.threemall",
                            "oltags": "normal",
                            "selfGoodsType": "1",
                            "costPrice": "9200",
                            "tagPrice": "9200",
                            "oId": "880202de2547d8f6285c3b28fb8d0869",
                            "spid": "200902",
                            "originLineId": "1717229201605514863",
                            "itemAdjustFee": "9200",
                            "newExclusive": "0",
                            "reqLineOutId": "1717229201603514863@1717229201605514863@3",
                            "subOrderSource": "tmall",
                            "outerSkuId": "372841596850717701",
                            "invStrategy": "1",
                            "combinedGoodsInfos": "[{\"deliveryQty\":8,\"shouldPayFee\":0,\"selfGoodsType\":\"1\",\"scItemId\":\"371519187635999917\",\"tagPrice\":0,\"taxRate\":\"13\",\"qty\":8,\"name\":\"星飞帆4段儿童奶粉条包25g\",\"outerId\":\"10065002\",\"id\":\"9300000341078436276-0\",\"otherShouldPayFee\":0,\"deliveryStatus\":\"30\",\"ratio\":100}]",
                            "isf": "1",
                            "isCombGoods": "1"
                        },
                        "itemViewDTO": {
                            "features": {
                                "outerSkuId": "372841596850717701"
                            },
                            "itemId": "372932626390361836",
                            "itemName": "【赠品】飞鹤星飞帆4段3-6岁儿童奶粉四段100g*2组",
                            "itemTitle": "【赠品】飞鹤星飞帆4段3-6岁儿童奶粉四段100g*2组",
                            "itemUrl": "/detail/getDetail?itemId=372932626390361836",
                            "sku": "规格描述:8罐组;",
                            "skuId": "372932626390361837",
                            "skuPicUrl": "https://media.feihe.com/fhmall/feiHeMall/materials/20220519/2c30cc06084c411f64501e5e758cfe7b.png"
                        },
                        "payStatus": 6,
                        "priceViewDTO": {
                            "actualFee": 0,
                            "assetInfoViewDTOS": [
                                {
                                    "actualAmount": 0,
                                    "assetAmount": 0,
                                    "assetDeduct": 0,
                                    "assetType": "POINT",
                                    "originAmount": 0,
                                    "strikeAmount": 0
                                }
                            ],
                            "costPrice": 9200,
                            "originPrice": 9200,
                            "shouldPayPrice": 0,
                            "unitPrice": 0
                        },
                        "promotionDetails": [],
                        "quantity": 1,
                        "receivedTime": 1668133355000,
                        "refundStatus": 7,
                        "reverseViewDTO": {
                            "action": true,
                            "desc": "申请退款",
                            "status": 7,
                            "tradeOrderId": "9300000341078416276",
                            "tradeOrderLineId": "9300000341078436276"
                        },
                        "status": "SUCCESS",
                        "tradeOrderLineId": "9300000341078436276"
                    }
                ]
            },
            {
                "companyCode": "shunfeng",
                "features": {
                    "outSubDeliveryDetailId": "null",
                    "subDeliveryOrderId": "930000016506034",
                    "deliveryOrderId": "9300000341078416276"
                },
                "logisticNo": "SF1372935889853",
                "logisticsName": "顺丰",
                "packageId": "null",
                "status": 3,
                "tradeOrderLineViewDTOS": [
                    {
                        "afterRefundStatus": 7,
                        "bizFeatures": {
                            "sinvidl": "[]"
                        },
                        "deliveryStatus": 3,
                        "features": {
                            "oiup": "12800",
                            "bc": "normal_trade_base_line",
                            "itemType": "0",
                            "thirdOrderLineInfo": "{\"divide_order_fee\":\"0\",\"part_mjz_discount\":\"0\",\"price\":\"12800\",\"adjust_fee\":\"0\",\"total_fee\":\"0\",\"discount_fee\":\"12800\",\"recharge_fee\":\"0\",\"platform_subsidy_fee\":\"0\"}",
                            "cobc": "com.feihe2.threemall",
                            "oltags": "normal",
                            "selfGoodsType": "1",
                            "costPrice": "12800",
                            "tagPrice": "12800",
                            "outerSkuIdType": "2",
                            "oId": "1973f3e21a56706defbfa558a67fcdd7",
                            "spid": "200902",
                            "originLineId": "1717229201606514863",
                            "taxRate": "13",
                            "itemAdjustFee": "12800",
                            "newExclusive": "0",
                            "reqLineOutId": "1717229201603514863@1717229201606514863@3",
                            "subOrderSource": "tmall",
                            "outerSkuId": "D106634",
                            "invStrategy": "1",
                            "materielName": "贝迪源城市购物小推车（059）",
                            "isf": "1"
                        },
                        "itemViewDTO": {
                            "features": {
                                "outerSkuId": "D106634"
                            },
                            "itemId": "419332145618187979",
                            "itemName": "【赠品】贝迪源城市购物小推车（以实物为准，单独下单不发货）",
                            "itemTitle": "【赠品】贝迪源城市购物小推车（以实物为准，单独下单不发货）",
                            "itemUrl": "/detail/getDetail?itemId=419332145618187979",
                            "sku": "规格描述:单件;",
                            "skuId": "419332145618187980",
                            "skuPicUrl": "https://media.feihe.com/fhmall/feiHeMall/materials/20220927/b70eba84af2597dc643122e8a553cf73.jpg"
                        },
                        "payStatus": 6,
                        "priceViewDTO": {
                            "actualFee": 0,
                            "assetInfoViewDTOS": [
                                {
                                    "actualAmount": 0,
                                    "assetAmount": 0,
                                    "assetDeduct": 0,
                                    "assetType": "POINT",
                                    "originAmount": 0,
                                    "strikeAmount": 0
                                }
                            ],
                            "costPrice": 12800,
                            "originPrice": 12800,
                            "shouldPayPrice": 0,
                            "unitPrice": 0
                        },
                        "promotionDetails": [],
                        "quantity": 1,
                        "receivedTime": 1668133355000,
                        "refundStatus": 7,
                        "reverseViewDTO": {
                            "action": true,
                            "desc": "申请退款",
                            "status": 7,
                            "tradeOrderId": "9300000341078416276",
                            "tradeOrderLineId": "9300000341078446276"
                        },
                        "status": "SUCCESS",
                        "tradeOrderLineId": "9300000341078446276"
                    }
                ]
            }
        ],
        "payInfoViewDTO": {
            "actualFee": 105000,
            "assetInfoViewDTOS": [
                {
                    "actualAmount": 0,
                    "assetAmount": 0,
                    "assetDeduct": 0,
                    "assetType": "POINT",
                    "originAmount": 0
                }
            ],
            "discountFee": 82000,
            "features": {},
            "itemTotalPrice": 105000,
            "orderTotalPrice": 105000,
            "originTotalPrice": 187000,
            "payType": 2,
            "postFee": 0,
            "promotionDetails": []
        },
        "relativeOrderViewDTOS": [],
        "remindViewDTO": {},
        "tradeOrderId": "9300000341078416276",
        "tradeOrderLineViewDTOS": [
            {
                "$ref": "$.result.packageViewDTOS[0].tradeOrderLineViewDTOS[0]"
            },
            {
                "$ref": "$.result.packageViewDTOS[1].tradeOrderLineViewDTOS[1]"
            },
            {
                "$ref": "$.result.packageViewDTOS[2].tradeOrderLineViewDTOS[0]"
            },
            {
                "$ref": "$.result.packageViewDTOS[1].tradeOrderLineViewDTOS[0]"
            }
        ],
        "unionOrderViewDTO": {},
        "valueWrapperDTOS": [
            {
                "key": "订单编号",
                "value": "9300000341078416276"
            },
            {
                "key": "创建时间",
                "value": "2022-10-28 18:03:35"
            }
        ]
    },
    "success": true,
    "validateErrors": {}
}
```



com.alibaba.oms.core.manager.util.FeatureUtil#calcTMallOrderDiscountFee

```json
{
    "oiup": "160800",
    "bc": "normal_trade_base_line",
    "itemType": "0",
    "thirdOrderLineInfo": "{\"divide_order_fee\":\"105000\",\"part_mjz_discount\":\"26700\",\"price\":\"105000\",\"adjust_fee\":\"0\",\"total_fee\":\"131700\",\"discount_fee\":\"-26700\",\"recharge_fee\":\"0\",\"platform_subsidy_fee\":\"0\"}",
    "cobc": "com.feihe2.threemall",
    "oltags": "normal",
    "selfGoodsType": "1",
    "costPrice": "160800",
    "tagPrice": "160800",
    "oId": "3341983dcd92fc954c819bdf35d522d7",
    "spid": "200902",
    "originLineId": "1717229201604514863",
    "itemAdjustFee": "55800",
    "newExclusive": "0",
    "reqLineOutId": "1717229201603514863@1717229201604514863@3",
    "subOrderSource": "tmall",
    "outerSkuId": "229756453648225427",
    "invStrategy": "1",
    "combinedGoodsInfos": "[{\"deliveryQty\":6,\"shouldPayFee\":105000,\"selfGoodsType\":\"1\",\"scItemId\":\"303621\",\"tagPrice\":26800,\"taxRate\":\"13\",\"qty\":6,\"name\":\"星飞帆4段儿童奶粉听装700g\",\"outerId\":\"10061018\",\"id\":\"9300000341078426276-0\",\"otherShouldPayFee\":0,\"taxType\":\"103020402\",\"deliveryStatus\":\"30\",\"ratio\":100}]",
    "isf": "1",
    "isCombGoods": "1"
}
```

```
tMallLinesCalcDiscountFee = 26200
tMallTotalFee = 131700
```



eaac10caf416776158125433946d51b3

368555100867434012



5040366017139819743











eac0a8a90316844816566537551d65c9

java.lang.ClassCastException


姜立春