# amazon Dynamo DB

## 主要AWS CLIコマンド

- 1項目insert

    ```
    ~ aws dynamodb put-item --table-name Users --item '{
        "id": {"S": "1"},
        "name": {"S": "Yamada Tarou"}
    }'
    ```
- 1項目取得

    ```
    ~ aws dynamodb get-item --table-name Users --key '{
        "id": {"S": "1"},
        "name": {"S": "Yamada Tarou"}
    }'
    {
        "Item": {
            "id": {
                "S": "1"
            },
            "name": {
                "S": "Yamada Tarou"
            }
        }
    }
    ```
- 1項目の中から特定のカラムのみ抽出

    ```
    ~ aws dynamodb get-item --table-name Users --key '{
        "id": {"S": "1"},
        "name": {"S": "Yamada Tarou"}
    }'
    --projection-expression "#n" --expression-attribute-names '{"#n": "name"}'
    {
        "Item": {
            "name": {
                "S": "Yamada Tarou"
            }
        }
    }
    ```
- table全項目抽出

    ```
    ~ aws dynamodb scan --table-name Users
    {
        "Items": [
            {
                "id": {
                    "S": "1"
                },
                "name": {
                    "S": "Yamada Tarou"
                }
            }
        ],
        "Count": 1,
        "ScannedCount": 1,
        "ConsumedCapacity": null
    }
    ```
