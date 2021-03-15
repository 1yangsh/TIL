## IAM 사용자

> 사용자, 어플리케이션, 서비스를 식별하는 AWS의 객체

- 서비스 계정(service account)

  - 어플리케이션 또는 서비스를 대신해 작업하도록 만든 IAM 사용자
  - 액세스 키를 사용해서 AWS 서비스 API에 접근할 수 있음

  ![img](4-cloud-setup.assets/2PcGiL1xaKC8IFlrUwVBiFhMPI2SytCbSpbpxfiXm0BZEW03oileO8wib6s4I6ndWi51HQ7X9gDG3Too1qKjtJF4F_7yeOTwfOAxarOG6xV5TjP4EymhZpWVT4XyY5rdFnEC4inA)

  - (1) 사용자를 확인하는데 도움을 주는 친숙한 이름과 (2) AWS에서 사용자를 고유하게 식별하는 ARN(Amazon Resource Name)을 가짐

  ![img](4-cloud-setup.assets/gKjk8CdFhdVDsNxrBEypsMT6yBL6UuuW_eFvOhYqAyRsqh8_CvknhQ1E2-4zdYUinymT7izUK4tc25Vp7g7jL4OaRwk_LshyZ07W6yLlgVrxEsfTZJ8t52tm34CXE2CunusAHkEs)

  - 일반적으로 사용자는 AWS에서 자원 및 서비스에 접근하는데 사용할 수 있는 인증서 및 권한 집합을 갖고 있음



### lambda-upload 사용자로 콘솔 로그인이 가능하도록 설정

- 콘솔 엑세스 활성화를 통해 로그인

  ![img](4-cloud-setup.assets/ThlvGx4ZOZ_Cdn7IoU_86emMpeKES99Kl7HZMuPbgMX2xYGN3K2p7HcNWZhyxCLUJtdYC1emHIHFC_mF65c1CFboHS9zjt37G6MetsYeXWck_nYhjWmtVk_4f4DqNWK3WsO5YtO9)

  ![img](4-cloud-setup.assets/GhPviZFLCKS5eOe3UgJJ_ytAGvm9mmu8u-EjGWAfOWpQoqIeg1az1Iaz7yD9jewOpaLwaenazMI_bqMdMq_SH15PNueDrdUVdpZgxItfFWE1kimhPY0zjNql2xuVlEA0qgk4f-cN)

  ![img](4-cloud-setup.assets/OnDqs1aiOr9Cc_648vVhO88zL_V76E7XOo9HgCx5Dy-JrLTESSaeyT8XIk6OBGOIbod59VRZLxx_fztl-oT8u-u24Yz4VvYPNAdSgIUHpLfXtaeHlss59EDQkRd65kr0Sid5JOvN)

  ![img](4-cloud-setup.assets/B7AiZYCUuTw7H4UNY577qLGUygvG3nJca57aSf4zA51T4l_aEQi4zcpBvAZzbWd53pqYoMRbRnniQAhxmptxZ3i2JcfnHaXvwgrE7CR9R4kHDFkvR6qZn9VhVSDajc20uN6HlQLZ)

  



### 다중 요소 인증 설정

- 하드웨어를 통한 인증

  ![img](4-cloud-setup.assets/kBA8DNPEte0yz6zGS3lxIFtsUBf3uUgsNfgjcucj3AA7wCjF7HP1KnDCXw4ag6xPGRM79Ec-8bNkHRE5dR8PhEdBIP4ZgI24X5sLmtSZ7KFSno413js1R5MPVadc44vfT5jasVSf)



### 그룹(p79)

> IAM 사용자 모음

- 여러 사용자에 대해 사용 권한을 한번에 지정할 수 있는 방법을 제공
- AWS에서는 사용자별로 권한을 정의하는 대신 그룹을 사용해 IAM 사용자에게 권한을 할당할 것을 권장
- 그룹에 속한 모든 사용자는 그룹에 할당된 권한을 상속 받음



---

<br/>

##  lambda-upload 사용자에게 할당된 권한을 Lambda-DevOps 그룹을 이용해서 제공

### #1 Lambda-DevOps 그룹 생성

- IAM 그룹 생성

  ![img](4-cloud-setup.assets/7qYyJwN13tzsye3wliFxa4xKOsTg0ZfMrU10pNFWdyAtJrCV5E470fceiPs5CHWUE3-6x7bOzJt7F2xeILRMgX5HNK0URgs2jgowstfSJN-89El0xcdFr1sqixnqhf5rxI3Zh6Z5)

  ![img](4-cloud-setup.assets/oNq37x9a96SjCNnV78uuKWglQCPof65g-BVE9QnRg1FbRn8BS38NA1N641meY9OhGSrfrAY6bDrEbebRdGc0q8ycKW65ovl0sdD1_uTs4dOmO6JUOe09-LL2ElYHVbN6v824G2fa)



### #2 Lambda-DevOps 그룹에 권한을 설정

- 그룹에 권한 설정

  ![img](4-cloud-setup.assets/wumwspNKN_GKaa25Zq9X-DFQgTrpR3qY5fGEUd7tebqYQTZ3CWbJhcp8Zr0LEGlLUPdvAvPmHH3h94zxd1u2lb4_NGkRJdU53SMziU1rxFezvZIr-vJt2QTpfBwAUsu48Na23AHV)

  ![img](4-cloud-setup.assets/tIfRZm9cZc5W7kZVeCPAvVwtN7-oI_5wQ-qp4uPhWpUQK7pIlgt80_EsXQfFlguD07dmlqAkHSJAinwIqa9fWk-5HdqJQ8-4WpdWqt-DzOLMWUyT1ED7PggYjdQQEy_a6rw8HiW3)

  ![img](4-cloud-setup.assets/lV5Pz-NMwiz-S1AnRyQXMPv5mPYUYQavd8CZmCHI2Z1rLTM7cDVBgcSSFv_oXWEhyMQPRir-bp7IQPqvrphnAiOE6PVCb_PinAjE9-s0kXs91aKmxC300IZ6R1IAQ060VK-e_nDe)

  ![img](4-cloud-setup.assets/R8dxtSPwX-rCQIG3E6NMI8w83uLTiiNkyLzWOSdCGnBkZgx8-pc2WLdWEy7otG2qDlzCRD3so7_A7r0Uge2xNPXfwD6WidA-XLfvX9vn3_u2MrBLHsQJ2IFvb-Qc6RwC3Np1y82H)



### #3 lambda-upload 사용자에게 부여된 권한을 삭제

- 사용자 권한 삭제

  ![img](4-cloud-setup.assets/zhiTqA2KuWMJemUZWrMBcXEE9AmsACGrKyGbR9Sh11WpPKwKrL0UsXJ8dLQA5i9CBGcOZsuEgGmVKOni5IRUZc3z7Ymqz7EYAD9oAJksjQdY2v0YCF94hdhRJXF4rVds9iq0p5HY)



### #4 Lambda-DevOps 그룹에 lambda-upload 사용자를 추가

- 그룹에 사용자 추가

  ![img](4-cloud-setup.assets/7WHxBcNG_cfKDzJM-oEO7e6i_pkilxB9CwJQQJxiHfF6KSuBqdmU_viRPziM_gK5K3bIsSQAhoDEtFI-Z606uDD9kMffn-7GQzjdFw-UVLwC17F_gXAp7W59v52V9gXUqbImkhpN)

  ![img](4-cloud-setup.assets/r8SO_aAbra5-z1N83uVc7NTcox76CsTg5GnjCaFUFZ8G543SHXaY2ITWyo0x9znoyDqSX38cYaHTRzqjsoe5FYdacj5Hg5eic2AmHGhhhhW9Kwww7wdEDI_HaZ-0EshC4NyLSsku)



---

<br/>

### 역할(role) (p82)

> 일정 기간 동안 사용자, 애플리케이션, 서비스가 맡을 수 있는 권한 집합

- 특정 사용자에게 유일하게 결합되지 않고, 암호나 액세스 키와 같은 자격 증명도 없음
- 일반적으로 필요한 리소스에 접근할 수 없는 사용자 또는 서비스에 권한을 부여하도록 설계되어 있음
  - 예: 람다함수가 S3에 접근할 수 있도록 역할을 생성



### 위임(delegation)

> 특정 자원에 대해 접근할 수 있도록 제3자에게 권한을 부여

- 자원을 소유하고 있는 신뢰는 제공하는 계정(trusting account)과 자원에 접근이 필요한 사용자또는 애플리케이션을 포함한 신뢰는 제공받는 계정(trusted account)간의 신뢰 관계(trust relationship)을 설정하는 작업



### 연동(federation)

> 페이스북, 구글과 같은 외부 자격 증명 공급자 또는 SAML(Security Assertion Markup Language) 2.0 및 AWS를 지원하는 기업용 자격 증명 시스템 간에 신뢰 관계를 생성하는 프로세스

- 사용자는 외부 자격 증명 공급자를 통해 로그인하고 임시 자격 증명을 갖는 IAM 역할을 맡을 수 있음



### AWS의 권한 (p83)

- 자격 증명 기반 권한(identity-based permissions)
  - IAM 사용자 또는 열할이 수행할 수 있는 것을 지정
- 자원 기반 권한(resource-based permissions)
  - S3 버킷, SNS 주제와 같은 AWS 자원에서 수행할 수 있는 것 또는 누가 그 자원에 접근할 수 있는지를 지정
  - 주어진 자원에 접근할 수 있는 사용자를 지정
  - S3 버킷, SNS 주제, SQS 큐, Glacier 볼트, OpsWorks 스택, Lambda 함수 등의 서비스만 자원 기반 정책을 제공



### 권한과 정책(permissions, policies)

> 사용자, 그룹, 역할이 수행할 수 있는 작업을 기술하는 정책을 생성해서 권한을 부여

- 정책 유형
  - 관리형 정책(managed policies)
    - 사용자, 그룹, 역할에는 적용되지만 자원에는 적용되지 않음
    - 재사용, 변경관리, 버전관리, 롤백이 용이
  - 인라인 정책(inline policies)
    - 특정 사용자, 그룹, 역할에 직접 생성하고 적용
    - 엔티티가 삭제되면 엔티티에 적용된 인라인 정책도 삭제
    - 자원 기반 정책은 항상 인라인 정책



### 로깅 및 경고 (p86)

- 로그 보존 설정

  ![img](4-cloud-setup.assets/IWeInIG1mwGNlqLBfA2SGd8Yjzo3hkvuKC7zlEKi3_4_ZAXRnK_Ptf1EvjFbQxvAq8VUqzwWw-64PA83vyfB981JuhF1TwwOljCrcPDHrlCguEmEIn4_iRjw7ELDtxcCNPpNyPuA)

  



---



### 프리티어 사용량 알림 수신 설정

![img](4-cloud-setup.assets/image.png)

![img](4-cloud-setup.assets/1GgzFx7A5KrovB9if1UrWdno9ZkxaPlOD9APiI-ovtu9nv_iyZWqhC91FOQzJwEvtiq33RmGwovOopz52yB21WheonRc2vGuDgFl-Yrgy3BxJATFqEg00BKYCGbRINXdo06Er5F6)

![img](4-cloud-setup.assets/yzNYKBLMzmIVgWJSVSCcSXEMCwa5QA0yHJ9SZZ5YXPua5N2gE8yamTVlaM5mX4Y0Lu3pAOhp6zlqLSrPVtyOT2yQ0R03a9psk5jWY5g9LUfp2A00zYOVRdMedX7L30C695WmnMQg)

![img](4-cloud-setup.assets/i1WKB3kHVMJkzduwTghIXsGFx8PVXAKp5x_de-XUtxr7HBO1KRFoWCTV3ciBce7YEJfkg-s4108-HMDPYMwM2wVHrpEO2VBtz8_QNXEUOBjeVn7d1VQvzkCixNqh9GC-UDLSeTf2)

![img](4-cloud-setup.assets/ESYwaWwHMDSxexwGqg0Ooan73v5kdv_dO3ws0RWi13qxzyEMX9-YP4rC17i_7oiiRCkPx7UEeqUQYl4pLolnwr96Ck9FGVwhN81g53UEGIPC-613-kqRsOFokX9thjNc6vZHO5lu)

![img](4-cloud-setup.assets/0o6AHWgO3bTF5DxL99GpO6Wy7lSovXbVcPTAbJuHyIvMKudGtqLMD8IeJr-oIqF0Xm911O3vE8c56gfMoyDWq2aMraS54VSWdk5fs_b1LakmMeeG-P64WiS7CqjJr1lfhri9blwh)



![image-20210315132816302](4-cloud-setup.assets/image-20210315132816302.png)

![img](4-cloud-setup.assets/TRm7R8p9PxXzj2il5x9Vbh6a4ygqJHX-_3J-463aOeQDKTXxjfN9Uw-IL746XP1LUIx0aJ32KP8Kc-HGgn-84_TRwXAxNgHdTEOw4ojGvD6FICTH-MRfExZoPdiyyhVSeFAZvzSZ)

![img](4-cloud-setup.assets/obcwiIjgYmBNW9RAG69DwWX_OGSWqqVG4lqDhqtc0kOCfo8aWiYvR0eqLPm4MAIBDNnsM47KSyVAHeAmS9o4v7DgYnNnBk9mY2gCkllCZzuHqpovb8kiBnQMyIF6_T5voLmc0aQM)



#### 자신의 이메일에서 수신 여부 confirm을 한다

![image-20210315133101617](4-cloud-setup.assets/image-20210315133101617.png)



- 요금이 $10을 넘게되면 해당 이메일로 알림
- 경보 생성에서만 그치지 않고 주기적으로 사용량을 모니터링 하면서 서비스 이용 로그를 주기적으로 확인!!



