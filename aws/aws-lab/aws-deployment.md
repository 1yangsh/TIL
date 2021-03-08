## 배포(deployment)

- `빅뱅 배포`
  - 애플리케이션의 전체 또는 대부분을 한 번에 업데이트
- `롤링 배포 (= 단계적 배포)`
  - 애플리케이션의 이전 버전(파란색)을 점차적을오 새 버전(초록색)으로 교체

![img](aws-deployment.assets/XR3GboKVpCY0c-58P63r6MH3O-Bd3ObIRvfqA2IDGl3Ua3SDbF_6wcOAaczAK9RfcJNCgH2o8kyrYjXKRxMKr5SBROoFqMnhmoVCJ0cPPXzqkL7ao1MDbq8J2GeijtTVc5bc40dQ)

- `Blue-Green, Red-Black, A/B 배포`
  
  - 두 개의 동일한 프로덕션 환경이 병렬로 작동
  
  - 하나는 모든 트래픽을 수신하는 실행 상태, 다른 하나는 유효 상태로
  
    ![img](aws-deployment.assets/GatLYEPgdo_6RDXgAyJfrJvkdqsLVNNpRmCn0ly8s4Wh4J3MEmMJGYNBJmfVvR5ida9h51rbLWxjrNNjToHR4kwLbCNAvvMjYKWIKniGDSOGKo8YIg2tJJYyCBDvyjzDUOwLjvxk)
  
  - 새로운 버전의 애플리케이션은 그린 환경에서 배포되고 기능 및 성능 테스트를 수행
  
  - 테스트 결과가 성공이면 애플리케이션의 트래픽을 파란색에서 초록색으로 라우팅(변경)
  
    ![img](aws-deployment.assets/3mYmXebMCSYCOYm6AihboawgRgfnVRnWXBHwMN9Z_ZCu8h03CSSzIyghJ2BJbWFujOy_c4-XsVDN8AVwNi_BRx7C5oZZpUUtYD8oXXyyHOhtBQi3YSRJ84OeLox0yxTLBbVMPAY-)
  
  - 초록색이 활성화된 후 문제가 발생하면 트래픽을 다시 파란색으로 라우팅
  
  - Blue-Green 배포에서는 두 시스템 모두가 동일한 지속 계층 또는 데이터베이스 백엔드를 사용해야 함
  
  
  
- `카나리아 배포 (canary deployment)`

  - 프로덕션 인프라의 작은 부분에 새 애플리케이션 코드를 배포해서 소스의 사용자만 해당 애플리케이션으로 라우팅

  - 보고된 오류가 없는 새 버전은 나머지 인프라에 점차적으로 롤 아웃

    ![img](aws-deployment.assets/1svtNcIIT6piHc9CvoMBAdedC3ddFs_-E878Ke5dS4P1olvp2rUMenpxqYNgwNgpmB-onpvpoLgFkp9kVkttR8qEa-Y3xprp1UqtLGaaUawG7Xvhx8539-x2vVOqUxWbrN_t5-Ow)

---

<br/>

## Amazon API Gateway

>  개발자가 규모와 관계 없이 API를 쉽게 생성, 게시, 유지관리, 모니터링, 보안유지를 할 수 있도록 하는 완전관리형 서비스

- API

  - 애플리케이션이 백엔드 서비스의 데이터, 비즈니스 로직 또는 기능에 액세스할 수 있도록 해 주는 역할

- API 유형

  - RESTful API, WebSocket API

    ![img](aws-deployment.assets/k2nTA6lN2WCmr5v2BI-mPrvs6gF8zu30NPprsRNw-pQ1BvmNxUJP6nCqk_d0LRZ-38QNL9_mLxoAbiWETxgh9h_yfvHuV_F8UBfKgM14kQaV-hJWwaiNjiTBhB_2lBwkJUnaetKK)

<br/>

---

<br/>

## LAB1 : API Gateway Canary Release Deployment

- <a href = "https://learn.acloud.guru/course/178db59b-70f1-4bd8-8d74-9ab9263f8f9a/learn/247ebc2e-5abe-4d65-af18-a8609c5596f5/ddc4206e-a44d-439a-bced-ed1abdb2b4dc/lab/7118edcf-32fc-4e67-9019-e0f1db5c848f">LAB 바로가기</a>

  ![img](aws-deployment.assets/aQZ99u7weIh5Z4WV8PgK_Z1ifHnn9hYqzNZLQg1STmC9x8LWyyJZHqG_TXZLr3TAjzqqT2gAEdUGXn3yJSzSx6vhbC6_TmIAQv6N3StSwLRGlCLMtUFOzM_c-cJZdBRNODPjr4ZG)



#### #1 역할을 확인

![img](aws-deployment.assets/zEEJzlQijQolgXq_Jz9PDDjXHn-U8E9rX642NBXTaqLZwF2IojuPP9m-9zNZG7eOQg7RS8dI61A8l-857_JUVWRmkdqldfwyxeXpozGyAtWTlKmKXTUfWuGFtsnzvNCXzmleIheA)



#### #2 첫번째 람다 함수를 생성

![img](aws-deployment.assets/Kek1nMnfZS50p7Tk4uYqyzAZaWXNma-7X_HMpkT8GPaR9KuHHfmrevz_9rewOEmsbOSvYw9mV8y88DrgWHSdwvGgOvg8nOuoKLbql0jM_YUmXi5off2G6lCXGhe4ii8vrfBarvdH)

- 함수 코드 `mathCeil.js`

  ```javascript
  console.log('Loading Lambda function');
  
  exports.handler = async (event, context, callback) => {
      let resultNum = Math.ceil(999.99);  // 1000
  
      callback(null, 'this is the original function (Math.ceil) = ' + resultNum);
  };
  ```

  ![img](aws-deployment.assets/GkWotT9js0CV-8D4ImctkxqKvQgpGkWLwPh9mCNj8CmMHCpFdridtkGkiRP_-nD1tpObEhqxetgxAIN-1rGnbfG055vsVFHJ-BeoeC6i5tTrU60HtCrYwKS2VxU1jP7Cid_d8VSq)



#### #3 두번째 람다 함수 생성

![img](aws-deployment.assets/pXB_G7WwTIvOt0MxTF5-uwjdc9tKz2omJtae08kMdjPsIx1ZQ0WTIUKGrSoVnOTYJTkLHMJvqjInXeQ4etDw54GkLLXk4FjEVrJt9hXlD76SMk0-RBfSrABlQRPaDC0SG8yEehp1)

- `mathFloor.js`

  ```javascript
  console.log('Loading Lambda function');
  
  exports.handler = async (event, context, callback) => {
      let resultNum = Math.floor(999.99);
  
      callback(null, 'this is the canary function (Math.floor) = ' + resultNum);
  };
  ```

  ![img](aws-deployment.assets/GkWotT9js0CV-8D4ImctkxqKvQgpGkWLwPh9mCNj8CmMHCpFdridtkGkiRP_-nD1tpObEhqxetgxAIN-1rGnbfG055vsVFHJ-BeoeC6i5tTrU60HtCrYwKS2VxU1jP7Cid_d8VSq)



#### #4 API를 생성

- API 생성

  ![img](aws-deployment.assets/4osxB6a7l5RNBAaWN4R4ON8OlVGTT7qH0UioOQGvB3CYyJlyG7t4lSD49X5G4Hv-oaquJpJtlJI_uRQsniYY0jbvq9aq7XvU16_Lm00zVk2tOjk33XQG7L_G8xBjDdjWT2Osb6lD)

  ![img](aws-deployment.assets/b4iMjeMQ0Qutr4k54_8A8IuR6fB3GNnJYWjps643D13B5nivhTInvPyEVujdHCJC5B2sejNJZ0pQvCpO4o1yqO6D4oYmNQGJD3Vt906RZs29e2jEvaHK01S2pdL6K6MukNCy7JdO)

- 메소드 생성

  ![img](aws-deployment.assets/zs3CSwqsnCnnneDVBFLWmS6crad1RbELBYBcHdl9PEXhF7dmc-UpYrHUnsslyO5WgYzpprlRz-6L9oAwINSN8ms1baTvd8csSUi3KV-HwT_WpIGWNmJPfX9KP7aj6ncjYWh54GGE)

  ![img](aws-deployment.assets/r-1ruTGMW7hsY4LvLHJNIcZt1VTwFwuwXDWQFLLDNlsHGpst8CHHMc5BDfSTl3BhkhHslc5LneRKtiQfGDMToO_BEuta81Bu7Lp3wdGf-3o4qb2c655lJLknIHAta_aGsB8ptZYV)

  ![img](aws-deployment.assets/6kwVz7yb9-4RirCXu6T6b56ApqoyiF-gxWlRWA1v_LgSXUGQSl7LdaOotxd5jhayes8GhA0nVTKud29W6dgIsErEnCVrFoJQAFo3tvumPEVVN7mmZFYBSwttK2owpB90gPJcd1l0)

  

  ![img](aws-deployment.assets/s-mvU1Wqw0AKsO1QoXqFdzFmU3sFZT3KWnMrJunZ8iokiWnptG6vlXL9bZ-tjXseARdF8B08VZQB_-GKy4tlKsUgINPiaFxW5CTRJPXjzZjfcBZQpjg3dsvrgyg2dwgqbDm8XZGQ)

- 메서드 테스트

  ![img](aws-deployment.assets/fFRGh0gZ2GqMR5PtJc_RJPOFmpbQh8WS7aQljmS-_pfquoyo6BLE8HLbOTB5zTzUYlfvFY5752rcMruz0-ypKwCu3e1IdN4vUFUrjUBqRRXJ3srCxCRQpfD5vqyMXXyDX5i-ESE1)

  ![img](aws-deployment.assets/G0vCaioG1r8yViauScM6iu2AteXusukTWaDSwmEyS_2EFIfKl6N113NkMKrQ8y2cyx0PIVMRHNcBc9ZeNx4XmZzjYe_zXyatXbJrcqXg2LgGzbJnRD_4Ro6Uq3MlLZQxMiqdGi4E)



#### #5 API 배포

- API 배포 작업

  ![img](aws-deployment.assets/lfKKsvOaVfuW5GgCvy-3LrDVMt64JtQ628C2uuvB_eyPPcxD3Ma6FsamNcfgrJcB-MFlBAfwDmPEtdfjnihhP5Oqs9Tk1rHZkyVlkrj78pE8LcLbm1N52erqzNZoftBbRAH5BLFK)

  ![img](aws-deployment.assets/EnTN6j0tyTbhAqeWmoiuuAlxClcTWkrOELqqee6do1_Om2RchfldZbYRHNdMDVqzdA1RoF3TuHwkspXcTPu3y55T33KBwgm_n6B08RQVQlFZvaogo1-QS30-ynK6USmO8f8v9MO1)

  ![img](aws-deployment.assets/_j-6bvjb7_dRXBxyw4l6DRgHR9piord_fffJMm5ISZO7DpArAFqFhJeMa7WQEMxjKphgC5B6YnXeKgSC_0iaRW0eeKZOJn1VcGhByB4tOOai617A403rC-KPtpuFbCegDoZmJu3L)

  ![img](aws-deployment.assets/l7FFdHyJ41YKBhhXKC958KyMAYXAbw9jxMyaVhrgtkoedoTcLrWrpw0Z1dNxi3Vnp-9ByUYUHjAnyLG1-dkwyjZVJXm_8Pfn4J8MOS-LrduTFtrJ8fU-x_aAOoP27JmgjmKHM9WJ)
  - 새로고침을 여러번 해도 동일한 결과가 출력
    - API 게이트웨이에 연결되어 있는 람다 함수가 MathCeil 하나이므로

- Canary 설정

  ![img](aws-deployment.assets/Jbt5HYvg_zXYhXZwugYNCbcXZGJ-zz9JZIjArmb7MrcwvwjOXJfAo4jNyjbjP34R722QKr_ajxIihHAUIg1XTL81zNHwASljGisKTQpHEUzIr09kWuwZdw55U4lJ-tgoOBMZd08q)

  ![img](aws-deployment.assets/Jbt5HYvg_zXYhXZwugYNCbcXZGJ-zz9JZIjArmb7MrcwvwjOXJfAo4jNyjbjP34R722QKr_ajxIihHAUIg1XTL81zNHwASljGisKTQpHEUzIr09kWuwZdw55U4lJ-tgoOBMZd08q)

  - 브라우저로 API 게이트웨이 엔드포인트를 호출해도 변화가 없음을 확인
    - 새로운 버전이 등록되지 않았기 때문



#### #6 API가 호출되었을 때 실행되는 람다 함수를 변경

- MathCeil -> MathFloor

  ![img](aws-deployment.assets/KcRUmAwwlwtmvhuifjW430JhrQf-LCxLR7GUghMNsF-yKzdBI_tqHOyu4AS5BbawsE7FpsX5wWpvRWk6Uu4wx_-RJYfhvkFXqykTMiO9kaHOIfgAp6CYyLpTHxlLoQ1Qfk7vJjB_)

  ![img](aws-deployment.assets/1oVc4d2TsfgTjk7alGdL0XgqLdjRGdDWyovb5ErpZ9cfTHM7xu9T6BePSjppqLrsO33rWbSCo_zb2qRB2AXygCRdnYPCulgQ12EkJ1Al2vvb9TgeIMOMC8Yx0HyB-_NfiRnJThyD)

  

  ![img](aws-deployment.assets/uV5l_SVsorQ-bDo_d8Kd5A6k715Y8WERsmzPJuQsBhsBBTitGav-baJ4gorfwyH55mZ5bw41KhXeKFxOSqYt0_gJnbOvNfwOf6quFopyTnir79Fd23PJPa99mT7wRzVQ3PVHHaMN)

  ![img](aws-deployment.assets/MGUjC4PQHa9k_JefY3td5NtD0PqOKhi_Q8-oyXUxzhgAkORRo3lSQcT1kIcIHypO2H7DwobbgWp-RLCFQjCUAREP5m-uzZKT40IX_QZqfJ-I_3MvgQ1NXf4xaWMRyXcKVWzsoSi8)

  - 테스트를 여러 번 수행해도 동일한 결과(mathFloor 실행 결과, 999 반환)를 반환하는 것을 확인



#### #7 새로운 API를 배포

- 재 지정한 람다함수로 API를 배포

  ![img](aws-deployment.assets/lKsThtJFVoJH_V9Ff4_lVKNHFNPPBp6qH_lk1uCOmtvFcH0DMF1gvBqSbjlu4I5D6I41EWLEOLzMwJcw5MEC2xxTLxp3uVepdepB6ZaYB4RTGUKIV1fRw1y6zY5nxKJKIYikvcIe)

  ![img](aws-deployment.assets/SIC47PjQEO9dH9l08EjB2TtaYt6yC6nb5sCbRk-VzV6YRIXtQS5mpMvr4Bl3ci6MO7-tFTcIHdzcze0TMh4qXUtlrf1TpFRAHrCF9qE4Q-B660I4RmyiHixy77AGVmvdEFoDdttO)



#### #8 URL 호출

- url 호출

  ![img](aws-deployment.assets/urkUVhE15ry7tmvh6X-9np88P0D6AXwbs3Wmz_ztREwrcBBYBVqOKm-VZ5WmpYInVCnk5fGm0sDtY75o_ZAq7MWnkrTnUi7Z7KqkMeIK3YFZtTp9JM0SwvUaKHRfu2ClFMHbfDAj)

  ![img](aws-deployment.assets/Du8-2PHDWCnW6Gf6sGl_eXQMQ8J8OVUgw3hY1QlCuKD0FDDU6gqHO3UwMB208mkLvf1zLqHF-9Rw-fooBCudNkhlzues35yZ5jStPnJkojFj6mY-Za7U8YNAUBePP0KWA_plvH0N)

  ![img](aws-deployment.assets/R-wleLlEzpX_1M7yv3YI0ftYyZ-12MDUqkaxPUb1DfXpr-4msvHGpdadogW6S-qat2ZOtMEid-XeaRie_GL6EFB8g5p6aMiCwlioOvgFfgYoSSSGfzYpBdd_o-6sl2RcNAg0ikY6)

  - **새 버전과 이전 버전이 1:9 비율로 제공되는 것을 확인**



#### #9 Canary로 지정된 요청 비율을 50%으로 조정

- 5:5로 제공될 수 있게 변경

  ![img](aws-deployment.assets/lhuHfbaTykt8shDBMBQ5G49_xVdrmGjq9jxy7ZXRo6dk3Bun4sFA9MYAcEIQvwVk33QVC2MJhZ4Kq7ouQ6_MmDpKDEkail95360YIVkZZTpV0oODu8sDB6-T4uApLooJXrGNI4O5)

  - 새 버전과 이전 버전이 5:5 비율로 제공되는 것을 URL을 통해 확인



#### #10 Canary 승격

- canary 승격

  ![img](aws-deployment.assets/_p0SMsk2LXInemb36it34RBxDEsYvU9G-4mgJ6yoc98nZ3YxH6nXc4JxpdGFvjgtdDY6-fQe-e_E8ZUP9aH3t_O4WUHHM7OW6c9BTCHytCvwqv57V2uWJujI-AYTrPJZB_1--NVx)

  ![img](aws-deployment.assets/cSCl6xbuGt7CVRqqcdecUpk4nHPOP_lhbTNmANJQUfMuoD0mqK58QyWG7aUKu43PRYm6G_1RAN1ColPYyjJ30OLY2gzDCMQLp228ADhYteu1mfcJYrA8V3m6cUYpb3BYbyNlyYjn)

  ![img](aws-deployment.assets/7hdnYnkqPIxZkLRv9xbwGORm2nes3_fj-EUIz-6Y3S-gfxRG5f4czz_yi_SO89plBzEYacLGCKbSPo39waYOyyIw21ztHqCKP6ldfIlyuVVmCxBxlFeBx6cQAkeuoJmrw8r0pPhs)

  - **새로운 버전의 실행 결과(999)만 제공되는 것을 확인**

<br/>

---

