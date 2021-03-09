### #1 블루/그린 배포(p110)

#### 1. 블루/그린 배포에 사용할 Auto Scaling 그룹을 생성

- Auto Scaling 생성

  ![img](5-aws-deployment.assets/VVRm_QH0EO_Vpx6zOmHaBtbxXgVoBUjic31LRsbgk-dmi7pJHxmCpYzBQDCmQu2S4hGPB9IlyLAXKIqT3cREXUwovVFo-qQvn6XjaPP-GJ7eMAIxokEa6XmWHRcPg2hHyrxhMocV)

  ![img](5-aws-deployment.assets/dVT7EVDmWDYsseTDEhImnHBEFiD9JS8HX_DGVc7WPKPmeksk2UzqFOalRW9bju5shMOLcavc6LzIkU9B1oqJ6FnWxZTj0QKrjfjVLlKYdE_6LkBvO05yQkwNswLyVva901H5i53J)

![img](5-aws-deployment.assets/6ToRukhtB1dUwo-Q2M4pzyu4oDsOLck-Th9mpk_wR0uNjZWuIXAOS1ckp2qHvMSydQkeghtSMz5u6JirrjPkMMjfSOXa1-diHW7er27GVnpOiD17Q0RxAZEGCIuxjEvYCwelae0Z)

![img](5-aws-deployment.assets/gcxAgS1qEJhJmU1z0C5IRNG-uZEDCDWe4xkvmUov1YTecZyFo6wtHDDRmeSopKPpmpigcsrO6iUO59RlRU88zzxRsEBnlqbVoFcIAIp-L6d71i-jxk5FRDTTbxKpm_Mskfa7kJAI)

![img](5-aws-deployment.assets/QgI7tPMy-lbZy3lpb7qYeCKkAOmV8gtI-nRd2LFIPJdxIXMq0I3MmPViKRg17KtnMuKN1NNrd09k4bM4500RGxvbygpXGEtzhoomTNCKg8mok0nqPx4zI42sLF4ESmliw_s8eeOI)

![img](5-aws-deployment.assets/T3hiROEsR3P6hOOfuybWD0T4ZOVqPnTwf6cpLR1WQahL9JuLZwJIrrvxT9ILcq6ZQspyu7eagukuT1NNrNBf623VGV328xQZEAIkNOKina4-9gCu08WRax015zMuJ9pkoHpdypow)

---

![img](5-aws-deployment.assets/02TkqCdZYnbJNFzCiBq1Qv0_oiUqP8wSRK-zPGHruSdsaji9axqhX3LkobQpplivpfk0FVmHFjnhRieoNqcAW7llSXjlckki6D_xVKXrAqW2dEfi2q4eNYcIngzxe-it6cxTrubC)

![img](5-aws-deployment.assets/Pe8hGSLrz0IeMmSwWjZljlIDGEFVaPf0y5Rt0ZE5xaum5P8PWqUmcVEcvm0JGsyQw7EAwZ9fzw1kOIDLyzO4QhzFk2_EAeSVjf8XqIcP9b_Q_9vFmICKye4v9vTASyWVgeiRC9tx)

![img](5-aws-deployment.assets/WR8S0Yc6fy14_gPnYviABtTSoRXxVLMWrYSYhi2n5ngJU6fWp0wmrxuc0W_GTNpG-5VelT_YLGMt1SCkhD0X6YpEBHfl1t0IxE5hqN03zJHGGIXhKp2rgsVKFP4byNIa69fmAH0Y)



#### 2. 로드 밸런서의 퍼블릭 DNS 주소로 접속을 확인

- DNS 주소 확인/접속

  ![img](5-aws-deployment.assets/Ui1SGv20O21_7oSjRvqOIUq8_kdvDyvMUL66t7L0yLxTP5pwt4rIYCgc5Hv6lqt1jqsPDEqGZ1gcKKKfsOfcy09KUfQo4wxHTrXQLHkOxZh0EquJcl0jfbCJYkYKNriac4E-7WtV)

  ![img](5-aws-deployment.assets/LWTopn8bNmLM65NKrHxutxrrtvQI-S8O0Xoq0BLRYz_NGXWrV4GmXjdOQlnAZJoHZY71cgaMdGWagS6MRM_abOEmRMe-xB_D6FpX6MKJ4C4JrWmn4ODV9RE-K7Lz7Nm-iX9Vj6tC)



#### #3 새로운 버전의 코드를 적용

- 현재 인스턴스 확인

  ![img](5-aws-deployment.assets/4ed4IHDGRHCMgVXFrn5bUz_NPKTl2FFeNyobOghkMnjS4NWPw8S6D0PIBi710LeMWDxv0OLffsYKKYLUfSdgX_h-XDKkfY7-4E5W2SyTOMOeS_LAXAd-XPI5-ZPZJG-MaAD2z-uV)

  - 현재 서비스는 exercise-group-blue 인스턴스를 통해서 제공되고 있음

- exercise-instance인스턴스에 새로운 버전의 코드를  반영하기 위해 인스턴스를 실행

  ![img](5-aws-deployment.assets/QeMjpQPs2BzSeEMzyYUMao4I-WG5ROxPBbB3p73aL_z_r2ktdwYMKNEtJSiegHA7uHHBOnazp3e29wKbQUGTod1eW-4ZLdXFiY6xI5TrLKeIdbZevtLhPxQfBoyjcB9MlbAtKTVF)

  

- exercise-instance 인스턴스에 SSH 접속

  ![img](5-aws-deployment.assets/j9ZGLZpKgA8_LGoGYTedogRDn4GtpHd8nQD0gTWpR9_KtylBg4VpOs4ReU-p1G8Vpoawy0Q42rSLwuEqlEdAnfbr9ajBV8KKn-HVHYwyO_pzV_B2k25UBMJA1PcGHv1DuuUaqne-)

  - <a href="https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/managing-users.html">Username 참고</a>

    - 각 Linux 인스턴스는 기본 Linux 시스템 사용자 계정으로 시작됩니다. 기본 사용자 이름은 인스턴스를 시작할 때 지정된 AMI에 의해 결정됩니다.

    ```
    Amazon Linux 2 또는 Amazon Linux AMI의 경우 사용자 이름은 ec2-user입니다.
    CentOS AMI의 경우 사용자 이름은 centos입니다.
    Debian AMI의 경우 사용자 이름은 admin입니다.
    Fedora AMI의 경우 사용자 이름은 ec2-user 또는 fedora입니다.
    RHEL AMI의 경우 사용자 이름은 ec2-user 또는 root입니다.
    SUSE AMI의 경우 사용자 이름은 ec2-user 또는 root입니다.
    Ubuntu AMI의 경우 사용자 이름은 ubuntu입니다.
    ec2-user 및 root를 사용할 수 없는 경우 AMI 공급자에게 문의하십시오.
    ```

- 새로운 버전의 코드로 변경

  - $ `cd /var/www/aws-exercise-a`

  - $ `git pull`

  - $ `git checkout beta`

  - $ `cat app.js`

    ```javascript
    app.get('/', (req, res) => {
      res.send('AWS exercise의 A project beta 버전입니다.');	// 변경된 소스코드
    });
    ```

    

#### 4. 새로운 버전이 반영된 인스턴스의 스냅샷을 생성

- exercise-instance 인스턴스 중지

  ![img](5-aws-deployment.assets/nyc6zuapKcdc58adPnS2tQ6Sa0dcijnmy3F9yrCuGMhoVPDJcbrLzsM-czFjMWqzZKl2kH0qD76A-rsimiveRIpA_nGquQsof5wTaI9cKq5-HuNubpqMcJHTiV1D4_hpZzwFMKw3)

- 이미지 생성

  ![img](5-aws-deployment.assets/LzWzj7gsW4jIElWoqzZ1C5M_Kmli1Qn45DgxujM-IHsbkj_APz2IHPKGsl4MntVUxn0bo9lo8jNvbTFPVFpVdRZvPqnmRC-YRDOKTF8Sq5TtnkkSKkfuoAVauiUqTTm_8M-pbmO8)

  ![img](5-aws-deployment.assets/sF7BelLLPWPSGaVXvZbCA51cMY4_hTtGHSgf_x_1QWFlDdSQfmeT3CR_3WKIYN6wsjppwXOgOXSSti8nKxrPJ6qygWS9CULb4LGj_r7b_3ZEa95N7oX8WL5IMnR5tAXGEKfYbPHo)



### 5. 시작 템플릿을 생성

- 시작 템플릿 생성

  ![img](5-aws-deployment.assets/ghKFRQ3zsxyQBcjoDioE-Nf8GQ-RV_Q5vMFC0k1YfV-YbqlN72or8oLfwiu-jtc9iqWsDu1hCdscx-Jf9snxRUantDe4Z9PfRolU_v6qaPaFTF6ZdNbbi69991Iye2gAFAij3tCq)

  - 기존에 생성한 exercise-launch-template을 기반으로 새로운 시작 템플릿을 생성

  ![img](5-aws-deployment.assets/iVsgpmhASXEq8w0ynaRgYRn-qGdlEdrA9wrhsJF7QBOxC4iRzo8Pi1PCwPDJ4OeZOuV8voJFENRTpjUiO4GodGsof9ivzadb7Bhm_C69eEc2r7ZzOwiL80sYV1Yd-YGsibk3lPBw)

  - 기본의 시작 템플릿과 변경된 부분만 재설정(AMI만 변경)

    ![img](5-aws-deployment.assets/4B3vz3wbaWUcMJJ9AIO7JAE0221LepIakZekudjGOV59YFh9ZmTu-JPAA_OFs1oexokYFD9F59kCBjOP6tglVCW5bxF_4tZN3TlIgEF4kakZEvTt9NQAI2jYr51_nPmauP2PexhH)



### 6. Auto Scaling 그룹 생성

- ASG 생성

  ![img](5-aws-deployment.assets/C58Df5LMLngaowc7uUowLs8AUCVMOX3PCO_fQiwTbCfwsgmzyiAt7JtrfU4hxu0rFwcKRkuIQDcHkIahukv0haFgF_A5wHubtMA90n0HHLIm3QQIBvwpXXdgAqpylQOMp24lO_Oa)

  ![img](5-aws-deployment.assets/ER0IiKtkaHlDQtnGoDtqvXeI6JHWwlatK9gi-GtdJNpjsTXOsaIycJECqgaTgqwFTnAn5QgKJxAcFklE06retavonlYU8QWBCVkL94_LCewo3bNv0LUrLEHd1y45tRvF_UIZImlK)

  ![img](5-aws-deployment.assets/g-Q2qfCQEVhY8dl7ciB18KyxDeBIDW6UA90qYfxBeIHDbKsCOyd91Tb5uaWjysE27dGFL85DY_r1U3QiQ07rDxlZbUKI4XxKDdFoAr4ElEt7hNEtMCPOjAterKPi6x4tpO3AmbIx)

  ![img](5-aws-deployment.assets/Ah9zvFFVeXba6FLQTyxBfCCmmPescVukeTj9jRkvBZ55shTC62ofUpsDB-2yrLOgZ7NNWG6Q47iJIgWquCzf_fhZMAk8deOw-B8FMWC3u0gFTydPiieXtvgtEgnoIYPIoJUgQZ8_)

  ![img](5-aws-deployment.assets/JbBChn_3eGpGU8gFYLwTnTk7gRNrU-cEoZNzq6TGEx_yHZKdHYiQKrM9D8HZL_ZBKKPODMNS33VsEaPjXBb2nNxiR7leoVuMt4mQPi44jpFy29adR-LsbdUFDzQ3HadLK8DLVShc)



### 7. ASG을 로드 밸런서에 등록해서 블루/그린 배포를 진행

- EXERCISE-GROUP-BLUE에서 실행되고 있는 인스턴스로 접속

  ![img](5-aws-deployment.assets/mLGzQJSkMXawgJ8yqKoKJWqmFziA4aeI7bHVPwD6ni8QO-noJ3756Wlza5JOIrRhWEIpnddF39oSUm_HDlJW76AtjgoOimnq1M90E1y8zZhHJBIIv7jom38Zy9pK_0lmGRafLp5O)

- EXERCISE-GROUP-GREEN에서 실행되고 있는 인스턴스로 접속

  ![img](5-aws-deployment.assets/H2JtII1XXKlrfA0pUKhNoWrlwlMz-jO9qaqRf2fFYkyfa4dT9w4EI_mc7wmUakcYxuRuQSpltR-HyQq48T3yVAORmvNqGYZ0cJoPADgMy09o6jHNAoUrjrJcRd051ZIi6uIbj5Qz)

- 로드 밸런서의 타겟 그룹에 exercise-group-blue ASG은 포함되어 있음

  ![img](5-aws-deployment.assets/SAWV_mrQCBuwL-23FxhvMBB8kBlGOlxx3ygUoUHc5EIsPnJZgKLQ5BeIEsN-T6MUNt2SfRElPB_ABxHoXQjy4BD509lrnB8K9F9V1NiEUk96vwt5KZ0SNS6g7r8I6-iUoh5wrz1L)

- 로드밸런서의 DNS 이름으로 접속하면 exercise-group-blue ASG에 포함된 인스턴스의 서비스 결과를 확인 

  ![img](5-aws-deployment.assets/rPXtwX3JDVAcZQ4fUWaxMivf4F4tzeZIHNKcsf7Wk7y2YwlZ3unphV_g6jxir2pD70dRaX3pQKb841uNijP1EJ7Z2ouycF_nEaB8MR-jm3_vboCf3-iXQVDfpff7-VlUA4NHVphP)

- 로드 밸런서 대상 그룹에 exercise-group-green ASG을 추가

  ![img](5-aws-deployment.assets/miOOyJHbDPFsDVrF_fD2IGVWHYnUtPA-Wa_U7FrofEQkSk21UIUsIXnL08n7EUL3my1rL5suSR-xTT9glXXI73YN-4H79_KeXZaLRjUMb4n8HEjBWGj3XYQzW4ZO7YBkiKY-fk98)

  ![img](5-aws-deployment.assets/-UPo8QDHiCTUY4HiIclcX7dK_PKBEeH42LSExV6kkURTMs54iMbLkYboc2h3q1T1HcmN4CyAu6GudiIr5rT1ib9uKRIxx2q7Nn30Utju89Re1tlRBMIUMNWBKf-2G3-5Q0fIAhAc)

- 두 개의 ASG이 로드 밸런서 대상 그룹에 추가된 것을 확인

  ![img](5-aws-deployment.assets/uL56gIzGHv9GAH02KNNsZkhgG4zdDNrSrkvcgl7mQdCbl7uD-CV99CmLHrMLPNZF6pwvT7Gae2Bt1qYgqclafLqI0mgMhlryfBNB3QC4Aeng_rF8sZJ0-AVDMpcReX9sCad66HfT)

- 로드 밸런스 DNS 이름으로 접속

  - ⇒ 두가지 버전이 번갈아 가면서 요청을 처리하는 것을 확인

  ![img](5-aws-deployment.assets/a_Pxh2q7X9iJn9d1tYb5L_4fj9e9baRcTuXRwrVUfkALIEHInmmrgMaC9x6h7jpQLhsbmF0AciRUHDal1rWuPFjt6qfKxGatk9vwSSiZlpgaJv-2F9cKTZQ7JzAgtxAn2w5t-WKF)

  ![img](5-aws-deployment.assets/vHbgrLxncUw2N87lcwCIRQa47MZxEXuPTA3k3J9800m_aJgb-EOJikhl0vegXoggMJXfYkAE0oZOMGhSYGjt8FoISkCxJXAclIl5i5qSK-dIzaO0SHm9CN8UwkL2QVQPbZL7LSCi)

### 8. 이전 버전(exercise-group-blue)을 중지

- ASG에 blue 용량을 변경

  ![img](5-aws-deployment.assets/GppSDt-XEqsvbVTkPWp1i5fRFwxAMcwp8UdiheL4-PBZnLJSx6q2lAd7QuDz1_GsjVVvG__YUqs4xsRE54-FSXYbGFQCu7pZpjNKULx7dddNefqgJSaRP03Zs_P8Dw1rZUN_OgKu)

  ![img](5-aws-deployment.assets/_LLBoaDjsKbTZRXanY1cvHNCNNsQMLP46OLR8nfbraEGxbChO85IfyTeAd4OTlNI3gwTBZAY7IqZxRRBc-puJixftKu43kBSXGQ9wnttfhJZm0qvAagsNVB5jgK-MvkhulQYmQFq)

  - exercise-group-blue ASG에 포함되어 있는 인스턴스가 모두 자동으로 종료

    ![img](5-aws-deployment.assets/GRhQHtiZSBVH12b6q1KiYFjV9YObjcsRL-x8F9M42-IBWuApjVe-KkxFNfCBbVaovoq99Pyyl5PHSOvSRoxe-SmQByvI_jWD6PtRDLu6DonKndm4xKfYjlEmo-CkU7hsQ6UeGHoS)

  - 로드 밸런서의 대상 그룹에 포함된 ASG이 unhealthy 하므로 모든 요청이 exercise-group-green으로 라우팅

    ![img](5-aws-deployment.assets/12OmML4M7bF8nn0oixm76g4r8uZpIx-Ij0lJT7l8S9LKYsO0DYHtAtnNpDPUR7hxuKtDS_Hik6DvJGEx0_zM4wsu9nuIdTdtSZ1YBevG-f0pGA816s3M_Fd4v6f4VwFu3S0WqoE0)

  - 새로운 버전만 서비스되는 것을 확인



### 9. 리소스 정리

![img](5-aws-deployment.assets/yTuI22Etnnq2DVylXCw817PPPwFuBsHtCXneoDN42_QJaYnQ57R8OyqrZgoYROd7WFyzCltQNFpn2aUrVpbSBaLwh6miNhvY6KCZKfpmGWc2zD_Y7ZPTLKL5Tkb9Kuvvo-HnPcP3)

- ⇒ 인스턴스가 모두 종료되었는지 확인 (p126)

<br/>

---



