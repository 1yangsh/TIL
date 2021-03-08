## Intoduction to AWS Identity and Access Management (IAM)

> AWS IAM

- 자격을 인증하고 권한을 부여

- AWS 고객이 AWS 계정 및 AWS 내에서 사용 가능한 API 및 서비스에 대한 사용자의 액세스 및 권한을 관리할 수 있도록 하는 서비스

- 사용자, 보안 자격 증명(예: API Access Key)을 관리하고 사용자가 AWS 리소스에 액세스할 수 있도록 허용할 수 있음

- IAM 정책

  - 관리형 정책 

    - 모든그룹이나 사용자에게 정책을 일괄되게 유지

    1. 기본적으로 AWS에서 제공
    2. 사용자 지정

  - 인라인 정책

    - 각 그룹에만 적용되는 정책

```json
{
    "Version": "2012-10-17",
    "Statement": [          // 정책 문서
            {
                "Resource": "*",    // 자원
                "Action": "*",		// 작업
                "Effect": "Allow"	// 효과 = 리소스에 대한 작업의 허용 여부를 명시
            }
      ]
}

```

![img](aws-IAM.assets/JFAUD34S1cABrcnKmjp1T_-B-p4EnXuq7ASgA0INK9___ImNgvTBvSz4TceC0JlQFC5fL1lOS_ftbbcRkeeDr-QCf2IoFeX6I7xTmddzdOh_rjjDjASWHDU6qAgoyrJxL6aaMxMk)

- S3-Support Group에 적용되어 있는 정책

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:Get*",
        "s3:List*"
      ],
      "Resource": "*"
    }
  ]
}
```

- EC2-Support Group에 적용되어 있는 정책

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "ec2:Describe*",
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": "elasticloadbalancing:Describe*",
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "cloudwatch:ListMetrics",
        "cloudwatch:GetMetricStatistics",
        "cloudwatch:Describe*"
      ],
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": "autoscaling:Describe*",
      "Resource": "*"
    }
  ]
}
```

- EC2-Admin Group에 적용되어 있는 정책

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": [
        "ec2:Describe*",
        "ec2:StartInstances",
        "ec2:StopInstances"
      ],
      "Resource": "*",
      "Effect": "Allow"
    },
    {
      "Action": "elasticloadbalancing:Describe*",
      "Resource": "*",
      "Effect": "Allow"
    },
    {
      "Action": [
        "cloudwatch:ListMetrics",
        "cloudwatch:GetMetricStatistics",
        "cloudwatch:Describe*"
      ],
      "Resource": "*",
      "Effect": "Allow"
    },
```

---

<br/>

### # 추가 LAB

### 1) EC2-Support Group에 EC2 인스턴스를 실행하고, 중지할 수 있도록 권한을 추가

 1. EC2-Admin 그룹의 ec2-admin (인라인 정책) 내용을 복사

    ```json
    {
      "Version": "2012-10-17",
      "Statement": [
        {
          "Action": [
            "ec2:Describe*",
            "ec2:StartInstances",
            "ec2:StopInstances"
          ],
          "Resource": "*",
          "Effect": "Allow"
        },
        {
          "Action": "elasticloadbalancing:Describe*",
          "Resource": "*",
          "Effect": "Allow"
        },
        {
          "Action": [
            "cloudwatch:ListMetrics",
            "cloudwatch:GetMetricStatistics",
            "cloudwatch:Describe*"
          ],
          "Resource": "*",
          "Effect": "Allow"
        },
        {
          "Action": "autoscaling:Describe*",
          "Resource": "*",
          "Effect": "Allow"
        }
      ]
    }
    ```

    <br/>

    ### 1) S3-Support 그룹에 인라인 정책으로 추가

![img](aws-IAM.assets/LwRQscYfsfUY2I98UAa1e-WjVA0YiLHZqft8uMgud_V2SC-N7WReid0RRSetmwvbcm58KFG2li3DhTNUZlRgNon-VsFrV9PTaFHlU8UdXtMhX0SdvXrBJOFqv92lWvCN4YWGQpx7)

![img](aws-IAM.assets/y6GheyMOu5c3nlU1SzQkkaRk8svIvcKIj3e5N_6WG6RfC-KId-F9jNfd8DzsUH2dSjfLibpRLkPCMcsxcuSffPuhxq-PLEd_9imHO0OMJwXb5RMlfakvhjWx9BQ6n4dYeTH2vzrK)

<br/>

### 2) user-1 사용자가 EC2 인스턴스를 조회할 수 있도록 권한을 추가

![img](aws-IAM.assets/i1WnzH099U8QAx1fTX4HX1rXWh9DiS7vHt0K0kaa6jXYK5Vjx2LfeohcZOFFiY-C9ISutVk3DRXi084WG8rAYcfDRYAa5ox_UIPTsEPLTMdMsP2-CHUuwImEoWWZTOX1RE5GiMpU)

![img](aws-IAM.assets/7kggqjH65Hk5Zaa27YQZW9ybU9lzvA6_9dbQx7jczqgQDiqioXdSyPkDwf9lLQTfAPsuQ0PhUQ-pJklIbuEIbRJXHypVT3uxxr-Phm1rF3xSzRdC3AhjjO6VxWrI-5mDvB_VBulU)

<br/>

### 3) EC2 인스턴스를 실행하고 중지할 수 있는 관리형 정책 ec2-manager 생성 후 적용

![img](aws-IAM.assets/wiUfyiNRuAd00Ui3dfp8Mo0nv51hxWTy5w69dSoWyJX2z568GY4an1l61OQsrrZpohZzCI-OShJ0-5g8HUgomtXQ7d68Xg2qZqbBI7_ifQaQL7zZkSOn4iD9XfOOBX4Cx7Vk7H29)

![img](aws-IAM.assets/K59ii1nr-4ofN8l8_oKYBMTOpOC8pzTBBJFIkW1a-imHxMLlrR4ONsJcKKQDj2mAPYsCNhlU7o5wJ44bpkBBHCgXj4L6Yw4Bl0_vzxUB50CyfKoHaF7udJbA70YKTcF2EeL9rYvy)

![img](aws-IAM.assets/RaVNbGGtkgL2C1c162pnzbdiXh3OJi5RBvfBJizl_RnfeMjcIOQIOivle5G8wVPIUjMhb5XSkudR2xzLocPz9Ict7FiS0ipvPvpn7lbKvLukEDBpNRtWBlqyTGL40VV8z1YtpQAI)

![img](aws-IAM.assets/3fXGlqG5Sscwu0ilvRsGA7HWRa1oegr8zH-bxvD_oWnx_UYe2by0dpTbN-ReN8iVlL9KazE0IOf0U1bOFuzIFEkLQKdsV7pLZw5lg0BzU0OidVr8_ocYbQSIfWv-a1pPIVNDT6XN)

<br/>

###  4) ec2-manager 정책에서 중지 권한 삭제 후 user-1, user-3 계정으로 EC2 인스턴스 중지 가능 여부 확인

![img](aws-IAM.assets/1waHD33s2qPi64RTcSH0c2Ih_LVcb75UE-F7hyfIrDpbCWN_-wkvXeNef3wruzViyvcCftRjdsW7uR7l0vmqLPzoENm2Ls5Vh8EPX8BpYnDJUAxZkc8Hku-wjAGLAV6TrnqvulRt)

![img](aws-IAM.assets/RVo1bor02Qp_HdmA0TNgAWDjxjspCYN8zLJmCUmnzDiJujVCyiMGFp6sqm-0qDHvAjWuhJXpDNXGoMEXNTOeGtSXgWUCyJAQknium6LVkMsu55iZGH7Ifma00E1i0quWza60F36v)