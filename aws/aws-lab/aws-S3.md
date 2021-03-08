## Amazon Simple Storage Service (Amazon S3)

> 개인, 어플리케이션, AWS 서비스의 데이터(=파일)을 보관

- 백업, 로그 파일, 재해 복구 이미지 유지 관리
- 분석을 위한 빅데이터 저장
- 정적 웹 사이트 호스팅

- 99.999999999%의 **내구성**을 제공
  - AWS 리전 내에서 최소 3개의 물리적 가용영역에 분산해서 저장
- 99.5% ~ 99.99%의 **가용성**을 제공

**![img](aws-S3.assets/efs_RS5bxsLT6a4rbvc5hvlkG10VeNsmtkUylWRgIxulo2RsXe698aFwJ4z7vkUut64c3r07HR5WayvblJS-t4T2aQmVrfGsqY2OlJcOyySICwGPgISimMmkFsVKNCoDbPsLFFt6)**



---

<br/>

## Creating a Basic Amazon S3 Lifecycle Policy

### 버킷 생성

- 퍼블릭 액세스를 차단한 경우, 객체 URL로는 접근할 수 없다.
- public 액세스 차단 
  - 객체 주소를 이용한 접근 차단
  - access denied 상태

![img](aws-S3.assets/JPnRHnDqN3S-S8r0R2QNs59rFMcIybgd4G2cn6qrV0rbBnSn3OjC5imvuh9y9BoHpyI4iBXsS-qOxty1CjVMSCbhKyXc1Sd6-UCP6Iok5ySfochidMTU93x_FeJjLgJ9HKbA0YlN)

<br/>

### 객체 업로드

![img](aws-S3.assets/t05B_WWzRM-aJ9En9fNAZKvRmOf4c04GNe5oBZG0OWSHUoddi6rVvnfcp7p5r1vffXvkl34Wmq-4olaZxpm_uE7OXdxxgj56XC8bUCFnUB4i2DqlfYA3_eRcdXpwSMWF4vAgv19Q)

![img](aws-S3.assets/XRH0G_jGfOwGN1FFS-ztK2qqGCaGAWS5hn1SxiIYlET07Rrn7Yk3NxKyorhShGUIO6d448Y4JPhjyT2koYmG5-92RuGMvIncsYTpyBARRj1MkRHw4NNK6WyUAhZ4AEvPeuaLAkfv)



---

### 퍼블릭 엑세스 차단해제

- 퍼블릭 액세서를 허용하면 객체 URL로 접근이 가능

![img](aws-S3.assets/3kjJibAgnFwQkCMzpPA-QBIgMIg6mxjXWfz0v8X9t2A4W9qkKs0w_wXwx3t9jgUYAz0MS50Dyr0Xz37oBKyCNkfBBAivvYwYdoXlvWx5GW16JmPvRrpJcCNt85ce0t9npXX2vlPS)

- 파일은 생성 시 그때의 버킷의 권한을 따르기 때문에 액세스가 차단된 상태
  - 따라서 객체의 액세서 권한을 따로 바꾸어 주어야 한다

![img](aws-S3.assets/J7XwIfm09NoxXTNSOfRZra3GNU26-9vuJXk9GH7DZ5gLoskTYMzakLKnc3HydIL2FJvlULZfhOQ_KHP8DOk_85-Z--DXKBAw5HhNoZFJvHPtZAwKZMOIidrowYRqNXz3Br4njECh)

![img](aws-S3.assets/gGV_Mxp9rGZSZL3Yl84kLcn7HGUyZTjS4zy74yBwyeVIMcT1vbBQ-Fz4ay8isplVWUI3_QBCorpSUQ95N9oCGDKRgMyrNDibAR08DJQPoOdLSQ7W5fCnhYkZdglh9PGYKveOLWaD)

<br/>

---

### 버전관리 (비저닝)

- 버전 관리가 설정되어 있으면 동일한 파일을 업로드하면 이전 파일을 삭제하지 않고 보존

![img](aws-S3.assets/LAs10ssDK9mCEj6UqKFLSZhgr3XOQQNH3CvlmM_ek1NUNDAihXhfYVra6sXElJUHHAhZ2WTN7ro80mdThPpawz1533xTMElodMMUYf7uTGqMqlUzP3tTqtd459kpLy0BiWQ6_-n_)

![img](aws-S3.assets/MtDISBfE5vqArPEvoCAuuyzufEarEC-xcu-IRyf4HJJNPgWNONoZyzr39DBcr0adoQU1NlTUN6jhBqcYL817kfhp93VnxyBgoqg9wv8x4S9TWvbb53Cn2m6QobT7AadAYcORHues)

![img](aws-S3.assets/_Emz32323r3QMd1c1nSY1aoiyKSb2GmbYNgwS10HUwfKNSIKoyiEgG1wZkIT_e7-mSWYxxL_4xD5fkY3HuPvBAMwGvm4Bea8I6YYgaLHIVcWPSTQ-FVstbKf14YRUGb8KXETpy5E)

![img](aws-S3.assets/l6PRwCHD5Laif9k45pg9R_MpvQfZfVHaPIBYVfeJscdQHgt1yaMEY2YkyQkbzd7MFXFqDlHmV-pk8EK9pFaJ862HFCay8RqPLp-omL0rND6Z4LkxQM4G26elCShtvHL9hn7h4z-f)

---

<br/>

### 수명 주기 (lifecycle)

> 객체를 사용 여부에 따라 수명주기를 생성하여 관리

- 수명 주기 규칙 생성

1. 수명 주기 규칙 생성
2. 수명 주기 규칙 작업
   - 스토리지 클래스 간에 객체의 현재 버전 전환
   - 스토리지 클래스 간의 객체의 이전 버전 전환
   - 객체의 현재 버전 만료
   - 객체의 이전 버전 영구 삭제
   - 만료된 삭제 마커 또는 완료되지 않은 멀티파트 업로드 삭제
3. 객체의 버전 전환
   - Standard-IA
   - Glacier
     - 자주사용하지 않는 객체
   - Glacier Deep Archive
     - 1년에 1~2번 사용

---

<br/><br/>



![img](aws-S3.assets/ID2rdTMmEVp5bxTBh0HHv4HqlF08sekowZNrsvaXfoiF02RkoHpNMbM5E4_SMlZN-94HeDwn4sRm1XEvk32Tyf-uG9zdKLd4TbC-HJWNaH39H-KGFHmzunc9ZU_eTOJYIlOCjEks)

### Amazon S3

- 객체 기반의 무제한 파일 저장 스토리지
- 99.999999999% 내구성
- 사용한 만큼만 지불 (GB 당 과금)
- 객체 URL을 통해 쉽게 파일을 공유
- 정적 웹 사이트 호스팅 가능

### Amazon Glacier

- 99.999999999% 내구성
- 아카이브 및 백업 용도
- S3 대비 1/5 비용

<br/>

---

## Configuring Amazon S3 Buckets to Host a Static Website with a Custom Domain

![img](aws-S3.assets/imYmNQwX9QAgdEdGInRvwEJdVnYwV2wCai5ZTfrNDECZykuwXUzt9XTmNQdAKVcxWi3sW03h0uLHDYaaA9tyx0q_ZCYUm0PY1AN70gBBVusvXRU94dHrDC0dLEXi5NTaiD5JD5T3)

#### S3 버킷을 생성하고 `정적 웹 사이트 호스팅`을 설정, 연동할 도메인 이름으로 버킷을 생성

- 주의
  - 연동할 도메인 이름으로 버킷을 생성
  - 버킷에 퍼블릭 접근이 가능하도록 설정

1. 생성된 도메인 확인

![img](aws-S3.assets/gRSclrI6Jk6fKVh_-MuZhRmINDBKFw33Rs_AkKsn03-Lcri0lbGFj6Aqw2JVXe_xvZVgjKuUbKhFd_kNOvQlW2GbVg8V2tJmpzIiEgpPfhSbhgQMDhvyc-HYdF5EQ2syJS8F7T4u)

![img](aws-S3.assets/17hwHfHqQ7I25xgr7BHrOJEP7ACu7JCA8pCqaxbLsh0xTzRpDGQW2tSqgnmrk36SeBr_iTl8jo7axm-MNWVGAafj3bJmwp5MHOJrcXkYSLzwN-OLDUEHtzXhF9CalrFlRdTC5RUf)

2. 버킷에 웹 페이지 파일을 업로드

![img](aws-S3.assets/yMtD2VLU0pmowpffZhyURgB0g5JYp7ZntGcmTXy9nosRMTudhkquRPRfxSKaKA-WzJ4--kM5akX4GpDNudqP6t0ecFKIJ3-L0RfyIRUc8zkcflXrbgbYcGF0UzIYYGoMm-h1OpHD)

3. 정적 웹 사이트 호스팅 설정

![img](aws-S3.assets/m5mSxDYt8RIDIjGt13GBJ65bijOw25q-VcPSE7zE22qL89YaEQXAKkcgS6I2zadt8cu_cVVoBXnrhgSkYcKm57DHVHjjUXBR-gkJOzjVXxtdjC_G99DJqmVglNonKjhdXYc01l7k)

4. 버킷 웹 사이트 엔드포인트로 접속을 확인

![img](aws-S3.assets/qqG-qV2XYLL0HPT7sEpNOvOfb1sAwGpHHe24dn9FaacFXyd2qFXr45yKEHMKxQd1TES81g9hI3mPRCc77A0NezdfWhm0cLsUNopYeJLgXXCRGUw2ykiuKJjupJrbQ44pfCH-2OiL)

![img](aws-S3.assets/EwMcZR7J_oUP_srSP6Ww-3MbLmf0h24Kw8ikJdPy01NCy_6k1Vhupg66Lc7sKwSHo_MhtUXeYb2EwBr7nkPaKonWsSFiN4N6bb4Cb5Y7sSqQ9Xqo1vhG6SFFL0DuPe6PRB6iAucO)

5. 버킷에 등록된 객체(HTML)를 퍼블릭 접근이 가능하도록 변경

![img](aws-S3.assets/zI2Ay7BVIX98lIlHwIUmEEhQrUcd9IoZVYjH3gyOURq6j-K1OtKa-UPas-hHM-Kp4-mjgVlrLyqZ0kGdAnhacWq1HNtoeWcdMuwAeNduyaPfKQpxO_R73eF3aLlNl7hivWZM_iv0)

6. 버킷 주소로 접근 가능한 것을 확인

![img](aws-S3.assets/12ME7I58umwfIQudOCDlWfmzkKtul8FcTzHpnGmYir9G5mtivxvAQPm2NCF6g4CRnRcvqG0z1NpUMgVrXVBQ5eWw9QvTx9nIu63q7vmp4IVtv6f8wXq-dbVx9C80EIukxEIlh4oJ)

<br/>

#### DNS 레코드에 버킷 웹 사이트 엔드포인트를 등록

- DNS 레코드
  - DNS 서버 명령
  - 도메인에 연계된 IP주소와 해당 도메인에 대한 요청의 처리 방법에 대한 정보를 제공
  - A 레코드 : 도메인 IP 주소를 갖고 있는 레코드

1. 레코드 추가

![img](aws-S3.assets/Or0mhMdEl7vRPf-Pg43rRk88-Aa69xMUr70Lf6w3rRafqTQXcpUFs2IdZTtaMPJjnqE35RIgLobh-EEbTHENOvHRjd2LQkFMEv05rSZMcGayfdSiwmzQoxV8458w8gkiSYFlNrPL)

![img](aws-S3.assets/CsePWPN58UWgmusLiukRxXbXLHNtVEy9XGVv64C3iekowhIS0GeYJUUO3srlf8oHmpxWSjGECBpLwb3zVYoXzXqV-QLygfUVZQVZAWA_juFFfn3lwvUg2Y5RdOCWqGj-RZyeSpqz)

2. 도메인으로 접속을 확인

![img](aws-S3.assets/G8JzU3sRkfYxfOfg_CywskKkCG8wlsLnJU98JzQ8aEDSn2FV_tvGDZ8f5w_HHH9Jp2R9YmCFW44BBekT5DABKETt6_lCLieymmDZ8HpUCzZ7gOlfTnE1WVyscSVWLRQqZJfVlaiL)

<br/>

#### WWW 추가해서 접속이 가능하도록 설정

1. www 접속되지 않는 것을 확인

![img](aws-S3.assets/vCPoXsaHID6GBaUbbO3e-BHT4xZB05hctSZDCCjJt6ZwNEbyvOgqmiAaaicvVjBeut1ervNPgH9BD6niaWxgaOfRZa5BEJ0_uB2FwFXlk7J5bWJysxCMrfnENAxBnazPVF3UWm5V)

2. www 추가된 S3 버킷을 생성

![img](aws-S3.assets/p_OfGAN5-PCBfR-cYSfm27DOVCZkwYQ361FkWdbOCkoNlcjZLhkjYAZUc2-6qsYkLyBYW_HjA36z46BwwavKE6DHvazFHT0DkcuI0Sm8WaQHQugmnmlQkoM1h2NBtWGA1RCJcWZR)

3. 정적 웹 사이트 호스팅을 설정

![img](aws-S3.assets/E46Fn_3uN5VupyIMR-e1DwOdXW8DEjqvqvmsH6Af6Toaw_7QTcYXYSHCRJ8AJJeYtxT_vpjs9X4FtbhUUZ8WSV9rijzQRtIBMU0EmOMIeVxMCL4tjrzNWX321rVPeO7A1yqiccLd)

4. 버킷 웹 사이트 엔드포인트로 접속

![img](aws-S3.assets/kbAO5SaoR5MbRfIw8kd_ozimpI_4pLGiYSXJdSgka9DyCEe4bkebixyVCCFSIoK7deEMBS2DETColbh7vx8AWGI29rBQZv0_-jQKYG5xLOpLiE8f7nTWmwcFeBHoSKsAkx-0Dwrd)

![img](aws-S3.assets/oRLdJWZhBAuIUsKGFpvI2FBr79jXutAsGxWra1CJxysoqO8cyejp8cuvvw3cVr7_icMA4OfPOWQI2yXoJZ2g43yEgpJ2m5GwXltDEbwt2X6k0PSUxDnyDKpniRrEQqmJFzT1HS_Y)

=> 기존 사이트(cmcloudlab788.info)로 리다이렉트되는 것을 확인



5. 레코드를 추가

![img](aws-S3.assets/xI20D0ezcUn0-HJzBo_xoeZAO48a8g6e2kdU1-GwYklEEsWYAgJet3PGaefQcbX6OgQ1kHlGGYvxMBUuRiwE0xPxEdra22fOHoRf2i4ciBA-ZzKHH05SfodZeymYR7sVnqEsC_Vt)

6. www.cmcloudlab788.info 접속 여부를 확인

![img](aws-S3.assets/HKr4CRrzs9xLqwjVMzTZDvqICaYOX9VQVfcdq4BVxoshCGRhnEfaGvTNof6wv8z-cEmrLQXTtINKlOU5qj3Df78CdkxsmgXJIKZhromtG8b2HlTiZKZIwSos0aBuLt4dPvopP6-t)

⇒ cmcloudlab788.info 페이지로 리다이렉트된 것을 확인

---

<br/>

