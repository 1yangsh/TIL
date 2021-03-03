## LAB1 : Managing DNS Records with AWS Route 53

1. 웹 서비스를 제공하는 인스턴스 두 개를 생성

- 서비스 인스턴스 구분을 위해서 index.html 파일에 인스턴스의 IP 주소를 추가

- `sudo vi /var/www/html/index.html`

```
             :
<h1>Test Page @ 해당_인스턴스의_IP주소</h1>
             :
```

![img](aws-advanced-networking.assets/4ODpbVWjOI3xi4PJv7wTyFqJ5pOPckteIi4LmbmejrPgzhf0r87uFN4fxY_FAVj2tZf-IGvV_MVQe_JK5OxCzdJ8WrTcKWFSQ_GHqvjCp0q_MTWvxupAtAHt5bqJS_DwcYw03JdS)![img](aws-advanced-networking.assets/fbqbu3dLx8EN-SU6WbvceRuiOHK5S5TjEc6bNDi-TB6MviaUanauZMcBz4VcuvT8zNog2zKsghtu3siBFArha944nW_PgAfL1hJdGjfqhwpNyfIUEg5egGT8S_Vq1gmMgO4J3y8d)

2. 로드밸런서 생성

![img](aws-advanced-networking.assets/T5h0d-tbOyH-tBM4AZtJn43vG_Xbi9OXVL2OC2HbGQFNyNiK0g86RCtcowDQ0p6yrHC_OEP9E2JhCMCnXOwV5vvC5uKtTK-jH8TRUJhJ0mVWRR8EYNIThx6u9iq6f_ouNFPxTRfc)![img](aws-advanced-networking.assets/9-oBjynYgUbEOa15Pio4m4IS2Zf8cZYYRZ_8Z6I0NLVin8khnV1KF2zxsEYQRJnptdRwSFFKRBywPLOBtQUWbTQzmq8yv1JExkC0WcPtRWlsO_QIpj29rfIoCV35-_QI8s2HhD_n)

3. DNS 설정

![img](aws-advanced-networking.assets/53yZiaK9i-G0ZL8av2sdyv672VTJuWV-Mq8vysdxFJwoy5YpVfTda6YpjFjtqC2PxnHFuv5OY5iubAT7BtDt3e1vxPTPj1yErZlYc89otVHJBwBiirG4VveBPdlSpT27cljTN1Ds)

- 도메인 주소로 접근했을 때 서비스가 되지 않는 것을 확인
  - IP 주소로 맵핑이 되지 않았기 때문 

![img](aws-advanced-networking.assets/mFWt8eVBateEtymIi_W0uhOIPmabGaSYRAqvPo8dSWeLxz1lTLv-UhUQ97QPPE-q4Y5bp_WIgzVxIXl_JLpA_xfb7MFa4EMFDdYWIOZnUySmVLWzHpqOE3ClGDL0J6-0ghbrs--2)

- A 유형 (Address Record)의 레코드를 추가
  - 별칭을 이용해서 로드밸런스를 연결

![img](aws-advanced-networking.assets/OSVp6htXRwQ1KOFmnCbtA2GKCkNINknwrC5dC_5iwtRxFDstApvxZALaLY1_gyKE5YGwNRhB4bqIcmsWS3XqoAfMRhoq_Q2GS-D7JBZ4zol4idwZvZq6ojMWQ6XvH_LYUPCRfB3a)

![img](aws-advanced-networking.assets/P8rWarevDMMc3X_mrjR6E9wJpstjETfqudpdROaITTfTrNgIGFBROz5DAE20-Sfv0C6i09Ub3okcTSy6_zcLWP7LVly5NB7p_jYcSsKeELFSvQs_e2sWTjXW0fTYVrUu91b-ris3)

![img](aws-advanced-networking.assets/NZMNEpigSHIJ2BU_8vwwrP-zGdwUJcLCxYa4JrFxGys85oZnK9-TwuWp3uxp--Ipb-jO4LvDS5lYljO6NMyHVz-VG2PqeOhPuCJZ4Xj8IXJGS5AKFAedL-Cp7VQrol-e0Hev2ds1)

- 도메인 주소로 서비스 접속 여부를 확인


![img](aws-advanced-networking.assets/Hgtzyg2Jn95jTAKC9eoj1SnDBTSJ6cgYZmuv-CyCZd45OBlT9C6wS16ohn6t9J5g92f12Z70U0W5cTwfSbid-Rzewf6PW_i8duNWydcPsbbmTPiDh-vOncjiJVtGrpFld4C2svVZ)![img](aws-advanced-networking.assets/cPlGskDDDKr5zMtxn8akFaAHqQgiLxMjxIbKMhqp9UAr8Qsci8ZOTL2ds4YiFvvY6I0j152dPqAytsZDgeLcM77ZPOudD4SDwJKErzkKz08U-4grcbHXU9pc3KJTXE_hDMMGtnO5)

---

<br/>

## LAB2 : AWS ELB Connectivity Troubleshooting Scenario

1. #### 인스턴스 확인

![img](aws-advanced-networking.assets/IbymnukCwGuSbsmUJvw_k_aG-ZHejpc9hOkl-vm3BbPLblA_qO635R5wMS3CBHyeJR7iZd0zJCDeKaZrPo2BxtCAojhVJN2TmtPjD8duT3Z_QjETC1zFedY-T6W8B0LStuWqv81f)

![img](aws-advanced-networking.assets/azccfbHL5M1tmYjqFEQzDsXObB3Wvm5TAHgu1C7Os5cAaoI69QKwTqInm1OCLpvReUVdsjM3I7HB8PxsZSgswcGU0susLHzTtLO7QGqYjngpuD6z7UQuNoPJAM_gliTAnHPaEhQu)

![img](aws-advanced-networking.assets/ZYVNujX4AUInJ6t2jvgPgnTFRkoU8rtLplYsocBjsT7J-3PYNHH7mzTNys25vv2EsgtWjSUywnLhlOTcCs1gpNkVxIMOJi7Xlxcjl8sHFYkgYBn8ad-xp30GyrV9-JInCSvmi-Oy)

2. #### 인스턴스의 퍼블릭 IP로 접근했을 때 서비스 되는 것을 확인

![img](aws-advanced-networking.assets/dWC_FlaLoknChiqMtmNS7YhRI_aRnIyDdr8IGty1qI4Cp2p62wbHcUKEI6LaZgvjnmUS1JXjz14dBUq3lMfFHtfd7n3EznNffORJO0TwGjjCrq6-kYty13yrWbqnbF6v57rko8lS)

![img](aws-advanced-networking.assets/7oZKss7Tm5jsjpH_O2aNjF1L3EYk6HEfnb5BNmnkYF_3LHCoO7nqNbr26fsmhaO7_8Tm8TitpadCnwtdrFR1PSGl1SR9EHjygOAKjPPHBDtxcj7zW5Zs-AvMz2fwOJrYGZq6ynao)

3. #### 로드밸런스를 확인

![img](aws-advanced-networking.assets/BY8OmE4IIvpF0l1uw1M51Hwt_M0iuDb2auBvCpU1UsxwLUh3ySqnx6lYEBBUp9zo7ZfrvWuMfKI0adA8eRPj2SHMFLBkZVFGeJPzcf36ZgPAdiBEmtyjXUdnp9VtviUT1wDqRLR9)

4. #### 로드밸런스의 DNS 이름으로 접속되지 않는 것을 확인

   - 연결할 수 없다 ⇒ 서비스 포트로 접속할 수 없다 ⇒ 서비스 포트로 서비스 되고 있지 않거나, 방화벽 등이 차단된 경우(security group or NACL)

![img](aws-advanced-networking.assets/WZI4wlI_WEaMk48fgfEV9N0QO0pWFSdw_CwOcrqIH7WNc95qM3yUuHpqr6K0ooHcSATPQ9Kl6q39PD6Gr36TzIynfuVmJEt0dZiyN_uxMss3BoI31qzYhggh5v3M0vnvjibeLkYY)

5. #### 로드밸런스의 보안그룹 설정을 확인

   - 인바운드 규칙에 80 포드가 빠져 있음

![img](aws-advanced-networking.assets/O-Dp_gYwhzFHzWXsJeg6eEAFxZQMgUqs1_ZTj7VHmWUPmq8SHuBWYDQOF2gfpav9kg4_2lxvMWvfUYB1dhmrrMpY9whYgbEntETDVikfFNR-Ef-bk3cqFM-kOftM2tKvcBRB7U0E)

![img](aws-advanced-networking.assets/eADJwAuVzsCJQyNt-kqZNyTlpEhxUt9LlMA0tRP8zdaTM9hm3lcgFxSZ0c87wckI0hULX4HPf1gEOhi85Vt4JV-CU4ZhtImdNDz72tbsT9mAxjXfW38VDsL2x5hkC3GNijxk66NZ)

6. #### 다시, 로드밸런스의 DNS 이름으로 접속되지 않는 것을 확인

![img](aws-advanced-networking.assets/Zhf7Fdi6gwcm-KDSxcv_7Lau7DLjk97yvj51USgftksaSVE_o0vgAcmLsfEoATstbVasKnAK95bVgO4E45qlUwcFyCC3jCMrSpyfazNClCvuRy5v4YsWFDGmuS3n3tCfcHIFGs4D)

7. #### 인스턴스의 상태를 점검

   - OutOfService 
   - 인스턴스로 직접 접속했을 때는 서비스 되는 것을 확인
   - 따라서 헬스 체크 로직이 잘못되었음

![img](aws-advanced-networking.assets/CMFtrps3BiSRUI61GGL_LeUP8Ew1TrS4uRGsjJ7SC7lBWD5oeyT8fXd5pu-ohxPbPiAuE8pGnSIxfwQvxLhREtKQkQko4bZwzEawOziZMBstJuF-EjgqGKRw8m1BBMX9jIUd0Vi4)

8. #### 로드밸런서의 상태검사(health check) 설정을 확인

   - 서비스 포트가 아닌 다른 포트로 헬스 체크를 수행

![img](aws-advanced-networking.assets/n99TkcSbYmC-FuO71RRR-yMxu6I0R4JgoyozgHhuA_z8c4xrMAqnyvFGfz6WuUdMTxSVgq6Hscfc-TpeeLbxEWWqi07xwLS91aYttCWVCSCO5XdJcNDq0d3bxznQw43Unrv32fj_)

- 포트를 서비스포트(80) 8000 -> 80 으로 변경

![img](aws-advanced-networking.assets/7dp1WlU8SVXBFm7We622kwT0PX5mn5-jhhJ6-2W7qrIGoNK-aNyx129Mx9kp6LL4rY0CeVux27P4CB7IKR1w9P4fqc-XScyAyHUCr8MPxWZU6skMoVHKcvnao9_uF-ELoROq3TtA)

9. #### 인스턴스의 상태가 InService로 변경된 것을 확인

![img](aws-advanced-networking.assets/W8YGfVNBKMyt1sGKMvqzjjJaI8KJAr4a7wZcN-83x0OaeGkBSiEYGAjtA7EH8fJSjqk3wj4qRpT2ta6n-2awL78vUVYUhIste2z3pYQSO_iooRg6a5FmLVKF_la30XrA0X_ifrku)

10. #### 로드밸런서가 정상적으로 동작하는 것을 확인

![img](aws-advanced-networking.assets/wEzY2yKKDtrfh_DGJPeusHLm6Qs7iU_pcfoTipGhAtg9YMIHR508eS4jDV1VEn6jyMFVVWluHyv_zV87n5YLkoiw7r7zTtvnrNi13jXToQ6uXAIAYXZQ30aQCocKuTjEtDY2NjMX)