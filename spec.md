FORMAT: 1A
HOST: http://api.olabala.com/

# Less'More API

- 沙箱环境地址: [http://api.olabala.com](http://api.olabala.com)
- 正式环境地址：待定
- Swagger: [http://api.olabala.com/swagger/index.html](http://api.olabala.com/swagger/index.html)

## 服务器响应码

- 200 Ok            服务器已成功处理了请求
- 400 BadRequest    客户端提交的数据错误
- 401 Unauthorized  请求要求身份验证
- 500 Server Error  服务器遇到错误，无法完成请求

## 常用参数说明

1. 国家编码请参照 [ISO-3166-1 Alpha 2 Country Code](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2)
    > 例如: `CN`, `US`

2. 币别请参照 [Currency code](https://en.wikipedia.org/wiki/ISO_4217)
    > 目前系统支持的币别是
    > `USD`, `CNY`, `EUR`, `GBP`, `CAD`, `JPY`, `KRW`, `AUD`, `HKD`, `MYR`, `TWD`

3. 时间格式 `yyyy-MM-ddTHH:mm:ss`
    > 例如: `2019-04-22T08:07:11` 默认时区是UTC

    > 如果要指定时区 比如中国(UTC+8): `2019-04-22T08:07:11+08:00`

4. 重量
    > 支持的重量单位: `G`, `KG`, `LB`, `OZ`

5. 长度
    > 支持的长度单位: `CM`, `IN`, `MM`

6. 测试账号
    > username=demo, password=Pass123$

## Token [/Tokens]

### 获取Token [POST]

+ Request (application/x-www-form-urlencoded)

        username=demo&password=Pass123%24

+ Response 200 (application/json)

        {
            "expires_in":2592000,
            "token_type":"Bearer",
            "access_token":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ0ZW5hbnQiOiIxMDAwMlx0LVx0MTAwMDFcdC1cdDEwMDAwXHQtXHRPUC0xMDAwMiAoQWRtaW4pXHReXHQxMDEsMTM1IiwibmJmIjoxNTU1OTI1MzQ3LCJleHAiOjE1NTg1MTczNDcsImlhdCI6MTU1NTkyNTM0NywiaXNzIjoibXlJc3N1ZXIiLCJhdWQiOiJ3ZWJzZXJ2aWNlIn0.pgGRW9v9gldOcLr8d16DYXFkdf3azTRT27oF_G9M8F8"
        }

## 包裹 [/Parcels]

### 创建包裹 [POST]

+ Request (application/json)
    + Headers

            Authorization: Bearer <Put Your Token Here>

    + Body

            {
                "serviceCode": "CN-US-Express",
                "parcels": [
                    {
                        "idCardNbr": "322321198912092341",
                        "orderNbr": "87352121663001",
                        "weight": {
                            "value": 700,
                            "unit": "G"
                        },
                        "dimension": {
                            "length": 50,
                            "width": 60,
                            "height": 40,
                            "unit": "CM"
                        },
                        "consignee": {
                            "name": "James",
                            "phone": "123-456-7890",
                            "email": null,
                            "company": null,
                            "street1": "12345 Business Oneway",
                            "street2": null,
                            "street3": null,
                            "district": null,
                            "city": "City Of Industry",
                            "province": "CA",
                            "postalCode": "91789",
                            "countryCode": "US"
                        },
                        "shipper": {
                            "name": "Tao jun",
                            "phone": "13810221111",
                            "email": null,
                            "company": null,
                            "street1": "RM 101, No. 123 Pingxing Rd",
                            "street2": null,
                            "street3": null,
                            "district": "JingAn District",
                            "city": "Shanghai",
                            "province": "Shanghai",
                            "postalCode": "200040",
                            "countryCode": "CN"
                        },
                        "lineInfos": [
                            {
                            "goodsInfo": {
                                "sku": "132453301",
                                "spec": "13Inch, TouchBar",
                                "name": "Macbook Pro",
                                "brand": "Apple",
                                "model": "MR942CH/A",
                                "hsCode": "165390"
                            },
                            "lineTotal": {
                                "value": 15000,
                                "unit": "CNY"
                            },
                            "quantity": 1,
                            "cmdyID": 0
                            },
                            {
                            "goodsInfo": {
                                "sku": null,
                                "spec": null,
                                "name": "女装 针织连衣裙(短袖)",
                                "brand": "Uniqlo/优衣库",
                                "model": "413744",
                                "hsCode": "225311"
                            },
                            "lineTotal": {
                                "value": 2000,
                                "unit": "CNY"
                            },
                            "quantity": 10,
                            "cmdyID": 0
                            }
                        ]
                        }
                    ]
            }

+ Response 200 (application/json)

        {
            "result": [
                {
                "orderNbr": "87352121663001",
                "trackingNbr": "9261342243522212341001",
                "errors": []
                }
            ],
            "message": "",
            "isSuccess": true,
            "utcStamp": "2019-04-22T11:36:07.8782103Z"
        }

### 获取面单 [GET /Parcels/Label/{trackingNbr}]

+ Parameters
    + trackingNbr (required, string, `890232412001`) ... Tracking Number (`trackingNbr`)

+ Request (application/json)

    + Headers

            Authorization: Bearer <Put Your Token Here>

+ Response 200 (application/json)

        {
            "result": {
                "trackingNbr": "9274800000000000842749",
                "labelUrl": "http://api.olabala.com/labels/FedEx_JFK_9274800000000000842749_FirstClass.pdf?token=9283712312834343002231"
            },
            "message": "",
            "isSuccess": true,
            "utcStamp": "2019-04-22T10:56:56.5523838Z"
        }

### 获取物流信息 [GET /Parcels/Tracking/{trackingNbr}]

+ Parameters
    + trackingNbr (required, string, `9274800000000000842749`) ... Tracking Number (`trackingNbr`)

+ Request (application/json)

    + Headers

            Authorization: Bearer <Put Your Token Here>

+ Response 200 (application/json)

        {
            "result": {
                "trackingNbr": "9274800000000000842749",
                "events": [
                    {
                        "id": 1234,
                        "place": "City of Industry, CA",
                        "stage": "Outgated",
                        "operator": "OP-107",
                        "operation": "CfmFlightOnboarded",
                        "operatedOn": "2019-04-22 18:59"
                    },
                    {
                        "id": 1234,
                        "place": "City of Industry, CA",
                        "stage": "Arrived",
                        "operator": "OP-102",
                        "operation": "CfmFlightArrived",
                        "operatedOn": "2019-04-22 19:59"
                    }
                ]
            },
            "message": "",
            "isSuccess": true,
            "utcStamp": "2019-04-22T10:59:58.0244672Z"
        }

## 重量 [/Measures]

### 更新重量 [PUT]

+ Request (application/json)

    + Headers

            Authorization: Bearer <Put Your Token Here>
    
    + Body
            [
                {
                    "trackingNbr": "13421345644001",
                    "weight": {
                        "value": 600,
                        "unit": "G"
                    },
                    "dimension": {
                        "length": 60,
                        "width": 70,
                        "height": 50,
                        "unit": "CM"
                    }
                },
                {
                    "trackingNbr": "13421345644002",
                    "weight": {
                        "value": 601,
                        "unit": "G"
                    },
                    "dimension": {
                        "length": 61,
                        "width": 71,
                        "height": 51,
                        "unit": "CM"
                    }
                }
            ]

+ Response 200 (application/json)

        {
            "result": [
                {
                    "trackingNbr": "13421345644001",
                    "newTrackingNbr": "983345637200000002",
                    "errors": []
                },
                {
                    "trackingNbr": "13421345644002",
                    "newTrackingNbr": "",
                    "errors": ["没找到包裹"]
                }
            ],
            "message": "",
            "isSuccess": true,
            "utcStamp": "2019-04-22T09:51:28.0773804Z"
        }

## 入网清单 [/LastMilerManifests]

### 推送入网 [POST]

+ Request (application/json)

    + Headers

            Authorization: Bearer <Put Your Token Here>

    + Body

            [
                {
                    "trackingNbr": "342187643001",
                    "weight": {
                        "value": 200,
                        "unit": "G"
                    }
                },
                {
                    "trackingNbr": "342187643002",
                    "weight": {
                        "value": 300,
                        "unit": "G"
                    }
                },
                {
                    "trackingNbr": "342187643003",
                    "weight": {
                        "value": 400,
                        "unit": "G"
                    }
                }
            ]

+ Response 200 (application/json)

        {
            "result": [
                {
                    "trackingNbr": "342187643001",
                    "errors": []
                },
                {
                    "trackingNbr": "342187643002",
                    "errors": []
                },
                {
                    "trackingNbr": "342187643003",
                    "errors": []
                }
            ],
            "message": "",
            "isSuccess": true,
            "utcStamp": "2019-04-22T09:59:19.129116Z"
        }

## 出库清单 [/SackManifests]

### 创建出库清单 [POST]

+ Request (application/json)

    + Headers

            Authorization: Bearer <Put Your Token Here>

    + Body
            [
                {
                    "sackNbr": "3421001",
                    "parcels": [
                    {
                        "trackingNbr": "129856001",
                        "weight": {
                        "value": 400,
                        "unit": "G"
                        }
                    },
                    {
                        "trackingNbr": "129856002",
                        "weight": {
                        "value": 401,
                        "unit": "G"
                        }
                    },
                    {
                        "trackingNbr": "129856003",
                        "weight": {
                        "value": 402,
                        "unit": "G"
                        }
                    }
                    ]
                },
                {
                    "sackNbr": "3421002",
                    "parcels": [
                    {
                        "trackingNbr": "229856001",
                        "weight": {
                        "value": 400,
                        "unit": "G"
                        }
                    },
                    {
                        "trackingNbr": "229856002",
                        "weight": {
                        "value": 401,
                        "unit": "G"
                        }
                    }
                    ]
                }
            ]

+ Response 200 (application/json)

        {
        "result": {
            "sackManifestNbr": "Sackmft165000071"
        },
        "message": "",
        "isSuccess": true,
        "utcStamp": "2019-04-22T10:31:24.297503Z"
        }

### 确认登机 [PUT /SackManifests/Flight/{sackManifestNbr}]

+ Parameters
    + sackManifestNbr (required, string, `763251001`) ... Sack Manifest Number (`sackManifestNbr`)

+ Request (application/json)
    + Headers

            Authorization: Bearer <Put Your Token Here>

    + Body
            {
                "poa": "LAX",
                "pod": "HKG",
                "mawbNbr": "3213401",
                "flightNbr": "f1231",
                "eta": "2019-04-25T17:39:49.5865488+08:00",
                "etd": "2019-04-23T17:39:49.58655+08:00",
                "boardedOn": "2019-04-22T17:39:49.5865214+08:00"
            }

+ Response 200 (application/json)

        {
            "result": {
                "sackManifestNbr": "111111111"
            },
            "message": "",
            "isSuccess": true,
            "utcStamp": "2019-04-22T10:42:34.5216559Z"
        }