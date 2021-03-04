## LAB1 : Creating an Amazon Aurora RDS Database (MySQL Compatible)

![img](aws-databases.assets/uzKYppAzg3MJX-OZBVX7jn_V_iNkN-DSmBb2fZrf8dhV4aDYBtJRDx1ePOzNdHTigWYgMmfaQYfMguXL7YbVc5-mIE_7kNWSHDHO8TXv_CMqayaWEOkNwVMzYd1i5bwNF0mWlDzm)

- RDS 데이터베이스 생성

![img](aws-databases.assets/CQFdRlpwMRLNNhFHwweQl0NNmPLJsv6Tukp6RnNW3J9aJhdnQJRK4dlyeCQrhAIgxZejRaDp4cz825wfiqZtDQH-1mW9W0nCh9zXtVPI_cUXYRHZUR4eMYlY3T9zrgr4WnSZXPcF)

![img](aws-databases.assets/F6kvwH3GwxM1OFTtbZsRqRjFotZl4gBcIQRS6IkXUEB3Rdi3FyrSEWW9eYTZXy77RPvVPIHd5Jq7pD1i3O9TLDY3JITaqGgB2lZMdF9qM4MKmpHhLGAyNvJ-4DQLKOZj0ko_ysgK)

- MYSQL 연결

![img](aws-databases.assets/2ShxAcBADy1WQLWeTPIv3lm-vJgOr_O4EsBvlbMw5eBfXK5ceC4ExROE6upgQbI5sMCfqVi7-Yn6QMaPWMtxlEMTpB_BT6Bkfn5Y_2Vk_vXbtsEwPT7PenqKu8JhG7twVMzhvXHi)

- SSH Hostname
  - Public Subnet에 위치한 EC2 인스턴스의 퍼블릭 IP 
- SSH Key File
  - EC2 인스턴스 생성 시 발행된 개인키 파일
- MySQL Hostname
  - 쓰기 유형의 DB 엔드포인트

- MYSQL 연결 화면

![img](aws-databases.assets/si4JVjwMnw5PJdUAbqAMhxpwIkU4_VgHOL2yPTEyKuwiBSrKsjjj0GCGmHhaBvfQgh_ALSjcsRP76CCcwuolJujghzqifwPsLTI9In-D5bkNBX39VVWdn4MIgfrEfonJhEw8v7Cg)

- dbeaver을 통한 연결
  - download link : https://dbeaver.io/download/

![img](aws-databases.assets/33GXSrH-w23K_9eaM9PAnxS3gu0BWlWWCBKoqamvz91Ea1jxGn04yS5-w4GhWqR8r-KmDJ_arx57Z7AwV9_Xee3UcCzwP7yU7usRpKvj5CpWmj__24Xgzvm_RFJlVmuE3VnejrKx)

![img](aws-databases.assets/54fGND8sWTolsK9jI4iY2LortWGqjb0RxDPlmh9zfvFDYvSYLo2elxbP4CLpxRxCW2xbysEilK4EkYXjpjll2au910Xqo7cRh7yjBw7Q6Vs7wfG8lggn76sKZm5rx68KGGYklHgL)

![img](aws-databases.assets/alxz-FkRA5ADmRalXUZivzUG-U5SWTjukg3XYZVVeftf7ROxtIvlR1br_1A1zH3lnqbI6yRXBTjlyNjYZlBd_3ba5RYGyOF4IY_gOPWCzG_I00Mmv7Q6ptbap5uAiXD_PWc6aFMR)

<br/>

---

<br/>

## LAB2 : AWS DynamoDB in the Console - Creating Tables, Items, and Indexes

- <a href="https://learn.acloud.guru/handson/98627abc-6348-4ac3-8c66-e03e76dfbf5a/course/178db59b-70f1-4bd8-8d74-9ab9263f8f9a">참고 링크</a>

### DynamoDB

- 완벽하게 관리되는 NoSQL 데이터베이스 서비스
  - 운영에 오버헤드가 없어짐
- 배포가 단순하고 신속
- 확장이 단순하고 신속
- 데이터를 자동으로 백업
  - 데이터의 손실을 막기 위해서
- 빠르고 일관된 응답 시간을 제공
- 보조 인덱스를 통한 빠른 조회
- 사용한 만큼 지불

### 핵심 구성요소 (Core Components)
- table
- items(항목)
- attribute(속성)

![img](aws-databases.assets/DLPAvyWQqtsLPmC5CscUHRpLC-DdbZ1Uv4U0ZuZ4pumpwknk-M3VEwnF6SwkI7m_A2S0s6X_3kmyN4VILYCgiwsqSIA5IY9DjBT7MngVCEle2mqKnWKaCQGbQoVmAeWPf2Gyt4eN)

- 예) Person 테이블
  - PersonID가 102인 항목은 Address 속성으로 끝나고, PersonID가 103인 항목은 FavoriteColor 속성으로 끝남
  - 기본키(Primary Key)를 제외하고는 스키마가 없다
  - 대부분의 속성은 스칼라(scalar)로 문자열 또는 숫자 같이 하나의 값만 가질 수 있으며, Address 처럼 내포 속성을 가질 수 있음 (DynamoDB는 32depth 까지만 지원)
- `기본키(Primary Key)`
  - 테이블의 각 항목을 나타내는 고유한 식별자
    - 동일한 키를 가질 수 없음 (Unique)
  - `단순 기본 키`
    - 파티션 키 하나로 구성
    - 일치 (equal) 방식의 검색만 가능
  - `복합 기본 키`
    - 파티션 키와 정렬 키로 구성
    - 일치, 부등호, 포함 등의 범위를 지정한 검색을 지원

![img](aws-databases.assets/oFtZnmA1au6ZoBhn_nJJStRpcs9PM5teQgvIfUaRygNIQFah5_LGo8ZX1e0vLoPV_u5S8ppsqv6ZYwai8HvoCIT0b5mudcxIPAuOmkDnaifU-vPgY7-5v9I20IfnSqwGzMWK1N6E)

- 첫번째 항목과 두번째 항목의 파티션 키(Artist)가 동일하므로 항목을 구분할 수 있도록 SongTitle이라는 정렬 키를 추가

- 보조 인덱스
  - 테이블에서는 하나 이상의 보조 인덱스를 생성할 수 있음
  - 보조 인덱스를 사용하면 기본 키에 대한 쿼리 외에 대체 키를 사용해서 테이블의 데이터를 쿼리할 수 있음
  - DynamoDB에서는 각 테이블에 20개의 Global Secondary Index와 5개의 Local Secondary Index를 제공
- Global Secondary Index
  - 파티션 키 및 정렬 키가 테이블의 파티션 키 및 정렬 키와 다를 수 있는 인덱스
- Local Secondary Index
  - 테이블과 파티션 키는 동일하지만 정렬 키는 다른 인덱스

---

<br/>

<br/>

## Python을 사용하여 DynamoDB 테이블 쿼리 및 관리

- 링크 = https://aws.amazon.com/ko/getting-started/hands-on/create-nosql-table/

![img](aws-databases.assets/VWM0xMKpcYmoXshUONARbfMu8_bcyb1wXkulIA7CkSe1xxBBpEYO0hkuP1X6MArgKWBW9JHbhanLDn9FjGc6nCdvGs7oE6geYxYLSQNpniAVybvTQ0X_xsF2rf1OapbPOzAJ8K_u)



#### #1 PC에서 PowerShell 실행 후 AWS CLI 설치 확인

```
PS C:\dynamodb> aws --version
aws-cli/2.1.27 Python/3.7.9 Windows/10 exe/AMD64 prompt/off
```

- **설치되어 있지 않은 경우 아래 URL에서 다운로드 후 설치**
  - https://docs.aws.amazon.com/ko_kr/cli/latest/userguide/install-cliv2-windows.html

![img](aws-databases.assets/121oyqCeI9a5B3rRzr9usnk5GJutUi19EAtPCr_LbobYCJlnLa4hYElr8_-45mKaajf8UDm2tG9nH5xoHYA-27WcQDojEU6AzawFsFhd9GSSI9IiBTH7pimhfW5udSfF-o-ezwpK)



#### #2 boto3 설치

```
PS C:\dynamodb> pip install boto3
```



#### #3 AWS DynamoDB in the Console - Creating Tables, Items, and Indexes 랩 시작



#### #4 IAM에서 cloud_user 사용자의 액세스 키 생성

![img](aws-databases.assets/SrNdlOTe6VOFnvD8srylHPnFFAGJg4KpH_vh68fNCgNAotvEkQ0Mkt1-evmWBSWLy2cfx-RW5zAH-SEjb32RFFYL4BkJL-e-zW10jGUGQMSxetcZZCbeTGG9KVf1e2pHnPQ0JZmy)

![img](aws-databases.assets/UDM9eJwu0gcfD4oLrY6ASRvxzwijEemKjrtgM9SjS0E6TrUYWDEhbwGi10hy01bNojVNuqLKCKdczeMQ7x4fOee7MecAprWWBDCYxbC8_OjrYz-T28rUXnWv36vUurc1saHkh76f)

- 생성한 액세스 키는 csv 파일 다운로드를 통해 보관



#### #5 aws configure 명령어로 액세스 키와 리전 정보를 설정

```
PS C:\dynamodb> aws configure
AWS Access Key ID [****************C3U3]: AKIAWUML4UQXD4EO3EGR
AWS Secret Access Key [****************FAoZ]: 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
Default region name [us-east-1]: us-east-1
Default output format [None]:
```



#### #6 DynamoDB 테이블 생성

- `c:\dynamodb\create_table.py`

```python
import boto3

# boto3 is the AWS SDK library for Python.
# We can use the low-level client to make API calls to DynamoDB.
client = boto3.client('dynamodb', region_name='us-east-1')

try:
    resp = client.create_table(
        TableName="Books",
        # Declare your Primary Key in the KeySchema argument
        KeySchema=[
            {
                "AttributeName": "Author",
                "KeyType": "HASH"
            },
            {
                "AttributeName": "Title",
                "KeyType": "RANGE"
            }
        ],
        # Any attributes used in KeySchema or Indexes must be declared in AttributeDefinitions
        AttributeDefinitions=[
            {
                "AttributeName": "Author",
                "AttributeType": "S"
            },
            {
                "AttributeName": "Title",
                "AttributeType": "S"
            }
        ],
        # ProvisionedThroughput controls the amount of data you can read or write to DynamoDB per second.
        # You can control read and write capacity independently.
        ProvisionedThroughput={
            "ReadCapacityUnits": 1,
            "WriteCapacityUnits": 1
        }
    )
    print("Table created successfully!")
except Exception as e:
    print("Error creating table:")
    print(e)

```

```
PS C:\dynamodb> python .\create_table.py
Table created successfully!
```

```
PS C:\dynamodb> aws dynamodb list-tables
{
    "TableNames": [
        "Books",		⇐ 테이블이 생성된 것을 확인
        "MusicExample"
    ]
}
```

![img](aws-databases.assets/Nh5rtzhtUScMGwPkwI-UJ4OKcgX5vd1w9aDuDg0LEq4aiEDdW46TqLnTzhfaQIwW8BwQ-juD13C82BSVy5xrVMQt5v-njB78ouDsiPGFKR4sX2QPEPxqKtbRLShNQxvKRaGiR3IV)



#### #7 Books 테이블에 항목을 로드

- `c:\dynamodb\insert_items.py`

```python
import boto3

# boto3 is the AWS SDK library for Python.
# The "resources" interface allow for a higher-level abstraction than the low-level client interface.
# More details here: http://boto3.readthedocs.io/en/latest/guide/resources.html
dynamodb = boto3.resource('dynamodb', region_name='us-east-1')
table = dynamodb.Table('Books')


# The BatchWriteItem API allows us to write multiple items to a table in one request.
with table.batch_writer() as batch:
    batch.put_item(Item={"Author": "John Grisham", "Title": "The Rainmaker",
        "Category": "Suspense", "Formats": { "Hardcover": "J4SUKVGU", "Paperback": "D7YF4FCX" } })
    batch.put_item(Item={"Author": "John Grisham", "Title": "The Firm",
        "Category": "Suspense", "Formats": { "Hardcover": "Q7QWE3U2",
        "Paperback": "ZVZAYY4F", "Audiobook": "DJ9KS9NM" } })
    batch.put_item(Item={"Author": "James Patterson", "Title": "Along Came a Spider",
        "Category": "Suspense", "Formats": { "Hardcover": "C9NR6RJ7",
        "Paperback": "37JVGDZG", "Audiobook": "6348WX3U" } })
    batch.put_item(Item={"Author": "Dr. Seuss", "Title": "Green Eggs and Ham",
        "Category": "Children", "Formats": { "Hardcover": "GVJZQ7JK",
        "Paperback": "A4TFUR98", "Audiobook": "XWMGHW96" } })
    batch.put_item(Item={"Author": "William Shakespeare", "Title": "Hamlet",
        "Category": "Drama", "Formats": { "Hardcover": "GVJZQ7JK",
        "Paperback": "A4TFUR98", "Audiobook": "XWMGHW96" } })
```

- `PS C:\dynamodb> python .\insert_items.py`



#### #8 Books 테이블에 항목을 검색

- `c:\dynamodb\get_item.py`

```python
import boto3

# boto3 is the AWS SDK library for Python.
# The "resources" interface allow for a higher-level abstraction than the low-level client interface.
# More details here: http://boto3.readthedocs.io/en/latest/guide/resources.html
dynamodb = boto3.resource('dynamodb', region_name='us-east-1')
table = dynamodb.Table('Books')

# When making a GetItem call, we specify the Primary Key attributes defined on our table for the desired item.
resp = table.get_item(Key={"Author": "John Grisham", "Title": "The Rainmaker"})

print(resp['Item'])
```

```
PS C:\dynamodb> python .\get_item.py
{'Title': 'The Rainmaker', 'Formats': {'Hardcover': 'J4SUKVGU', 'Paperback': 'D7YF4FCX'}, 'Author': 'John Grisham', 'Category': 'Suspense'}
```



#### #9 하나의 쿼리로 여러 항목 검색

- `c:\dynamodb\query_items.py`

```python
import boto3
from boto3.dynamodb.conditions import Key

# boto3 is the AWS SDK library for Python.
# The "resources" interface allows for a higher-level abstraction than the low-level client interface.
# For more details, go to http://boto3.readthedocs.io/en/latest/guide/resources.html
dynamodb = boto3.resource('dynamodb', region_name='us-east-1')
table = dynamodb.Table('Books')

# When making a Query API call, we use the KeyConditionExpression parameter to specify the hash key on which we want to query.
# We're using the Key object from the Boto3 library to specify that we want the attribute name ("Author")
# to equal "John Grisham" by using the ".eq()" method.
resp = table.query(KeyConditionExpression=Key('Author').eq('John Grisham'))

print("The query returned the following items:")
for item in resp['Items']:
    print(item)
```

- `PS C:\dynamodb> python .\query_items.py`

```
The query returned the following items:
{'Title': 'The Firm', 'Formats': {'Hardcover': 'Q7QWE3U2', 'Paperback': 'ZVZAYY4F', 'Audiobook': 'DJ9KS9NM'}, 'Author': 'John Grisham', 'Category': 'Suspense'}
{'Title': 'The Rainmaker', 'Formats': {'Hardcover': 'J4SUKVGU', 'Paperback': 'D7YF4FCX'}, 'Author': 'John Grisham', 'Category': 'Suspense'}
```

![img](aws-databases.assets/lP5WLAiAahnPfTwq0WFcSADFQQHD757OO4KzHuJRPxVhgMxDmQ834H6QhNBVdOTKRKIqxZXOxMx6yt9E5GEVPN_4JJDc0t5oD7ipNh50NVA_K69v1kVLUU1Vt4rPISy2XwlxS2dm)



#### #10 보조 인덱스 생성

- `c:\dynamodb\add_secondary_index.py`

```python
import boto3

# Boto3 is the AWS SDK library for Python.
# We can use the low-level client to make API calls to DynamoDB.
client = boto3.client('dynamodb', region_name='us-east-1')

try:
    resp = client.update_table(
        TableName="Books",
        # Any attributes used in our new global secondary index must be declared in AttributeDefinitions
        AttributeDefinitions=[
            {
                "AttributeName": "Category",
                "AttributeType": "S"
            },
        ],
        # This is where we add, update, or delete any global secondary indexes on our table.
        GlobalSecondaryIndexUpdates=[
            {
                "Create": {
                    # You need to name your index and specifically refer to it when using it for queries.
                    "IndexName": "CategoryIndex",
                    # Like the table itself, you need to specify the key schema for an index.
                    # For a global secondary index, you can do a simple or composite key schema.
                    "KeySchema": [
                        {
                            "AttributeName": "Category",
                            "KeyType": "HASH"
                        }
                    ],
                    # You can choose to copy only specific attributes from the original item into the index.
                    # You might want to copy only a few attributes to save space.
                    "Projection": {
                        "ProjectionType": "ALL"
                    },
                    # Global secondary indexes have read and write capacity separate from the underlying table.
                    "ProvisionedThroughput": {
                        "ReadCapacityUnits": 1,
                        "WriteCapacityUnits": 1,
                    }
                }
            }
        ],
    )
    print("Secondary index added!")
except Exception as e:
    print("Error updating table:")
    print(e)
```

```
PS C:\dynamodb> python .\add_secondary_index.py
Secondary index added!
```

- AWS 관리형 콘솔에서는 아래와 같은 설정과 동일

![img](aws-databases.assets/q1TDMGwNmQAEtSbZAQ8SQ6z2CifwmyhO97oZKvpsHY1nnd1joP7GvUdCDz_qKHT3FwgdmnjjvI0WClcfqEDNqFkaRZt4ZgpjdYCTeulsXEHMvR27EQFcxI_oPQerXQsGXNQRpItH)

![img](aws-databases.assets/L5xFbOs9QQwJXpPADT85WhZ-MBzf5anAxPt2ucQx20aWyMPmcXtfw1G_UPjIyhaOqanQmKpLBZiflyLxLXQuJqwPNCyn6YFlC0XTXA5uMexosHFJvM-41kpzV_Ut_1ViQpRaHYnU)



#### #11 보조 인덱스 쿼리

- `c:\dynamodb\query_with_index.py`

```python
import time

import boto3
from boto3.dynamodb.conditions import Key

# Boto3 is the AWS SDK library for Python.
# The "resources" interface allows for a higher-level abstraction than the low-level client interface.
# For more details, go to: http://boto3.readthedocs.io/en/latest/guide/resources.html
dynamodb = boto3.resource('dynamodb', region_name='us-east-1')
table = dynamodb.Table('Books')

# When adding a global secondary index to an existing table, you cannot query the index until it has been backfilled.
# This portion of the script waits until the index is in the “ACTIVE” status, indicating it is ready to be queried.
while True:
    if not table.global_secondary_indexes or table.global_secondary_indexes[0]['IndexStatus'] != 'ACTIVE':
        print('Waiting for index to backfill...')
        time.sleep(5)
        table.reload()
    else:
        break

# When making a Query call, we use the KeyConditionExpression parameter to specify the hash key on which we want to query.
# If we want to use a specific index, we also need to pass the IndexName in our API call.
resp = table.query(
    # Add the name of the index you want to use in your query.
    IndexName="CategoryIndex",
    KeyConditionExpression=Key('Category').eq('Suspense'),
)

print("The query returned the following items:")
for item in resp['Items']:
    print(item)
```

- `C:\dynamodb> python .\query_with_index.py`

```
The query returned the following items:
{'Title': 'The Firm', 'Formats': {'Hardcover': 'Q7QWE3U2', 'Paperback': 'ZVZAYY4F', 'Audiobook': 'DJ9KS9NM'}, 'Author': 'John Grisham', 'Category': 'Suspense'}
{'Title': 'The Rainmaker', 'Formats': {'Hardcover': 'J4SUKVGU', 'Paperback': 'D7YF4FCX'}, 'Author': 'John Grisham', 'Category': 'Suspense'}
{'Title': 'Along Came a Spider', 'Formats': {'Hardcover': 'C9NR6RJ7', 'Paperback': '37JVGDZG', 'Audiobook': '6348WX3U'}, 'Author': 'James Patterson', 'Category': 'Suspense'}
```

![img](aws-databases.assets/IYUpL-h_KfKrUw4zszY9ro9tOdrPddOhT_n8ZrmV341r9hb-pNZJ0Hd0AK3EMhGZuLCFAu-B39eODPqYUcwyIOsLuVOgoSLxZBqQYJQeO5NfqduO_rFT9nu2n_ZR3XVTjg_QRlRL)