## CodeDeploy로 현재 위치 배포

> AWS 인프라 구축 가이드 (p134)

### #1 CodeDeploy를 위한 서비스 역할 생성

- 역할 생성

  ![img](6-aws-codeDeploy.assets/tf1GNT-Gpu0OlOaGQ0mIqhPN-EI2zy3eY_d185Cx9m8aKer9eQ9qJOj4uFbjgW_gHG5JHy9aPfuc0jjorFDkQuJWUMWtuKC6XOWwcWdme05llvjcxgXw31rVfTES55S_bQD7bbJN)

  ---

  ![img](6-aws-codeDeploy.assets/5RuoOZpp0tAaXiOOdZKvd2Tk-DFx09-SgoVIMzxy80W7Si_g7uM54NZrQ6TGWL-k77CAJEEr1eW-Y_YOPh8AutG3LvkMRIjOVTY4FvAHGVTOTP0aMSTv9JoXmcc8oYWV8WzWD4vo)

  ---

  ![img](6-aws-codeDeploy.assets/6FGbwYItG20mw3FLupsnfQ-iLM5Bx_8f7HE2WZ5eHIPBOmAENwy0BUjm1APHXvGElLDkwhUlQs4SfFPxQSOCbVqCGpsZa2yp25Rzs6TACzYFFRdbX7dGCXGgiZzYwlduYKc5R24M)

  ---

  ![img](6-aws-codeDeploy.assets/iS5XBFIl0rq33gzXYUQUPCLxT2kLpBdHsk8eSQv2ejp4yN5BgE9WVyhSD0r2FQ1-bW-rJTFtMzhSaFiM27PyXbg8VZELitDVwZGxLnV9PSBFzndDZlq2mJKMgcRo18oIfoFLmq7R)

  ---

  ![img](6-aws-codeDeploy.assets/Ox5S4yROhlQer5uLubFN_TACd32kvd9YaLJZ3tN2CeIOlkwjPP-muAv2Vk0ACqtVhl5eo3Y5jgl-FF-ixJAjjIlULe9PnBNm7B_j6zhTRWaedurK3fDRPGOXwGS7vPsnFV4H0GJl)

  ---

  ![img](6-aws-codeDeploy.assets/iUjyVqjqq5u8NCsoC26QMU9jAH3FzA5bO7kANkDKYYIATUFv0m7_gm45nINlbt2924YTiq6cGrNn5yAWBOF30hs5jFNLrbZNMryEG8l8Dpe7HvvnCLs08P5rumsReG_2GRKKedk7)



- 정책 생성

  ![img](6-aws-codeDeploy.assets/y4iWNCNJLi9PccuzE7XZUgcA0xfnH-NvP4HaOMQFLG1WgDdNH33nFmm2MUlTtVBIJYK3kKk2GXXmhyNtx5yjWHfYHTKma7EurAdYVaCTybOu47Ua1BFL44BQieCxomUkLtB0K0xM)

  ---

  ![img](6-aws-codeDeploy.assets/ABMr4253evSwyBBIgxXc7cQKFnEZNCr2ENCRnJ9rZup3rcXETI648yKy2uhJpv3K3H5x0YELxMF0PSHgXnQctN48yF0uguGiRWdnB_Bx9_FN5j4nvgAS0rxmsZE2KclD5SxOCcrA)

  ---

  ![img](6-aws-codeDeploy.assets/TjKdcnz-6eSzduWhrgvFUW828nbyOh17jisAYEyDTJcCEnQw7PKBAwYWLTYb1JxiMXZ9cfPpJMhePWZFmmI0lJe9WhW95O2CA2U3Br93-BlEw2rve914KwmlSswAd_iNQ2qpn2Cp)

  ---

  ![img](6-aws-codeDeploy.assets/hakGfMHsnab6it2LNIj96a66VJ8Pa3T11pXLF0m4XamQOs6IU-ts6pSbTjhuinZA9GbkkZR9_OTwXa705VD-zxTnPYenQsBMDjdWg3iROTD4C9O8hhpXsEgJe4ly6Ru4U3DzODpF)



- ec2 역할 만들기

  ![img](6-aws-codeDeploy.assets/DQ-QM0KeuITjHPvPsPYiGToNwDcQ_uF1_v-JuCCeqh7FAE6Ibuo_qJFDIptKkU4kbmhIpr9H8yseX3Kzxw2MgVeconyFAZfUEQ8W0DSQl65FsGZLQEisSDXKzvp5Qp4vFL4-1I2r)

  ---

  ![img](6-aws-codeDeploy.assets/NX8XBiDSGekUtimxhjGvFZJwDnmU1OvHRhvbGM3mMD0mQOzU15uJig6_U3-LMH2A2Kw7LyZYViqIkWfnCevrSgoF_hHAFuPM37GkxWgyVSisvD8p6h_0p7iskdgleGvF08ILqmHN)

  ---

  ![img](6-aws-codeDeploy.assets/UG1cRPWlxFhMaZPrp5fV7Alo0A09qRmvdatGGrUpUGcRm1lqWYuqgbzkpgbGtIbzk10KzOTNcND6Tvm7v3YshF_a_9YrqraKGuUTLNs5tt1cQ-GoX3md06sk-6IOXGmFVxOjHCgh)

  ---

  ![img](6-aws-codeDeploy.assets/7qyA_sL-Y3GW5ICVH7opK8uLOjvt8eyS0IOx1S2CbR7Y-6abLjTL-xQTXdc2BGAylR0_1frFy4esWXTwUth6LNitCa_r4YA7HpfX-7t3QywCoM6uZMWgw-l0CyqvcApY2v8WljdK)

  

### #3 CodeDeploy로 배포 가능한 인스턴스를 생성(p143)

> 애플리케이션 코드가 빠져 있는 인스턴스를 생성하고, 서비스 환경(로드밸런서, 오토스케일링)을 구성

- 인스턴스 시작

  ![img](6-aws-codeDeploy.assets/SWLkDkX7l1xqm6uk2nUcGoAWxDSolzxsaHltsRt5aWDkuQnUON3Fz0qBLxxYJo22MM7i9sDYbW7BCGfFEcR-uQ63wGZ0UulgaUoakwkT6-YQNWABOKK4J91auoaPvOuLqJHEz3jt)

- 인스턴스의 퍼블릭 IP 주소로 SSH 접속

  ![img](6-aws-codeDeploy.assets/eNFuvnhnjDUxfIQ_rZILWR3w28av-BouY43AH3KEOhSmkYYj0h3DyG4g0hxf41J2DLscbD8Ct0dX6CnbvOqlPPCV6E63m9bnsHQ_O7mFJkr8kQO5XFmWt_2K_ZK4JcXGwYxpdpoL)



- CodeDeploy 배포 디렉터리를 삭제(이유 → P144)

  - $ `cd /var/www`
  - $ `rm -rf aws-exercise-*`
    - aws-exercise-a,b 삭제

- CodeDeploy Agent 설치

  - $ `wget https://aws-codedeploy-us-east-1.s3.us-east-1.amazonaws.com/latest/install`

    - CodeDeploy Agent를 가져올 주소를 생성(확인)하는 방법
      - https://docs.aws.amazon.com/ko_kr/codedeploy/latest/userguide/codedeploy-agent-operations-install-linux.html
      - https://docs.aws.amazon.com/ko_kr/codedeploy/latest/userguide/resource-kit.html#resource-kit-bucket-names

  - $ `chmod +x ./install`

  - $ `sudo ./install auto`

  - $ `sudo service codedeploy-agent status

    ```
    The AWS CodeDeploy agent is running as PID 3671
    ```

- CodeDeploy Agent를 설치한 인스턴스의 스냅샷을 생성

  - 인스턴스 중지

    ![img](6-aws-codeDeploy.assets/xFEOUYtP6N0TIrn0B671eb-WFWYHU7PauKkim90sglTthcZSgmJRsUxg1D8cYCc3-iSyKMkc0W3Yu7asMVuhYCK1RrkOTi8uNmTfnDN0vufDwlSy9OmKY67GAbdN-xEWgshQevwI)

  - 이미지 생성

    ![img](6-aws-codeDeploy.assets/IK6Ql91b-We1GDqvaboxrRzGa21JwJV5ZAkD_TMIYR_mnJx-FGkCY4g5xBjRDXq5cg3gs8F_FRecsH5W77c3Yqr27_Tm4_pMAQoGX-VK47UzhM5IQw8p7GxslaNnjCBdrbl8GHtk)

    ---

    ![img](6-aws-codeDeploy.assets/ZQF1Q5tErh_n1MFaFoQw0TwxDU2MSzOUvFkwtVRo3BIZL8pgKscUFRSBocsN41pqM5IE8WAohzP2yM7Rc-8Fxq-pU99oxZmXiAWWUUMuFRZ4Fb8zFdU9vTc7R5kMHsTRRW7l_Rls)

    ---

    ![img](6-aws-codeDeploy.assets/Rd_Gs3zoDn_NezLQ2XouF3tvtoMe6cUTY2TsWpufd8yb6jzSyck92yHAot6E8KorwmuEduL7QGl-4iV8EnzMwiOJQaHN3iCBdK_ASS3yRtPo5LUpj8lZrS2g0E6eVBfEuvrGIEJF)



- 시작 템플릿 시작

  - 기존 시작 템플릿(exercise-launch-template)의 설정에서 AMI만 변경하고 나머지는 그대로 유지

  ![img](6-aws-codeDeploy.assets/tJr5nSJ9Xr7SigC_KHzYZf5s8WXizjstwqa4BQVBbx0-4lfaQyyt5roax6k2PQ5LIrYRMALc4faggTytTtZKwM_az-0G_wK5qWOJPFQMriYus4zpur102YXbWWHWMsYFEc0ocptf)

  ![img](6-aws-codeDeploy.assets/oZ-IntGg5ejWCwAX98PHwWnbqSxgq1SwiveUDZTf7DZKSvUa8v0oK9XGnPMGUF5urQIMgVdRDs9RxNKi5ln0_2ZTr1OA8SuT0wWfyebAhWBr_n7rSk7uTc-c-xmw0i-th6AZHc0q)

  ![img](6-aws-codeDeploy.assets/9TOFnqJ-Dsy6twwq9QHUcydWwvEEZVstsH_xEc3BLKkQz5SpQZiCODk7LX53wt4JdUCHQfjRaxY4SeXDPTeHZ8621CbfawXIz_v4hQfMyJrldID43mljYSlcWJKgqsjyEFCWY1Wr)

  ![img](6-aws-codeDeploy.assets/KiZbEqf2OSjondjkSK30K-olleCnraKxWhGS7RMc36egdlJQdLL8rjmbXZ6StR0NZ774LNe1hRoaie_83MFqPS9icUntQMOOgM_GkjXRG1vHTbsFuqsYjc4dm8C0T4zzaOeAvkaH)



- EXERCISE-GROUP ASG의 시작 템플릿을 변경

  ![img](6-aws-codeDeploy.assets/Jnw1jdF5JCY316J59yv5jZ8FTW9PBNL8tiE_wDUoNILG3i_XrHf7ZlVq4WMHigTijZlECVocuN7IJDbt9ibK-35_b19KaoT3XWdKPDpH3lJwd2zunpJ2G37wC9eVUbOXzkJq8HPd)

  ![img](6-aws-codeDeploy.assets/0xkkq8bWCb-hJ_Rtqk_QrxLP5e8r9i_jA65etgx8vcylqd6QilLHJftJyt7rvno4bCz05kBl2GKd9BkQEzZYpVdxhD0bMCDrtLYnF_gtFWnvT49KZve9eerZDNCOSKxtVwl5mdrP)

  ![img](6-aws-codeDeploy.assets/yKCb0AlnrcsIhyoGgNwmiTRzCH2uPUENVlgJqdAbl7Cr60RxzxzduskUaLx-_RIbbLlpdp0VCQFYOQRYuOf_KMv-cm_EjVu36RWzKwU-KbIe41ooUFBnSXPIbNdFUcN9wkj0kCT-)

- 용량을 편집

  ![img](6-aws-codeDeploy.assets/mLHwDUd8tkvq3Km0K9QFxFtGDASQd7hqFN58BdpICjtE8du8y6k9zt6jv6rFCYQuTroCZ_aLO7t8115lNHkeHA8OVq_D6pL1p_7h1KQSdLYqbQ2e3Kdj_AsUuw6b0_m_kuj4gePr)

### #4 CodeDeploy 애플리케이션 생성 (p152)

- 애플리케이션 생성

  ![img](6-aws-codeDeploy.assets/k8Gk3LtIwenEq5xCwdLjr6u_c3rNWSf1zDYL-04fyYy6oUu8awABE1fvAjWEsTKcfjwtYAU6duex5lIL2ctfyc5sOM0IHvmZqHckJDvEeX0VaNhjHTxh0kCTKgeqWgf58aLNnq4B)

  ![img](6-aws-codeDeploy.assets/YsZoPzZXxVRCbOsSIQNR_vmOAd9MxZiMMMYAkw0ywm25mwVqTLwVKrT7azC7oJ7ffC7XRhXSYNioG-Z6JXqx65AVgI8mVIJDE25mhwv0xKniP-MlqULaFVYV7cIQ66SbnyweT4Xm)

  

- 배포 그룸 생성

  ![img](6-aws-codeDeploy.assets/NoMrbjIbLv-qL9WtoS5ZGuLsdHQBYkOqJQL1jY47H9_-YfnvHif6W1FkJxCYpo4hNMU0Cu2-eR_sEQmEUlbQRwTfbQHFJvuXAEBMUsloh6brTPNfAvZfYADK8CH46cmtUfoE74nQ)

  ![img](6-aws-codeDeploy.assets/NYCSfK0-9tim2L1H5xltEak7t9nVkuSVZNyZ7KaPNXQYM1bd8ugb1OAZEmFxJuqy_srRAk826LN0Z7pZdZVt4NZdipgoNpN34ZUWsuOeQd4nu5tiAUGGmraPzNqRbztNJ2n11xa4)

  ![img](6-aws-codeDeploy.assets/LHXqyZOQAfgoMrHxGssYiCTtC3gT-RetEP2iwHQOH1mQbN1OSh2_FLbJWXzq58Gve-YbyYkyeCjcfw3kHt8aAfeIYiwwW734UOZMy_gXIBkeIlKM6WF0hjaVJ8QfujZUsofKpK5n)

  



- 배포 생성

  ![img](6-aws-codeDeploy.assets/7pEXdlHEXViRjLGAoI23QoKRIxvujZpnSEphn28ewfWCppyRdy80K2DzUY8fsEMvXMNiz3VOgZ1ZgaR2UYV-XLEX0fC4nMGx9jBe6DXhoqAsW2Laua-kFw7wzngdgKT3zlct9XlP)

  - Github 계정과 연결

    ![image-20210309170407657](6-aws-codeDeploy.assets/image-20210309170407657.png)

    

  - 본인 Github에 접속해서 repository 생성

    - 파일 업로드

    - `appsec.yml` 작성
  
      ```yml
      version: 0.0
      os: linux
      files:
        - source: /
          destination: /var/www/aws-exercise-a
      hooks:
        AfterInstall:
          - location: scripts/set_owner
            timeout: 5
            runas: root
          - location: scripts/install_dependencies
            timeout: 120
            runas: ec2-user
        ApplicationStart:
          - location: scripts/restart_server
            timeout: 10
            runas: root
        ValidateService: 
          - location: scripts/validate_server
            timeout: 30
            runas: ec2-user
      
      ```
  
  - repository 정보를 입력
  
  - 커밋 ID 입력 후 배포 생성

![img](6-aws-codeDeploy.assets/ajTYCuDRnUHyscAGovQoBOWFtEZfMdFXGP6VjuAel0_2CKt5iV-KWxff4VDDuM_PFZnAO_MRiknGJM70kKe--GrqaceKUrikreTxW2R8Gl29b051p8-uuRJUHovp3xvf3FgWhkvD)

![img](6-aws-codeDeploy.assets/rGRvQ_xpuMUUG1UuLO4KBpbjOBAXOOwgJa-jNR54-_cz3ZC5JIX0GQx5luhCrAohMxG2MuaOYvKpD67iEbt9VJW_gtNYE9fwYiO7FEUQYwVWFSfnor9DXL6BwqnGfKBorvkiR8lf)

- 로드 밸러서 DNS 이름으로 접속하면 서비스가 되는 것을 확인

  ![img](6-aws-codeDeploy.assets/h_xe9API5OXrAIQWAy0OyaBgxVaNciEppC-UIMBUUisSajKeLud4geLFEk8aMwxoPmmJVAYE6WVyUgfnHJV9JEdXGe5Ws53Lz0kqDsUeGanfBqEEvG-asfzdKWBY62WygveCQVL-)

- 소스 코드를 수정 ⇒ 애플리케이션이 실행되는 인스턴스의 IP 주소가 출력되도록 수정

  