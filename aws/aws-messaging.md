## Amazon SNS 

> Simple Notification Service

- 게시자(Publishers, 생산자)에서 구독자(Subscribers, 소비자)에게 메시지 전달(전송)을 조정, 관리하는 관리형 서비스
- 게시자
  - 주제에 대한 메시지를 생산, 발송함으로써 구독자와 비동시적으로 통신하는 논리적 액세스 및 커뮤니케이션 채널
- 구독자
  - 주제를 구독하는 경우 지원되는 프로토콜 중 하나를 통해 메시지 또는 알림을 소비하거나 수신
  - 웹서버, 이메일 주소, Amazon SQS 대기열, Lambda 함수

![img](aws-messaging.assets/UCO4ziddlNkHR1fUr3o4nOOjT2N7mMAgxp7d4NTeVSlYIx8hFIEGeA3FIO0rbhq6UKgHs2KANX8i5FT8LG42ac2JdRJ7AJImP_iM1EBjh2Zf7Mcetle7ErXjSbMQB-frbHmkOVbd)

![img](aws-messaging.assets/AtW5KCYTT2G_l-Mi1dMofgIlLBrjjLwiSb_2jk-UUCQjKSeouErzQy_UOOqDhdsuJNPLSdmvtbF_V_xmTphqZg3A_szPwhbTWuSAX0LZavBin9z_8vMlvSG19UJNgE_BDCW45PIA)

- 주제(topic)

  - 통신 채널 역할을 하는 논리적 액세스 포인트
  - 주제를 사용해 여러 엔드포인트(예: Lambda, SQS, HTTP/S, 이메일, … 등)를 그룹화할 수 있음

- 구독(subscribe)

  - 주제에 개시된 메시지를 수신할 수 있도록 주제에 대한 엔드 포인트를 등록하는 것

- 게시(publish)

  - 주제에 메시지를 등록

- 사용 예)

  - Fadeout

  ![img](aws-messaging.assets/MiDVJ0N99mrREPviVpZUlEIt4kVwbotQG7Cc7gdhw-qTJiWI5yqIqJ-EmnQtPalnq0K2tTkPsOFWAc_2r-lVXnFnA82Hw1LuXe2JNA-pKbIfyOqGimSvkIC4R_0l-5JbcUwlZKTT)

  - 경고 : 사전 정의된 임계값에 의해 트리거되는 알림
  - 사용자에게 문자 또는 이메일 등으로 알림
  - 모바일 푸시 알림



<br/>

---

<br/>



## LAB1 : Creating and Subscribing to AWS SNS Topics  

#### #1. 주제(topic) 생성

![img](aws-messaging.assets/UW_b2zMfye7JkFUaqkgfutNSCZB0e5tHs2_TBdY20bf83zFXMLzVO_NE-sZYtvNmwnuzBmqzu0j1qPPXq106IUkzRFlQgCJ9oPO54-z8hCv8tu1HLk7cj41Ka3moasswnTStVcZ-)

- FIFO - 순서가 중요한 경우
- 표준 - 순서가 중요하지 않은 경우  

#### #2. 이메일 구독(subscribe) 생성

![img](aws-messaging.assets/MNwjyMrfYmQnMc5ol3M8ZjmR3-jzAmh8T6xNjl6qPHfBHKi-cdGkgolGpDauBxrHOfKPa4k8pBdh9P1p_hwivV5uB6UfG3NHqy8QibbUbFMDPJm3UE5Z6Ec_puAZvlbO0FJGYdaB)

![img](aws-messaging.assets/ZGtAVXnaMqY0Dcm2dtj1lIt7kqCeSZCBLvsuLgrzuEKqhqTpgxIzeIXe7b26V4IpbNCgxYm-RIOIMMdZ42_GvBujFWjY8a8EZg2NW42l-i-_ByLwDPDbfUtb9FR1k9VZNJRmdnaR)

- 메일 확인 후 confirm

![img](aws-messaging.assets/vuZAHUe5lesBbdsw0SxgU0xRI1pKA-rHilKQbdjgA55wwOiKNEIZgCSBh9KA3LSI17ytKSaM99QVmJgmY-dG8yWFQm44NZ82h8qofEBqG-nVhSKJWAqXyNIHYGDnlQ5sbWGhHcPL)

![img](aws-messaging.assets/q1nTKbvuyyFTOwRyFhfidhVDZ5-D43yD0gRXEUPG1pBGyTfZwLDIXfcyIyr4d5B45EELrMTj4R-2gyAWxa5L0xSPq9Jz9WIrxO4CGuiU352Om3Q3WeG2if0ay7QCAVCE5-ZuAXuF)

  

#### #3. SMS 구독 생성

![img](aws-messaging.assets/KoZsPsuCm9G1K9ivWssOCHo-pHYrK1n36d-UiLZmr70JpKRYbWSvcF5yAWYC4fRNxaiPyCvguh6jJy6vCwLZV2USSC2SHtjTvFNjd-sNnK4okOzmSOuqF6aPbIWIRjX6JHNgvqTQ)

![img](aws-messaging.assets/ekPdG8pf4LJSaFQ1YA7qrlDLjiowGgV3OINfYao-ziW5rxOwBw7eoz5M6gUlO6e28wM0SnAW7phGq7hwwSMpFf8ToOLyMWMkv24EG7QQ3It-A7JCxiZVX8fncfDsrP_wB2y105hf)

  

#### #4. 람다 함수 생성

![img](aws-messaging.assets/cIGD8LVjdkHUHsdFeJ-PTp_kzjY8DDsfgWMa4BUWLUSAKnTTUh_9hbaAUW_K1aTDiddZcq0XCd_SKaBKZUJJXP2nAXzKSFlhgID-b4q2VAqDAlAsj_EapWl_L-8oG07TOcbELPH6)

![img](aws-messaging.assets/B9Q9tER_lrVFl1a_7_DeWPD7SFAZ0odArZ9TRCvuVPU7VHD7y6XeJjf5brmpnVh1Ke2HXMp_bIxE4qUlyrQcdOhH6Vdh8MSXTbXNRauXB3ElzeU_KwmhLIiKbxPwS-txkuXkWRy-)

![img](aws-messaging.assets/YvToJHVthdcjdL_T86qUDvwXyCgmcSO9kuJhCgB2Pb0gPvh-dGW-oyeNfKL7hAaImeqn9J-k9RtjZOezXpUahdW_089deyilDPyfS_DtvtOpFM7MLd1oKbjcFUAC1SKVh2_sHeTp)

- AWS Lambda 함수 핸들러(Python)	

  - https://docs.aws.amazon.com/ko_kr/lambda/latest/dg/python-handler.html
  - 첫 번째 인수는 [이벤트 객체](https://docs.aws.amazon.com/lambda/latest/dg/gettingstarted-concepts.html#gettingstarted-concepts-event)입니다. 이벤트는 **Lambda 함수가 처리할 데이터가 포함된 JSON 형식 문서**입니다. [Lambda 런타임](https://docs.aws.amazon.com/ko_kr/lambda/latest/dg/lambda-runtimes.html)은 이벤트를 객체로 변환한 후 함수 코드에 전달합니다. 이 객체는 일반적으로 Python dict 유형입니다. 또한 list, str, int, float 또는 NoneType 유형이 될 수 있습니다.
    이벤트 객체에는 호출 서비스의 정보가 포함됩니다. 함수를 호출할 때, 이벤트의 구조와 내용을 결정합니다. AWS 서비스는 함수를 호출할 때 이벤트 구조를 정의합니다. AWS 서비스의 이벤트에 대한 자세한 내용은 [다른 서비스와 함께 AWS Lambda 사용](https://docs.aws.amazon.com/ko_kr/lambda/latest/dg/lambda-services.html) 단원을 참조하십시오.
  - 두 번째 인수는 [컨텍스트 객체](https://docs.aws.amazon.com/ko_kr/lambda/latest/dg/python-context.html)입니다. 컨텍스트 객체는 런타임에 Lambda에 의해 함수로 전달됩니다. 이 객체는 **호출, 함수 및 런타임 환경에 관한 정보를 제공하는 메서드 및 속성**들을 제공합니다.

- SNS topic notification을 통해서 람다 함수가 호출되는 경우, event 인자를 통해서 전달되는 값의 예

  ```json
  {
    "Records": [
      {
        "EventSource": "aws:sns",
        "EventVersion": "1.0",
        "EventSubscriptionArn": "arn:aws:sns:us-east-1:{{{accountId}}}:ExampleTopic",
        "Sns": {
          "Type": "Notification",
          "MessageId": "95df01b4-ee98-5cb9-9903-4c221d41eb5e",
          "TopicArn": "arn:aws:sns:us-east-1:123456789012:ExampleTopic",
          "Subject": "example subject",
          "Message": "example message",	⇐ event['Records'][0]['Sns']['Message']
          "Timestamp": "1970-01-01T00:00:00.000Z",
          "SignatureVersion": "1",
          "Signature": "EXAMPLE",
          "SigningCertUrl": "EXAMPLE",
          "UnsubscribeUrl": "EXAMPLE",
          "MessageAttributes": {
            "Test": {
              "Type": "String",
              "Value": "TestString"
            },
            "TestBinary": {
              "Type": "Binary",
              "Value": "TestBinary"
            }
          }
        }
      }
    ]
  }
  ```

  

#### #5. 람다 함수 구독 생성

![img](aws-messaging.assets/WsuIMzPVNovYUwZM-OiQr4onqWc6jHNelEY9Gbz-KSJgA4wsA571o97AaW-qiUG029QQjiPy0SllM144hUGEkq-cLSmmvdoQILuvO9iGIDx-Ngof35hs-BPx89dnWyL_wXf12V0_)

![img](aws-messaging.assets/amXo2mIsgLuJxe1x5RaCJirgEUgVr7wu8ftmtGnBJNmE-D4jq3FEM_13Wz9imMcexTGdWLEPFJ3IzDItHSOtiCIiMDrqIOdHKnq_cq-gjYf6jZ5RuLUwL887AMQFvL1x8lxZWP18)

  

#### #6. 메시지 게시(publish)

![img](aws-messaging.assets/r56sdP1FOSJfocfcct0J2EkIQN_8kvjqLj9abIZD3M7p1wD1lJcX9dyR_SZ3ZWjLqY4RildCo5AP-6Z1pHH9q1wvcO9dO0Cxnv64WsfrQqbPTSkGmB45BXzcxgqTva4QvSAXrvZg)

  

#### #7. 메시지 확인

![KakaoTalk_20210304_104004060](aws-messaging.assets/KakaoTalk_20210304_104004060.jpg)![img](aws-messaging.assets/7_jpMRg6gKuVaBNF0wh2-vZhv8LzR3al7RHsZ3Np2giPFPxY2-k8cFrjlez_n-xq05xX8DVKtZclkhdqlnDr-YkVDDB5E3hAiw4UFhXHs52XAAJlZY-pgnbZsRkaM-tjUvrKhyvn)

- Lambda 함수 구독
  - 함수 실행 여부를 로그를 통해서 확인

![img](aws-messaging.assets/GVDcd8dJ-nVe0U6B3H6U5tY02Mp0H94-7prUDSoUTN_x_ZSjBForTxGAqbsN6H1glfPAdnKQa_aqbFXRvlx8pr4EDM_g7aTNMqfYsTICSvzkZTeeLJkCG0nevvMgbqRbD3Y2Yko_)

<br/>

---

<br/>

## LAB2 : Configuring SNS Push Notifications on S3 Bucket Events Inside of the AWS Console

![img](aws-messaging.assets/Kz4RpKu2O1ypFRXq1qvmFPLsmxOwn9AOPbfA5wWyW1wtRvRTM4X70-8dX208Hp0dcOsfqGu8c2e3AkBG2ITZKIziAu3R92JkAiSlLXVxs0MDE6Twa8f2D2nCJOXLF190_d-iauab)

#### #1 S3 버킷 생성

![img](aws-messaging.assets/1n9qEYwSnW4OilAEPHM5b08YxWCIq6TH3vWfm6ugqSibC1W5-daUT_H05qdmqdW2qY4fV_zHsJok8aK2R5CMGKdpSOJdZMSAAg6XIVsXmfQKKIkCmvdSLoiwX_xyl5D-e-jrCWn2)

#### #2 SNS 주제(topic) 생성

![img](aws-messaging.assets/YhNOyG3zEWzj-faBgb2q5qQhuBbaomRepdS1NKAPTzamL5mO63swT7IaCEUyy_hw-NSdH5Pw62O6HhW2WCubPD0tgt3q0UKW4bH716zEO6m1QJYyrKg-Pwpkd69Mw1EGTK_33ihe)

#### #3 S3 버킷에 알림을 추가

![img](aws-messaging.assets/g7C75LrgtT90wNXDtWxv4NuTk4kIsMrc4tyqVL3Y1MqppaIzwPe4dskyIs3PtzagGu5MzQR4MqOwPMHhbcR7O7FCunQD6-_VeeGAAq5mKMyZwAcHNR6TluhJ1j2PeLilShMiVrTp)

![img](aws-messaging.assets/ijH-2_nKQo7_HxwLwC2malVHhE8mFzsSuWLxFUAt0_TMPDFezwG2AEmnk-I3rpkt5C8_Byf0312O5-HuZGZKK-IA_XN-wqnSBE5jmKy1U071qZqyKhwTaccKFOjMLiaUu4HTm9sd)

- 저장을 누르면 알 수 없는 오류 발생
  - SNS 게시 권한이 없기 때문에 오류가 발생

![img](aws-messaging.assets/M-6Xqag0PEfx0w87zq8t6izm3QauL9BF7cA_MjwK9giv-yOpAOl8WlDGSzH51aBotmNZDTzw0b0vk1FsXHuVX1rkSg_E5fNoEdIkWffGAICTakxMCBhHLPdFQfJ5neRvDoW-0xLm)

#### #4 SNS 주제(topic) 정책을 수정

![img](aws-messaging.assets/2yBUyio7OVcrh_ZpfSYVumpr_HvJCQ7pFQ0tmoxgTBNDCjwOumrq5R-9q4Cya2sIHFzmyf6QK2RURPo_mLOLee9uX6nbhmYdCK8MzyWG6QNmW19o-gQQMhVsdZ5KcqkseXQFuSMX)

#### #5  S3 버킷에 알림을 추가 (#3 다시 시도)

![img](aws-messaging.assets/MmDTod7_MXqy7QFcD_Uq5GOXkT-Yc4e6Rw-hOObQk4UWLfSSVWdLji-U3VqnPbhZfr4y2k8Q991ZQjsfFzSdKDA5G1s7P7IdHmd0aFUVvxgg-MmGooaE-uqljAmjPSTN8uSEPaRY)

#### #6 이메일 구독 추가하고 테스트

![image-20210304133257864](aws-messaging.assets/image-20210304133257864.png)

![img](aws-messaging.assets/W723BRtdkMM56DzmsBsETDpVkI2fRt-L-Q0lnNiL-Z4iXpPvTMI-1x1GSnOxiPyVdOR0OPh0laEKp2CXv-17JXZJsGKvl8Q8EEIzXvAmE5hJ776-phuED6PIxJvDEvgaeqksu_GK)

#### #7 버킷에 파일을 업로드

![img](aws-messaging.assets/pbSLbb4hwMD4Enyo3pUCcR9INcx4MXdm3AnynFR9yn1-A7h2-CN2S2zQvDooYobZUhRNqcPhqjtU-1DBPn-nIvJ9eboeQ-oqZVeRayVY0mRpX9R5hG2QCGZQ0e_ZwR1bWUpPn28W)

- 이메일 수신 확인

![img](aws-messaging.assets/_hWO0wWczZfuDdJfD13rkJC-8ZhxxA4066zHC0M5NG1Od0MeTLtUzouDNpyBFVnij12QgC0gMzoHxHA1HBSjRrpVA3uk2rmvH8OF7i5tbNqN2P22QuLsalcsxN0wmzg1kFrHJ4xt)

#### #8 SNS 구독을 생성하고 테스트

![image-20210304133557822](aws-messaging.assets/image-20210304133557822.png)

#### #9 S3 버킷에 파일을 업로드 후 문자 메시지를 확인

![img](aws-messaging.assets/R4KDGRVLC3o_lax06lxpw9fWN6Lom-ge3IaZ4yDXeErZJEv0NbUCGgxbPXe5b4GSsm5IPa_rm2ybHniFu6aZKKEawe1gMsIsQ28NW4AUjwa9xDhBmPI6sVNKCCBQ7yNoonrvPQgd)

<br/>

---

<br/>

## LAB 2-1 : LAB2 완료 후, 메시지를 로그로 남기는 람다 함수를 만들어서 구독에 추가

#### #1 람다 함수를 생성

![img](aws-messaging.assets/yPlERUneFVqiydY0mzkS3jDgqDqobv19BhBzvqBFMc9e1wjLJuGYzGGBq--XJ3-kEl2rgWIok0i1Zs1TNKYUpF4eDvPRP_FMN22eqzefpxrOExDZyyb7PPz8Pm67z6Zb8a6vCE4f)

![img](aws-messaging.assets/HURZujG3x7JV1p1S5urIJ234JAF0vZj03-jDLUZPIrJTkm75pVGW-gtkRLgKfo2a6h14YXHtF9VOFlL1TU6pIDwx4YOmIwOeEgPnZNOMKm1Xu6j3PzMmm6GXDCeKxlsFtqYk2Kp8)

- 람다를 호출하는 서비스에 따라서 전달되는 event의 구조가 상이하기 때문에 확인 필요

![img](aws-messaging.assets/4Jlkl3NC3uqq5vS63ODGAiLzoIOHrgaUoJSkwesB-SXSM6-zeMaRJS26ZeR4NJWxTDuqg1Sp1YjTGXS26RXJztG1xs81UJWrHWDnYBz3hrgfsOHqzraUYYsWqP88EJMWvTendc6q)

![img](aws-messaging.assets/XNxZC2IohXGXT-vA98L5fF12dyZWjrZC2eHaXHEq07wPl6RP7UgPw0uM6hk6twXUsXZWoAfUZH3GziaVSrjgxBA81OQNBftZOvUddfJkPWx9AUVSHxZS97K_Gk5hYFFNpI9kSpAt)

#### #2 람다 함수 구독을 생성

![img](aws-messaging.assets/5cMeijrz_fqxiwwD7WQkWQiITailVYPUnRTbmB-ILfnGCCBmTXiP6CJ-Jkmdki1A93_W7e8oktiRdcM9atOReFmteZcwWzH3xUSMMbxhHcEtY1y4tBPD3B5ihb8MAEmHaRmDeQj0)

#### #3 S3 버킷에 오브젝트를 등록했을 때 람다 함수가 호출되는지를 확인

![img](aws-messaging.assets/eCwpZctaeDGhKs0H_VDGLF5JJ5JJTP81FN9gXS4mt4wByO4YWPSY1WJw6izCIzGdNSvEPktKE8l2IPAalA6r6RTZiCFnWzkRjDmO660a2hkyh1vHy7qASmZJHpnZwgSTnqo0J1VR)

<br/>

---

<br/>

## LAB2-2 : S3 버킷에 오브젝트가 추가되었을 때 추가된 오브젝트 이름을 로그로 남기는 람다 함수를 호출

#### #1 람다 함수를 생성

![img](aws-messaging.assets/mYxlXjR53_DBx2DBsuDUssCzIUaFMaZDY9dcBhuUhF4B7zDH_EWUoNqkmjKWRVhB2f-RSYHwA3bNG1v8bkUKSV-LcYG1NiO0Zkc_lyf0JnuuGRgY-U0DXIHmTT8NHQe2W2y0T_VM)

![img](aws-messaging.assets/u2sXkVOzPjbZnLaEWUFRBlLc4b8Crodt2c7JLOOQIWCEjITuj6kBFAxeg1veg3GfMimk4CNerj2a1xzN5Pv9g9I62h13bg5iaEJT22zDLa6RLyFKvQ_TFBilWOKbcfajFT04gCoW)

- 이벤트 템플릿

![img](aws-messaging.assets/zO6jRLlCnogNa0oJxaSSZsFNJl8qaODpuO0aBLuZPFhYsFG7hVoLHeR99UFR4sufoDnyoOVti-38PpR5GPZf8Zo0Q-N6DzJTSgn-v_hLxUwh9eNDfK7kBC5BVGbgCMKnNN7Iskut)

- 업로드된 객체 정보

![img](aws-messaging.assets/VmG5fhi-0DXzRoPzy6eEU0nY_081iNjV71o-tCvb0mE6tuky3Ob6dTLaQwfT5zN3W8fHdYc4rEGBHRbl-kfoqlfSm9bj8fQW27umnjQu7A9sFpxthl6RpQmh8nP5SYXYDR0NutOr)



#### #2 S3 버킷에 이벤트 알림을 추가

- 기존 이벤트 알림을 삭제하고 진행

![img](aws-messaging.assets/OzgaDJ3JI7FjXhn3vrs1bwySCY6phY2Cm5uBuqJVphjP9gIwhqVj5YJgnT5hIFJGDQ_ygWUlBO-5JTJU6CADJNRe7Zt1Kkc9kBqP71oE4nwf5k4o5tFr4M_bWRV3hOEcWwZv5NFd)

![img](aws-messaging.assets/6ktsyqXXyhIXEftx5ZGCTYq33isYkZKAeVuJzBT3Hk5K6wGD3MkK4fMYkcBee2ZoYUIDd1FF8Ce18Jrw0PkwT9K_3E_uJ6i0BMC49nIteV3svXAQfex5Tj9-3vTu6urkH8ePknCs)

![img](aws-messaging.assets/NsxRDge6uKQCk_Xrkmr28vfpPze6TDpzrt_labB9u78_ku82cuvwZl9b8CGilyemcs-eebFICvWvD4tt2iXBo1GrQJzXbBc5oEUn-b_N4BawzW6kF3uubPtuQvTSiuSPoPOqjKud)



#### #3 S3 버킷에 오브젝트 등록 후 람다 함수의 실행 로그를 확인

![img](aws-messaging.assets/b-au8sxEEBvpFBfuCDTUiQHTW78e6i72nwrAPBgLSzaqdyP5fx3mak4DSf55A4or7cW643YNfh9fnAfNiXcH24fWKwOhi6pc0sh80890ThVuhF3GyeYN4s2UYCwsAzvgifNtyLZi)



- S3에 폴더를 업로드 할 경우

![img](aws-messaging.assets/86MEruZoGbPlL1ONxNdgf4wD0HDxe4SrIyf4BH1JvRh6esNgXbNHclwS3yiER7IOCniSPyoAV8r8sTPyI7vIruIsRWz7Zv6gjTmqb9ors92TilNl0IfZ46vBTauczFNXol28zzde)

![img](aws-messaging.assets/wsM0ua1ZxkFE1foyPOcMlB9fojQ3VC0FVw5P9vWcwwpWb0TkJ7QLDeHYGnNZQ3Se9ba9mPgfSfATrVxKwMG0SUJuJ3aTEh9JsgFUdUAItAfWPkcSTToUMMZtpS4mrXjmAAhp0ABX)



#### #4 S3 버킷에 파일이 업로드 되었을 때 SMS 문자가 발송되도록 람다 함수를 수정

![img](aws-messaging.assets/O00Llvx-np3o8xOSqorxp9y0E4b9z7EgRq29nXjn3NpeGiiC-qFDLoxfTQSgvVetiSWTfj0JEURsjs2u8DisWRx1nXbnfg6K5Ho81tgBVYtTznEaLm8ev24Hy8_z-pavqC1qZuI6)

- 람다 수정 후, S3에 파일 업로드 할 경우 오류 발생
  - 해당 람다 함수가 SNS:Publish 권한이 없어서 오류가 발생

![img](aws-messaging.assets/kC7EAF4CZnyhosOdYdzWdDriHczHMz6tg-_c_r1b9tHPVNLEB1gL6DedzTIin2CR9D8E90smXIifiDpJb-pvkRvwb4Vk7789Ln41Q3Qdw6HhXoaPz15bIHE2bR0HOpgKE6jDGh4j)

- 권한 변경
  - `lambda함수 -> 권한 -> 구성 -> 실행역할` 에서 정책 변경

![img](aws-messaging.assets/zfgYM3l94RXo915EuC167-vLLMHNEXvq62mMktAgw1B6KD42DlcNed12Y9AZu2U0eVNI1iRNfXPVbsJTEKPIjbnca2EQ3GfBqO2-e7mEyiwegxdlymhwagNaOJUF-PqT8Fcbnlrj)

![img](aws-messaging.assets/X8zsi6hOwWTys4R3G_BS7MKrYTX7XtT-808ItYsj8CSyNcZMN9p5QQcjr7HgI2dNA3UmEyIalEnFNDwrTzRlQE7jxqtEeJ66YHFJ6auvAKOaG0GRluHwlLcOVpFjnaBJulJT0LGm)

- 다시 S3에 파일을 업로드

![image-20210304141411562](aws-messaging.assets/image-20210304141411562.png)

- 휴대폰으로 전송된 SMS 확인

![image-20210304141528160](aws-messaging.assets/image-20210304141528160.png)

<br/>

---

