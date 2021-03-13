## Serverless Application Constructions

> AWS 기반 서버리스 아키텍처 3장

### #1 첫번째 람다 코드 를 작성 (p50)

- VSC 실행 > Open Folder > `C:\serverless\transcode-video 선택 > New File > index.js` 파일 생성

- `index.js`

  ```javascript
  'use strict';
  
  var AWS = require('aws-sdk');
  
  var elasticTranscoder = new AWS.ElasticTranscoder({
      region: 'us-east-1'
  });
  
  exports.handler = function(event, context, callback){
      console.log("Welcome");
      // https://docs.aws.amazon.com/ko_kr/lambda/latest/dg/with-s3.html
      // S3 버킷에 저장된 파일의 이름을 가져옴 (= 경로를 포함한 파일명)
      var key = event.Records[0].s3.object.key;
  
      // 객체 이름에서 "+" 기호를 " " 문자로 대체하고 URL 인코딩
      // URL 인코딩되었던 파일명(객체명)을 원본 형태로 변경
      var sourceKey = decodeURIComponent(key.replace(/\+/g, " "));
  
  
      // 확장자를 제거 ==> 파일 이름만 추출
      var outputKey = sourceKey.split('.')[0];
  
      console.log('key:', key, sourceKey, outputKey);
  
      var params = {
          PipelineID: '1615427970749-j6u8b9', // 본인의 것으로 변경
          OutputKeyPrefix: outputKey + '/',
          Input: {
              Key: sourceKey                  // S3 버킷의 객체명(파일명)
          },
          Outputs: [
              {
                  // 트랜스코딩된 결과 파일명 ==> 원본파일이름/원본파일이름-프리셋.mp4
                  Key: outputKey + '-1080p' + '.mp4',
                  // 미리 정의되어 있는 동영상 포맷
                  PresetId: '1351620000001-000001'
              },
              {
                  Key: outputKey + '-720' + '.mp4',
                  PresetId: '1351620000001-000010'
              },
              {
                  Key: outputKey + '-web-720p' + '.mp4',
                  // 웹: Facebook, SmugMug, Vimeo, Youtube
                  PresetId: '1351620000001-100070'
              }
          ]};
  
          //                          인자  , 트렌스코딩이 끝났을 때 호출할 콜백 함수
          elasticTranscoder.createJob(params, function(error, data){
              if (error){
                  // 트렌스코딩 과정에서 오류가 발생하면 핸들러 함수를 호출한 곳으로 오류 반환
                  callback(error);
              }
          });
      };
  ```

  

### #2 로컬 테스트 (p53)

> run-local-lambda 모듈을 사용해서 람다 함수를 로컬에서 실행

- run-local-lambda 모듈 설치

  - `npm install run-local-lambda --save-dev`

- 테스트에 사용할 데이터를 생성

  - `c:\serverless\transcode-video\tests\event.json`

  - `event.json`

    ```json
    {
        "Records": [
          {
            "eventVersion": "2.1",
            "eventSource": "aws:s3",
            "awsRegion": "us-east-2",
            "eventTime": "2019-09-03T19:37:27.192Z",
            "eventName": "ObjectCreated:Put",
            "userIdentity": {
              "principalId": "AWS:AIDAINPONIXQXHT3IKHL2"
            },
            "requestParameters": {
              "sourceIPAddress": "205.255.255.255"
            },
            "responseElements": {
              "x-amz-request-id": "D82B88E5F771F645",
              "x-amz-id-2": "vlR7PnpV2Ce81l0PRw6jlUpck7Jo5ZsQjryTjKlc5aLWGVHPZLj5NeC6qMa0emYBDXOo6QBU0Wo="
            },
            "s3": {
              "s3SchemaVersion": "1.0",
              "configurationId": "828aa6fc-f7b5-4305-8584-487c791949c1",
              "bucket": {
                "name": "lambda-artifacts-deafc19498e3f2df",
                "ownerIdentity": {
                  "principalId": "A3I5XTEXAMAI3E"
                },
                "arn": "arn:aws:s3:::lambda-artifacts-deafc19498e3f2df"
              },
              "object": {
                "key": "my+video.mp4",
                "size": 1305107,
                "eTag": "b21b84d653bb07b05b1e6b33684dc11b",
                "sequencer": "0C0F6F405D6ED209E1"
              }
            }
          }
        ]
      }
    ```

- 테스트 스크립트 추가

  - `c:\serverless\transcode-video\package.json`

  - `package.json 수정`

    ```json
    "scripts": {
        "test": "run-local-lambda --file index.js --event test/event.json"
      },
    ```

- 테스트를 실행

  - `npm test`

    ​		



### #3 람다 함수를 배포

- 사전 배포 및 배포 스크립트를 추가

  - 사전 배포 ⇒ 배포 전에 수행해야 할 작업 ⇒ 소스 코드를 하나의 zip 파일로 압축

- `c:\serverless\transcode-video\package.json`

  ```json
  "scripts": {
      "test": "run-local-lambda --file index.js --event tests/event.json",
      "predeploy": "zip -r Lambda-Deployment.zip * -x *.zip *.log",
      "deploy": "aws lambda update-function-code --function-name arn:aws:lambda:us-east-1:457117745386:function:transcode-videom --zip-file fileb://Lambda-Deployment.zip"
    },
  ```

  - ```
    zip -r Lambda-Deployment.zip * -x *.zip *.log
        ~~ ~~~~~~~~~~~~~~~~~~~~~ ~ ~~~~~~~~~~~~~~
        |   압축 파일명          |   압축 제외
        +-- 하위 디렉터리를 포함 |
                                 +-- 압축 대상 → 모두(전체)
    ```

  - ```
    aws lambda update-function-code --function-name 람다함수ARN --zip-file fileb://Lambda-Deployment.zip
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ ~~~~~~~~~~~~~~~~~~~~~~~~~~~ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    aws cli 명령어                  람다 함수 이름              소소 코드를 포함한 배포 파일
    ```



- 배포 실행

  - `npm run deploy`

    ![img](3-serverless-application-constructions.assets/YEthPufNWhLfMBd9QJN_IEs0d8VbtkbGa0srV0xCaJEWAIqKMlFEd-oeHFr8PmOj_9JhOKTOb6LLe__YFDKJg_XUfqiixtEqP1wRdhOuZVGfqDhWDsTcb9YrElLDfKjcB7Vx66Vk)



### #4 S3를 람다 함수에 연결

> 파일을 업로드 버킷에 추가할 때 마다 이벤트를 발생시켜 람다 함수가 호출되도록 구성

![img](3-serverless-application-constructions.assets/zIVfIuDwYraCJalx9QD3WYg3HMoptZRtwT27nqxF0m5QJtwERid899kghgr88eEzj9hD1avuPMfjtE9MhgAv-8DmVUaUzLAHw9zm_41cXVNgSJlvM4nTKeaaYtGp6dtecl2Dc8M2)

- S3 파일 업로드 버킷 -> 속성 -> 이벤트 알림

  ![img](3-serverless-application-constructions.assets/UJmG4Dz9nfzs3JYMCP3DP1U3R59qKWyqVnjQSS1E9A1VI8ZKRutQAAtIG50mlQakCLp2CtS8jBZSwChbI0fNRtchR5-dKz6uBy_2B3VBEyZ4Odh56xva_4pMrAKNapeu77L4le0H)

  ![img](3-serverless-application-constructions.assets/x7TZX4g4TmNgM8QorPNgreF9byrQHN0l6W4xwBQ1gALkdm48zaY6wt7Bkzm99iIq8pXYukbg0O9R5JR01hNCOTr9Oal1H0I5cwXwFr3g5TM5CDFuWTw4FEnuLtYP8ost1KpXXp--)

  ![img](https://lh4.googleusercontent.com/x7TZX4g4TmNgM8QorPNgreF9byrQHN0l6W4xwBQ1gALkdm48zaY6wt7Bkzm99iIq8pXYukbg0O9R5JR01hNCOTr9Oal1H0I5cwXwFr3g5TM5CDFuWTw4FEnuLtYP8ost1KpXXp--)

  ![img](3-serverless-application-constructions.assets/rIyyezTCMONc9d6_MeaC2sF24hYu7mffpda6al0hG2Gul_thqM2kRsDI7ZEACp5gmvg3FPNZqjFQ0z8Qz41XHnTwJjhu9Xi2mmCWYle37eyAJ2MeQnNW6TEQzbqCXXKjh97ozFVk)



### #5 람다 콘솔에서 람다 함수 테스트

- 람다 콘솔

  ![img](3-serverless-application-constructions.assets/fehgGKaOv-jBD-gFk9H8OACz_PjX3zNHeJnFAX4jIyEM0A9yNdQTrnUJyNpBfO6QaoKdz_dwS1WekM9GJcBZumzldi4wgtUdeb8EFH7BXrYgnmZLEU20OJ7LQs45v7JNr9QQpjnU)

  ![img](3-serverless-application-constructions.assets/-jM0qs_-z-10HSwhgx8DqzfwkKWUAxieenMK9tg32PgX7I8tJ-_cOJijh-kTlYH25h68Kl7kV1WNEVwQDi1ImrSBUm_Xoxqx4zTqfuSC9ktpaj71u8O_pUJlPn_7QotTBOVHufEM)

  ![img](3-serverless-application-constructions.assets/Zn-gFGQXKIzfoAdDXYlMO0R6YKGcQ0QFJxxAuNm0tSNEvIuS2M-4ymccD2Mh4K4eNqrOWlSxPcdVuI3k7leY6HBr691jPVwajDJDzO7D-Y6vWv2u828XkZUt02q0Jkud0HuFvq6U)

  ![img](3-serverless-application-constructions.assets/RZj6J_8xIwJWvZ5tPsVkSddjvU9PFgYVubWGgYy3_QrY04F_th43p9V7RxVJ1UlWiPnD65Pxxa6hxGKIAyqLzkp8IoWdyEYgXJubM_Fk0MJB8QXzI_Q4DWIg8Y0E9Yy4oRkjbFqc)



### #6 업로드 버킷에 동영상 파일을 업로드

- 주의: 작은 크기의 동영상 파일을 업로드해야함

  - 링크 : https://file-examples.com/index.php/sample-video-files/sample-avi-files-download/

    ![img](3-serverless-application-constructions.assets/5g9xw9zZQ8cEf1L8uw5Ic3vgSrYSBFLvawGz3IJTyrxJnMDvcC-2pR7fBVJD8opeERxZ-A1EOtXQVbZUMa43-wiGvjVbMR_dLMB1xo6Fd5LyIBRaw29AKv4toq4lOb7qXylXVNYq)

    ![img](3-serverless-application-constructions.assets/uo1TiJvh6BQYE-T95Smyf5hulguN5-vV_t-tlmhjYZ1FI3u8Fpcm0szRDeZKUr8iBAKNCoKquVshPovTp899nbGh_Hwbeb073JJfLcb70bFzsxnyBE6pFe2IPJqWfH097ChDoOna)

- S3 버킷에 업로드

  ​	![img](3-serverless-application-constructions.assets/xxigMTHqzOjz9-KwgOyJM2XkPoM5E3421AxD-ozni6iadk1_12RR9L8iX4fyIQDe5vwm-xkhfkyu4X6Li4Dr7c2z0QDOQEGVs3HfsfEWaQCihVIs1StFzgx4NM9-4R6ZhupdExNa)

  ![img](3-serverless-application-constructions.assets/ned8uEB4HkbxBgZLH_c6vd-GNbs3KB5QuNXko0wW2Ooyhy1KDo9jNYggdekN1Oim7HoON6Zi8Fv0oXGkO71UqKysE0QMLT5tJHpJ-RBz9Nxvcq31v7GxPFjan-m5Edefbr1o9ulC)

- CloudWatch에서 로그를 확인

  ![img](3-serverless-application-constructions.assets/leCPr9k9Gu2P_EV5L9kBUSWdhuMZQBwLFerl4W6Cz9OSK9SQ-Be1nfLi9MzLfJWEFbi0LL5ChC_D4E6vkyndwM4Ve3mwb129qWbZ6ncRMlgiZfOZpYXxQbHh7xuTr3DnIJRL2h2k)

- 트랜스코딩된 결과를 저장하는 S3 버킷을 확인

  ![img](3-serverless-application-constructions.assets/H9_opyM3ZxBYZIjnRlrYFep9zmksjq6JIilAW2qA2aFC959MsCYGq9_3l5nH2qkOBTCdBgF9qGcwKq3F9uBwvRyg4-8_6t7UGz03moRqgYEzXLJW1eTlm0bdOiwd58LRFgJCN0jq)

  ![img](3-serverless-application-constructions.assets/S8t7xfD4FOK1C8EjLvT7S-Xs-RDThzvV_5m66o7EN6Fgg1CrJC07XLaek_wli7hllzWo_SDL7dU_2GtffVqbKc9myfLEHMZG1fx7o8z68btdvJaRi4tdWsIgeAPz1Yx_vmTKewcy)



<br/>

---

<br/>

## SNS 구성

> p61

![img](https://lh6.googleusercontent.com/NSqW4NpQD-3CG-2hCqpuRrAhl37zHoxLfvYAWVtcCOlv2v9BtgtNlOee_k6P9PtFGd70LOMEZL18KgRdyXuK9mtSFI_8OeMGATkvh7A4Nx9MskqlSASi7gJ0IkrKzn0A5wP5MnGN)

- 트랜스 코딩된 파일이 저장되는 S3 버킷과 SNS 서비스를 연동
  1. 관리자(사용자)에게 이메일을 전송
  2. 트랜스 코딩된 파일을 외부에서 사용할 수 있도록 권한을 변경하는 람다 함수 호출
  3. 트랜스 코딩된 파일들의 메타 데이터를 생성하는 람다 함수를 호출



### #1 SNS 주제를 생성

- SNS 주제 생성

  ![img](3-serverless-application-constructions.assets/YEjAodwCFE04fTKn9iWhrDalmzLlLwWwllMxZOdeU3IlDTykAT0c72woesD-m5_e5lADdEFe-j0tkNGgsP3vHa7jc49dyB7hJwmON8YjECNFXheiIU6WS4rIa-Qg1Tz4GKI4B6qv)

  ![img](3-serverless-application-constructions.assets/TAhKYWJfilvMZM921RLYgSbx_N4e0UAdkZm2moWJtpN7zNW-2f3uUckO1NsyTc32CDmfDqEWUqy_CW4CJ-5Vb1_NhdCZBkFE2477d4vhB48164MTyLUZ_LROT69eEVuPrMKtYBOL)



### #2 SNS 보안 정책을 수정

- 트랜스 코딩된 파일이 버킷에 추가될 때 SNS 서비스를 호출할 수 있도록 보안 정책을 수정

  ![img](3-serverless-application-constructions.assets/PoL6K0KfTI2xXUwsDWR0uhFTf5_rUcGhjadm4dFVSBfh1puvmdCzCUVR-T21LooIapEVc9iqw8v5KN5ORZiSq13zHqBblItiu7-1IZfWOQqIorCMAtD3YlfJtS8r_KJ4jVT6zX6X)

  ![img](3-serverless-application-constructions.assets/maZf6W1CBSw5Ty7_V-O4ubs05Pzvo7DcT2RwVmAqjfX3jNfqbK97V_Lp0sbbf44-kxWlJ0AkGBfcAW1ewKt7Q66OIC9b3agR8DklYf4Yi8U_vIjBJjFk52XkSJnAWueZqLSjt7Ko)

  ![img](3-serverless-application-constructions.assets/P8H8q4N2GTDw0y8UEBmOKWOjpvhfyih8_IcUkzMGs7-I1CyH0_WK8xZ-JJIoPDNUliSW6wEyQugYGSuT0IotLkvQjP1Zj5q7QwDnul5RM4WqcsRV3AF3Yv8u3q4RA6dMLfVNhmn2)



### #3 S3를 SNS에 연결

- S3 버킷 -> 이벤트 알림

  ![img](3-serverless-application-constructions.assets/NxUQ6JFoJXnCKUSM4T2c7z-kfgWfPE2oPNG9B77e11dDl488-Lm0F_T85RuphcoRLNNu6anQp8z-lDJHruYesvGUWxjKn4RyvAbLLglDL6mCo4vQhKvkrtbhrCSUpSqtBuODuvmo)

  ![img](3-serverless-application-constructions.assets/vJXaqencgx_1iF19Yi0c4_ptCS8-q0l4UZY4Y8ZU7nbJuzXUpfFC0JAp0134zAHI2g0X8pxFKC-pexGUx9rUNBimqWf6mQD1j54i8NzkprMddSfcs28dIa2930GZAq_8Sy4I3ej8)

  ![img](3-serverless-application-constructions.assets/1ChDHEHusPYh-H2JPOCX4EEHoHA9xu8QhtlxfxKh2VATxZlubrP6nxLAWuXjYYxzLekI-B0YOwwRyPVWANoJ0vjNurXFil0VWF1UefwLy5-Zr_QGAUdLDcZyFaTNs-pElU99MvvZ)

  ![img](3-serverless-application-constructions.assets/q1UyF_jI5-gntPjE6TCiyXF5B2majJZon_PLRtipsIwu3Y-2rzvc4Gn1Kq-pR8jx8qT432-XXGXyW-EBHagWEkqYJS-5eyzxALGGh7iCyCztyjKLEOXhEvgR3ysugRz28ru0hCDE)



### #4 SNS 주제에 이메일 구독을 생성

- SNS -> 구독생성

  ![img](3-serverless-application-constructions.assets/ZS8ZbEQF5b5y4D6ck0PdGVAYd_eU1G3ztAhwnPh6oB--gA7x82QdkpiAlzyBrJsqbNj7zYlN_tex2YrTlKH1OhKKzrioZarRzXbHvkO92xHmqFqc_pKIJyn6YTz2LSvHCt9pETmV)

  ![img](3-serverless-application-constructions.assets/mkeJi8GYl-kd3Xzwtf5RBFlh-ch5lxff4wbKB3S1H-C7sdJXTDRpuAjbLnShbBNetuGtfsgq_sMx2KWtJ2NFqIRGLC2Z-zhoSRUSeIC90nYAa1HpTNl8HbM8MgFtCwrz_ei38qij)

  - 해당 이메일에서 컨펌



### #5 테스트

- 업로드 버킷의 파일명을 변경

  ![img](3-serverless-application-constructions.assets/UjUyfo_RPWWhIoaVTB_oVgwRyY8QA0rj6Zp1mqKI2QP7sQGP9IYgtI_PZsOkAlMcW76hannF96MsQul9zQSHxSPHYxTf63fi8HXjr13MbpsOuHOLoX9GTZqfBw0n0kvWiwrMCgEE)

  ![img](3-serverless-application-constructions.assets/Kb2W7qplvbYN24PW-RqEL333EpV_5fDxL0ByeDVMPeRGAB2mMhj7aCxqUCtL-4EkFH61KQqJlU0dmz3PuNcTt_u97JoMmiAOyNOrxPRnSw2czFZBOsXYB23e8RPM48mMMOMBbyPe)

- transcoded-video 람다 함수의 로그를 확인

  ![img](3-serverless-application-constructions.assets/JQlTZrZR7lmWZIbNutZ0amuiohEke5mjSDDIcYQx8nV5rn2rEUDAQVGgXe9g1i8mvXcTJqL_4L03RX_RRSeCzfoe0KLnVKS5pfQSUOdj0r7Ohio4v7bVmYvGi8fddvWb6bzdnv0l)

  

- 트랜스 코딩된 파일들이 생성되었는지 S3 버킷을 확인

  ![img](3-serverless-application-constructions.assets/1PtQ42ueu5qTXgROoHVA33RQHqobMTIjUjSL03aq2vxBsM0yZ23M_KUAsGswshsjoV9uLlTKGLZqXTfIxjIDz_h-D7VHveKmaq5gBNb-Um7xrr4aQDpzjyT71eSfFjyuWtz0NniJ)

  ![img](3-serverless-application-constructions.assets/luN3njDlrHLEvwz0GKlKkKbAqEOM6dJPKMPNHWXZUBUw6lPGd83qDHQQmcdNN76tD5vqIn8LoDNjZsOgTpI94S0Dv6xA93gFnUw89C_u71iY0vfI2fNeENrygvfQlDdq32wTdF5L)



- 이메일 통지를 확인

  ![img](3-serverless-application-constructions.assets/5_zNbWiqnGYxY5ef672gmJYEFdHirl2m27jqvnwnJbI8255SlZsjH0b-BioxU5y1_5jYXkHv_2RcYuwKsBhfuxMFAfTJVuocbMuy7ddGaErNAS8imSr-dtQ6h263RpHwQeur6_Mn)

  ![img](3-serverless-application-constructions.assets/Gu_OJ1KTVaVeG3DwSKWME_mByLhjzvf9n3eAqufyXAE0OoTzagT6xQbSKR-n4NdW0_G_HRCw9xbCDbreLxFdz5fk5wkVEdv_GY5aO9V5CTfxEHLJx-PnI6GqtLsroRBtCmk8yEqd)



---

### # 지금까지 구축한 내용 

> p50 ~ 65

![img](3-serverless-application-constructions.assets/bpE_UFC8b9I0ZZnnelqTmym7eZU-WO70en3jK3sugHMCSIT5t1zRsDDpDLoq5Bp_WJ0DVZ0lKMmQ1o453ssNfIAfXl9GBBwwYBHJKdzWH3cnEHUbJqf7fzfSl2Asuta8xtVfCLuh)

<br/>

---

<br/>

## 두번째 람다 함수를 생성 (p65)

> 트랜스코딩된 파일을 외부에서 공개적으로 접근할 수 있도록 설정하는 함수

### #1 람다 함수 생성

- Lambda 생성

  ![img](3-serverless-application-constructions.assets/DqwVx6LqRvDyEW47VJyf1vfIwSXBmd7CsmrEKgTQtR6fDANOeLVtGynS-IuVAS8h-9iPIjGSj03P8FzZ8ZfDFcepA3zl0FlnZPVJEvZjS-nCjqbG0KfE-Yu2NdQgDZDh_9p3IFgr)



### #2 람다 함수 소스코드를 작성

- 프로젝트 생성 및 모듈 설치

  - `cd C:\serverless\`
  - `mkdir set-permissions`
  - `cd set-permissions`
  - `npm init -y`
  - `npm install aws-sdk`

- predeploy, deploy 스크립트를 추가
  - `c:\serverless\set-permissions\package.json`

  ```json
  "scripts": {
      "predeploy": "del Lambda-Deployment.zip & zip -r Lambda-Deployment.zip * -x *.zip *.log node_modules/aws-sdk/*", 
      "deploy": "aws lambda update-function-code --function-name arn:aws:lambda:us-east-1:457117745386:function:set-permissions --zip-file fileb://Lambda-Deployment.zip"
    },
  ```

  

- index.js 파일을 생성하고 코드를 작성 (p66)

  - `c:\serverless\set-permissions\index.js`	

    ```javascript
    'use strict';
    
    var AWS = require('aws-sdk');
    var s3 = new AWS.S3();
    
    exports.handler = function(event, context, callback) {
        // 이벤트 객체에서 동영상이 저장된 버킷과 이름(키)을 추출
        var message = JSON.parse(event.Records[0].Sns.Message);
        var sourceBucket = message.Records[0].s3.bucket.name;
        var sourceKey = message.Records[0].s3.object.key;
        sourceKey = decodeURIComponent(sourceKey.replace(/\+/g, ' '));
    
        // 동영상의 접근제어목록(ACL) 속성을 public-read로 설정 -> 외부에서 접근 가능
        var params = {
            Bucket: sourceBucket,
            Key: sourceKey,
            ACL: "public-read"
        }
        s3.putObjectAcl(params, function(err, data) {
            if (err) {
                callback(err);
            }
        })
    };
    ```

    - 람다 함수로 전달되는 이벤트 객체는 다음과 같은 구조를 가진다

      ```
      ...
      "Message": {
      	  "Records": [
      	    {
      	      "eventVersion": "2.0",
      	      "eventSource": "aws:s3",
      	      "awsRegion": "us-east-1",
      	      "eventTime": "1970-01-01T00:00:00.000Z",
      	      "eventName": "ObjectCreated:Put",
      	      "userIdentity": {
      		"principalId": "EXAMPLE"
      	      },
      	      "requestParameters": {
      		"sourceIPAddress": "127.0.0.1"
      	      },
      	      "responseElements": {
      		"x-amz-request-id": "EXAMPLE123456789",
      		"x-amz-id-2": "EXAMPLE123/5678abcdefghijklambdaisawesome/mnopqrstuvwxyzABCDEFGH"
      	      },
      	      "s3": {
      		"s3SchemaVersion": "1.0",
      		"configurationId": "testConfigRule",
      		"bucket": {
      		  "name": "example-bucket",
      		  "ownerIdentity": {
      		    "principalId": "EXAMPLE"
      		  },
      		  "arn": "arn:aws:s3:::example-bucket"
      		},
      		"object": {
      		  "key": "test/key",
      		  "size": 1024,
      		  "eTag": "0123456789abcdef0123456789abcdef",
      		  "sequencer": "0A1B2C3D4E5F678901"
      		}
      	      }
      	    }
      	  ]
      	},
      ...
      ```

      

    

### #3 람다 함수 소스 코드를 배포

- `npm run deploy`

  ![img](3-serverless-application-constructions.assets/7fCw5ZVYpauwY2U2WaiZc_qyGtS48PHDODafwQH3J0bHMrB-qUg-aklHiDJghNhrRQ0hBKXr6UTbYrMumh8c2vzH1KJM-25vbtQbMV-LxxR-VVS6vKr4FxrNj3wIKMIw49U8cg4i)



### #4 SNS에 람다 함수를 구독으로 추가

- SNS 구독 추가

  ![img](3-serverless-application-constructions.assets/b37lEbzB2FloFNb2880-hr9igd_izYp1-tGSHOgxYR2pdm2z6id3Yd6s3TJi74X6hM2ut15a7TpeqfJFYuZV5wsafw8uFsgceRtg5_uYcpxsnTidrnmHIEKtNBH41ZdFaXA00M2w)

  ![img](3-serverless-application-constructions.assets/JlDLb7M2U31KtXgZbq68QchlDYn9CT_JPwF7_gQqgXx7MwVAXJhGQj-oj3N3-u8hTsMU11G-pIbIHvtJAlh3syx93LBG2tenFEX7kPRS3_khGJDiwLLOUGwVqaKAjX3BtDWNKdDv)

  ![img](3-serverless-application-constructions.assets/kchAr6bnj0NbEn1avRECXJkdYcGEJd2i-51-M-S-1ZOid6og2LSTpPdV0tKR09lzjv0uBcz8zpX4yD1hksZcZSsQj6QwmsFz8HiIrlqxmsn-efe1Q2U4H_WQ4-Z7DVUuYAN3qhMW)



### #5 람다 함수가 객체의 ACL을 변경할 수 있는 권한 추가

- 해당 람다 함수의 인라인 정책 추가

  ![img](3-serverless-application-constructions.assets/Yo8FyUx7m4cTEoClrW03MPXgSfwgjdt37ODIaRUHoG0Z7pyJUAAYMQw14zaoCUADTP_Osbj2DYx8urQCRbhcp9l4LUHssXQIQ5JAPL0CaM8J8hOZOiFFDQT4riYfxgjNfKGpe_DR)

  ![img](3-serverless-application-constructions.assets/uE9YAJWDpX4eucVIDlAcDurw0R6I-weGpdgJMZxq8cmf3BGasqfNiq-7FLR4mqlfe_S05uT8SdeLpesiGgBP2ju3tDAe10aqNAu1YVPNYVvqd__CFz970LCiz7YETv2ayr18K5Dc)

  ![img](3-serverless-application-constructions.assets/AESEz0WwyYlgolYwzvlZTcyZM1ZouILnsnpbQU1gkFg8AWf_mgPixGq6UlWLA8Up1DA6yOSVkkHLKUBvjUvx814MsGfSc1eZbSenEn5MNlg7RdfCexEJ2BoBuBqltdpgmkn7Kwwc)

  ![img](3-serverless-application-constructions.assets/JUTs8ARqvoMv6frsEDUEKs9SUbzdLUfVkrM2742QniUXZRlxbvR5Y9o8Dk8YnzzzV6urxtEqdjFkfngdY612wf7oaJ5j6YcTI54DQEvMKOCBIGrLLxRsxZO5UkOzlyFfDhEXXwBP)



### #6 테스트

- 트렌스 코딩된 파일이 저장되는 버킷의 파일 권한을 확인

  ![img](3-serverless-application-constructions.assets/ITUigU8V5oulagHlmU5G3hD-H5S72SOQsVF5yI47UD63-BtD7htEbVUSFGp8V7sT7P7y96wNl_NhcM8CSH3O-nAzX0ainVPzIIIk3JN5nG5ywjIpwa3x66gJOKemAemarq4hNJ-F)

- 업로드 버킷에 파일명을 변경

  ![img](3-serverless-application-constructions.assets/a4GqfdyeOmK5z2sq4gtebPwEXjpx35jn6PWAT_RKvrqHauIQj7qVvrI-tlVHPR3vdgJgZ1Ls9yFSlpVMf5vIu6bkcJjFxgVohbGVgK1UjY-sbDPpbw5mMvWW70RZsIuJ13SleyLV)

  

- 첫번째 람다 함수의 로그를 확인

  ![img](3-serverless-application-constructions.assets/ll6Ixh4yXl7IwryLnoVFemsJlDu8aJiw9rqVZaxtAsI0_7Hz7PZFzRrtjOlkVUE1M4gVqayij8_rKp4pSstQTVjJ8jbAMX4JHbVaiObBtogs7FZzA9OHkVYldokNPjcBH0go3xb0)

- 트랜스 코딩된 결과가 저장되는 버킷을 확인

  ![img](3-serverless-application-constructions.assets/ukrIz0lZEe3ipiqvb7BHw6VM_eI0ziCpvVpfB-Oa3GsptPCPPQk6LH-yW7NlxVPPHM8XhKyupvWdDq8tOaHVDFmxPy4DeuoSMV357esdjaN2CLwQlok2417GXSBPI3gBelH9culH)

  ![img](3-serverless-application-constructions.assets/1t5K9jGbCdo_UMX6RzqBwzAt1lBnC7rsw78GbPg_bXt2Tkmc4wdrjjbNNDuxX9xIiqScTM76w_9g8Z1IahGMPTRc9-lWd40FpBPKPxHFJAU0f_F1fDdyS9Gp4d7cDONNK5Gr-I1n)

- 이메일 구독이 실행되었는지 확인

  ![img](3-serverless-application-constructions.assets/Gf2hbmnVNZ7c_M-1yCVV_tt8n9-k7Q_Xr4fJ-rvv06uzUbr4XgfAJ_H7SakjRGjex0PxzJnvT9Dp-oJkBgmlIj_L9gtOj9SZHUiOTLMsxSVzGAVub4xLjMIfg61PSGtN56-igtQg)

- 두번째 람다의 로그 파일을 확인

  ![img](3-serverless-application-constructions.assets/zcgeWB1zsq7E7ct0h4N-KhqERIzTDdQHgVtQpyH1OrZi0_6E_nQEx7v3UTTUWFp-IJ66QQ-rVrNU3iTnJvVU9toCTBFY0zwjQHPQZD_0VT_-3XvzqEfxcu28E7rIrPJDcJbQwtbD)

  - 파일이 저장된 버킷에 접근 권한이 없어서 파일의 ACL을 변경할 수 없음

- 해당 버킷을 접근할 수 있도록 해당 버킷의 퍼블릭 액세스 차단을 해제

  ![img](3-serverless-application-constructions.assets/x-_9vhXH4LQfr56fxF0hqlz_8mJ7WtCO_C_ip6ONxuqtGEkZedm3NwDflAXh45zxAH9yI_UQKIIYoXdUdLrHbQKg1PEk1UfDUGG7DIj1LgFmYw3nBl6GeHk0m1BrD6IQ2Vd2_xF-)



- 업로드 버킷의 파일명을 변경

  ![img](3-serverless-application-constructions.assets/tWq8rCOobyso47MCs1CfAmqmIJfVn-WlTEK5VYA5VfzqqIotDZUqmRZkiT6MugcuaFCT6xW7oXOgx3bxf333CYeRXRP4cZnvXVDek2LsNyYOvFQ9hN_sP1bXWd5PlMagmlYYRtfk)

- 두번째 람다 함수의 로그를 확인

  ![img](3-serverless-application-constructions.assets/6pSXP9fNLvLFL_Ac4HawXRhl-YCNG6iJfgofgJm8Mzx0SvVWvkcRyUovi1wZJ73cGzJaWcEdjApIcqTUD8Kx8XNdB7nbXnNyGaCOh_cYiHMTjyKI8DIDrEjVepKEKraQW05oyW7o)

- 트랜스 코딩된 파일의 ACL을 확인

  ![img](3-serverless-application-constructions.assets/NokAH5MdgDcUlcmDttnlEVAEpM09YtJAc0JbZIAt3b5ZvyjENpzbknxdho45_me4LAwG0O8hGUD725L-zjknFXAItOVW9_w91kPRNClRaRhv0eMqnv8RTc5mbQiYxLFauHWr0YiR)

- 객체 URL로 외부에서 호출

  ![img](3-serverless-application-constructions.assets/iQMSI3pLBlcZnG3nSVw0a-cWqUjum2_Yqu5SBT5SMIoV_5i45M7PoLSPKqsZN_AFkGptBQE7_lCskXqAtUCnCVKIgDfqCnfikFd0B0crwdsJfxPBcav7R2PYZD93KmD_B2dVft_N)
  
  ![img](3-serverless-application-constructions.assets/VQvkhyjer0dcFGj9RZmxBqWIbabbM2qgcIacQ33d71eUkOiDXahnbOnBA-cM1buqFCAP2wo3G_3PWgOmplOHtLdtUwbMP_wwKGpBVDQuyfO6eFdtNvS7Lj-1OFskAIfznHWhrUcM)





### # 실습: set-permissions 람다 함수를 파이썬으로 구현

>함수 이름: set-permissions-python
>
>런타임: Python 3.7

![img](3-serverless-application-constructions.assets/Ng1w0gXg-weIDt9GX74isdtVZiMYdc_qJunif2kkb13-zY6s5egZp4k6doDsD-iC7EcMvJjoxeI9QgrXj_9l_inwFTUnmDMZfnwsBlNZEykbbQLfuvOa9Lz6ttr8kb6sqpA4txud)

![img](3-serverless-application-constructions.assets/STSL3AJ7c-Xn86YrkXeixbwyJfTDAVjtNsZJxgCL-2HZ9CdYZQnlgC4hzkCWgCdms2Jz_yM9x_d-iCUbf5tG0Dyn4dqeFlFRaBnbCwl5H_ymQ1pT-XXaiFWp54MF1CLuwmdWTkdi)

![img](3-serverless-application-constructions.assets/Mj-wQtt2wwy59QbuoONj6WxmD3c3busweG7mXk_Xmi9nIRjL20ELqsL0FVjUH48W01aXG-qoOw3WVcfj8WbjlY45v10h7r0DR6bBzY6L3Oa7LiQgukNTTlCxomBXE_A8dFUZHAal)

- 테스트

  1. SNS 구독을 추가 
  2. 트랜스 코딩된 결과를 저장하는 버킷의 파일 중 하나의 이름을 변경
  3. 이메일 구독 수신 확인
  4. 로그 확인
  5. 파일의 ACL 변경 확인

- Lambda 함수 생성

  - `lambda_function.py`

    ```python
    import boto3
    import json
    
    def lambda_handler(event, context):
    	print(event)
    	s3 = boto3.client('s3')
    	message = event['Records'][0]['Sns']['Message']
    	print(message)
    	message = json.loads(message)
    	print(message)
    	bucket = message['Records'][0]['s3']['bucket']['name']
    	print(bucket)
    	key = message['Records'][0]['s3']['object']['key']
    	print(key)
    	response = s3.put_object_acl(Bucket=bucket, Key=key, ACL='public-read')
    	return response
    ```

<br/>

---

<br/>

## 메타데이터 생성 (p68)

> ffprobe 프로그램을 사용해서 동영상 파일의 정보를 추출

### #1 Amazon Linux EC2 인스턴스를 생성

- EC2 인스턴스 생성

  - (window환경에서는 ffprobe 프로그램 사용 불가)

  ![img](3-serverless-application-constructions.assets/RnSmpcpAa93qKaEmsQdINkj2vLzjgyRkncfBXDKJCEEZvbAO1IAFb2Vk76UjoCucLtBvXt9xjEA-zDLbSifaoR-F3djvfWKtlFx0NdwSUpdCKQ9mWgJr4cLO6T0xDeZTvMSwdOEX)

  ![img](3-serverless-application-constructions.assets/yyg_DlGCNEf6WptkiFe-SVcVKhhg_WN84VvS-4g7vljGhFzzEA0qewifI5gkj-gtw9ja9AKaKZwe08y2omsjbovyFL2ROlsJUOV6VX_zQRc13MNBpg1SmKzfrmboybjcqwPxQ6qk)

  ![img](3-serverless-application-constructions.assets/kdUGwYw4jPHQ-fNED1BslY0F8dAEzVoeYh6g6efnrilUj66CAnKmSQ3P5yCoNkN9Z2Uru0FfulTH4H0eODWQuDiucmQKxuseH2QAafQD5QO79rkx8zVV4ngtvLrCEiqr0sK-otCX)

  

### #2 인스턴스의 SSH 접속 후 node.js 설치

- 인스턴스의 퍼블릭 IP 확인 후 SSH Client를 통해 접속

  ![img](3-serverless-application-constructions.assets/Ck5dFEqZb3VvM_NmFsTUlTDDzUWvEXiNwQwtfTjqQBjIRHL2oG78a0zalFAvXRlSy7-LYzR5WWgkHL0zh6h3eDq8gF1eBREMLN8Cxo-YWcruvrQFRNsCN8i2LUCTtZMBFSXyKIK7)

  

- node.js 설치

  > 참고 : https://docs.aws.amazon.com/ko_kr/sdk-for-javascript/v2/developer-guide/setting-up-node-on-ec2-instance.html

  - $ `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash`

  - $ `. ~/.nvm/nvm.sh`
  - $ `nvm install node`
  - $ `node -e "console.log('Running Node.js ' + process.version)"`



### #3 인스턴스의 리눅스 버전에 해당하는 ffmpeg 정적 빌드를 다운로드

> https://www.johnvansickle.com/ffmpeg/old-releases/

![img](3-serverless-application-constructions.assets/P1Bf69gYVlU5OYfzfKSp35OpvE8P6vGIUlY6kckDas7o3Cbzyu18KZncPUwZV64AIsCtdG7LvVHS31RfdkBOv8tWykxaDaDT1dx2GkhGdBs_7ZtswjHb66wZ4O9gnX0Yux-SwyWh)

- $ `wget https://www.johnvansickle.com/ffmpeg/old-releases/ffmpeg-4.2.2-amd64-static.tar.xz`

- $ `xz -d ffmpeg-4.2.2-amd64-static.tar.xz`
- $ `tar xvf ffmpeg-4.2.2-amd64-static.tar`
- $ `cd ffmpeg-4.2.2-amd64-static`
  - $ `ll`





### #4 세번째 람다 함수 생성

> 동영상 파일(트랜스 코딩된 동영상 파일, 3개가 생성)의 메타 정보를 추출해서 S3 버킷(트랜스 코딩된 파일이 저장되는 버킷)에 저장하는 람다 함수

- 람다 함수 생성

  

  ![img](3-serverless-application-constructions.assets/UkfVX9nF6bUVsW6m8knk2WNkalwW-SHclFdF7envOVjmkuQQPlY0oyCdsUk-YAIIK38QjsuW-pW8DGFz0iI-hpanoQu5rG2dRFxL4B7nE9lLJCy2NyRsXYPRzCGLqgZsBIsTGeqw)

  ![img](3-serverless-application-constructions.assets/5CcP4y3UiTZbwh9brvVR2Zteqzn2oU-s4gayLrHYVIxAjq7r9_rayUZuB6-OG14bTEDXAF1dDMfF7NES8lNldFyr4BtpT_GnxSf_-AwsfTVa-jI9D9nCr8s24aosXetJmZB5okv1)





### #5 세번째 람다 함수 소스 코드를 작성

> EC2 인스턴스에서 작업 ⇒ ffprobe를 람다에 포함해서 배포해야 하므로

```
$ cd
$ mkdir extract-metadata
$ cd extract-metadata/
$ mkdir bin
$ mkdir tmp
$ npm init -y
$ npm install aws-sdk
```

- package.json 파일에 predeploy, deploy 스크립트를 추가

  - `package.json`

  ```json
  {
    "name": "extract-metadata",
    "version": "1.0.0",
    "description": "",
    "main": "index.js",
    "scripts": {
      "predeploy": "zip -r Lambda-Deployment.zip * -x *.zip *.log node_modules/aws-sdk/*",
      "deploy": "aws lambda update-function-code --function-name extract-metadata arn:aws:lambda:us-east-1:457117745386:function:extract-metadata --zip-file fileb://Lambda-Deployment.zip"
    },
    "keywords": [],
    "author": "",
    "license": "ISC",
    "dependencies": {
      "aws-sdk": "^2.862.0"
    }
  }
  ```



- index.js 파일을 생성 및 작성

  - `index.js`

    ```javascript
    'use strict';
    var AWS = require('aws-sdk');
    
    var exec = require('child_process').exec;    
    var fs = require('fs');
    
    
    process.env['PATH'] = process.env['PATH'] + ':' + process.env['LAMBDA_TASK_ROOT'];
    
    
    var s3 = new AWS.S3();
    
    
    function saveMetadataToS3(body, bucket, key, callback) {
        console.log('Saving metadata to S3');
        s3.putObject({
            Bucket: bucket, 
            Key: key, 
            Body: body     // key(파일)의 내용
        }, function(error, data) {
            if (error) {
                callback(error);
            }
        });
    }
    
    
    function extractMetadata(sourceBucket, sourceKey, localFilename, callback) {
        console.log('Extracting metadata');
    
        var cmd = 'bin/ffprobe -v quiet -print_format json -show_format "/tmp/' + localFilename + '"';
    
        exec(cmd, function(error, stdout, stderr) {
            if (error === null) {
    
                var metadataKey = sourceKey.split('.')[0] + '.json';
                saveMetadataToS3(stdout, sourceBucket, metadataKey, callback);
            } else {
                console.log(stderr);
                callback(error);
            }
        });
    }
    
    
    function saveFileToFilesystem(sourceBucket, sourceKey, callback) {
        console.log('Saving to filesystem');
    
        
        var localFilename = sourceKey.split('/').pop();
        var file = fs.createWriteStream('/tmp/' + localFilename);
    
      
        var stream = s3.getObject({
            Bucket: sourceBucket, 
            Key: sourceKey
        }).createReadStream().pipe(file);
    
        stream.on('error', function(error) {
            callback(error);
        });
    
    
        stream.on('close', function() {
            extractMetadata(sourceBucket, sourceKey, localFilename, callback);
        });
    }
    
    exports.handler = function (event, context, callback) {
    
        var message = JSON.parse(event.Records[0].Sns.Message);
        var sourceBucket = message.Records[0].s3.bucket.name;
        var sourceKey = decodeURIComponent(message.Records[0].s3.object.key.replace(/\+/g, ' '));
    
        saveFileToFilesystem(sourceBucket, sourceKey, callback);
    };
    ```

    

- exec() 함수의 실행 태스트
  - $ `cd /home/ec2-user/ffmpeg-4.2.2-amd64-static`
  - $ `wget https://file-examples-com.github.io/uploads/2018/04/file_example_AVI_480_750kB.avi`
  - $ `./ffprobe -v quiet -print_format json -show_format ./file_example_AVI_480_750kB.avi`



-  ffprobe 파일을 extract-metadata/bin 아래로 복사

  - $ `cd /home/ec2-user/ffmpeg-4.2.2-amd64-static`

  - $ `mv ./ffprobe /home/ec2-user/extract-metadata/bin/`

  - $ `cd /home/ec2-user/extract-metadata/bin/`

  - $ `ll`

    ```
    total 72256
    -rwxr-xr-x 1 ec2-user ec2-user 73988648 Mar  2  2020 ffprobe
    ```



### #6 람다 함수를 배포

- aws configure

  - `aws configure`

    ```
    AWS Access Key ID [None]: AKIAS*****CWZS64OWB
    AWS Secret Access Key [None]: d91uKyd********************I8zSPBXaZtVx2
    Default region name [None]: us-east-1
    Default output format [None]:
    ```

    

- 배포

  - `npm run deploy`

    ![img](3-serverless-application-constructions.assets/KF3opYmNcgZCm9q3OALBbKkLc8wt-VpFSgCEAB_XSwYZ9j3CYK1K7c_L0-PNT24N_xUbx68u4GD6LEWdRiZtMu0Q1shVj50tdmsBpKr4Vm5qmGSsYXoVL7VxcHC2cpg1N8-4Qx91)

    

### #7 SNS 주제에 연결 = 구독 생성

- 구독 생성

  ![img](3-serverless-application-constructions.assets/dC9GoeoxkQMVkrP52NnTp4xQWw9T8scu4ztrTJsaJ3Cxg6vgkmWzcMvStkg_quq689xR7EKGZMTlgbgCEMGhXMGGieKP8N0WVEKCTfHfP7YLfRAEf0A0YEmm_AmeqkFdsFHI13EY)



### #8 테스트

- 트랜스 코딩된 파일이 저장되는 버킷의 파일명(객체명)을 변경

  ![img](3-serverless-application-constructions.assets/7iitVvKINxbqL1g9C-Ve9aUro7_9bQRSSgFxxq30IZ83OFZRewh1-YakhgOOQ81iWDfkmitPhIcbHwdcnsJyACMcRTxA1akseaQMBjP3qUmRzsWSiZo6dUY5Xe7tg0hFlEOFIouh)



- 동일 버킷에 동일 객체명의 JSON 파일이 생성되는 것을 확인

  ![img](3-serverless-application-constructions.assets/C06c_pen_xUHZ-jG2RNPAEY68nEQoOKtFCOITtM1uax4x9qmjPsEBgDsTrKdoSIxXBVLZjWpeMSqDGjDWXe41yJ_F9uIbWfH0o8e_MS47MVwk_on7TYyERFy6USh1WLLojFfQn0o)

- JSON 파일을 다운로드 후 내용 확인

  ![img](3-serverless-application-constructions.assets/FHTNLiOHlAKUXzAaW1QLYvMQDwXaeuGQZezOCR54f0Ri7eGdeSuvIosvx25LH_Cgah9cIl0IGds4kZNb30MV4W3MmYR6-KLZ4l9BonNoNgbqMotgZ9d1XiuBu7ka7cHeYOtQZFAY)
  
  ![img](3-serverless-application-constructions.assets/baGaFy_ayZUAxhf8FzpYNeUjiD8KEgzVLoKYbljnd-kVRoPJKjB9Q_WjaNtPJYw-w5M9-BMxLJI_O2neDNBDUhU2QHO9p5LxW2oYmVVWhgL6OC3rR2hMt3r1LjkYREO3-6siZGIG)



### #9 파일 종류와 무관하게 이벤트가 트리거되는 문제점을 수정

- 람다의 불필요한 호출

  ![img](3-serverless-application-constructions.assets/Yd0748eogei9YK0EBSqzjrB3r-JfNBG_u8TY_xxU5woHFV20qG5FzIEUytCjy14VUXcQUavN7RBewJA_j3jM_nDj7sUeJohhuy4Q8qE03V6ZkeREzb7kGUX21KZz__5wUIrxjMvq)

- ![img](3-serverless-application-constructions.assets/z8ZN6e4b6VfDlpEg2V40HZ0rkl00RKTNSZMoHsaHBevP6i6uNp5n6_g6A6yIxHOnpUIowJagKUlBx9xavo-ZjLwQB9KPaFkK7U_QJJjaEME1sFjE0O68-YRHZ9cpozUEj4SL8-oG)

  ![img](3-serverless-application-constructions.assets/nTVJM500D-3pRgsb-SfR4FwGjIo0kfuh5xGEj5KaDwoahzo6etjr1PSQPvi3ulWyBYSHevzXNZqUspiFvWOvh8RhzRygUaF7dvbdZwL3c4Exw94Kx-L30p2uwRiLbHkpCNxChZNB)

  ![img](3-serverless-application-constructions.assets/ouQ45a2J3P7-_k2PBUrl_cAxw2-J_lsXXfTyylzaNcoH_0qq0O3nrKk2LLIu3EJBaasrFs_XKC-pmVXi8dU1vhRBswf_MX9KV57ztv3xfU1QNHBgQ9R7IKKMjQYgqLq_hMyXSA8w)







---

<br/>

### 조별 실습

- 조별 활동으로 첫번째 람다 함수(transcode-video)와 세번째 람다 함수(extract-metadata)를 파이썬 기반으로 작성해서 조별로 제출하시오. 

  ```
  제출 형식: 이메일 
  메일 제목: OO조 실습 (예: A조 실습)
  메일 본문: OO조 실습 (예: A조 실습)
  메일 첨부: transcode-video.py 파일과 extract-metadata.py 파일 2개만 첨부
  ```




- transcode-video

```python
import boto3
from urlibe.parse import unquote

elasticTranscoder = boto3.client('elastictranscoder', 'us-east-1')

def lambda_handler(event, context):
    
    print(event)
    	
    key = event['Records'][0]['s3']['object']['key']
    sourceKey = unquote(key.replace("+", " "))
    outputKey = sourcekey.split('.')[0]

    response = elasticTranscoder.create_job(
        PipelineId = '1615427970749-j6u8b9' # 본인 pipeline ID
        Input = {
            Key: sourcekey
            },
        Outputs=[
            {
                Key: outputKey + '-1080p' + '.mp4',
                PresetId: '1351620000001-000001'
            },
            {
                Key: outputKey + '-720' + '.mp4',
                PresetId: '1351620000001-000010'
            },
            {
                Key: outputKey + '-web-720p' + '.mp4',
                PresetId: '1351620000001-100070'
            }
            ]        
        )
    
    return response
```

- extract-metadata

```python

import boto3
import json
import urllib.parse import unquote
import os

s3 = boto3.client('s3')

def saveMetadataToS3(body, bucket, key):
    print("Saving metadata to S3")
    s3.put_object(
        Bucket = bucket,
        Key = key,
        Body = body
        )


def extractMetadata(sourceBucket, sourceKey, localFilename):
    print('Extracting metadata')

    cmd = 'bin/ffprobe -v quiet -print_format json -show_format "/tmp/' + localFilename + '"'

    data = os.popen(cmd).read()
    if data:
        metadataKey = sourceKey.split('.')[0] + '.json'
        saveMetadataToS3(data, sourceBucket,metadatakey)
    else:
        print('stdout error')


def saveFileToFilesystem(sourceBucket, sourceKey):
    print('Saving to filesystem')

    localFilename = sourceKey.split('/').pop()
    with open('/tmp/' + localFilename, 'wb') as f:
        stream = s3.get_object(
            Bucket = sourceBucket,
            Key = sourceKey
            )
        f.write(stream)

    extractMetadata(sourceBucket, sourceKey, localFilename)

def lambda_function(event, context):
    message = json.loads(event['Records'][0]['Sns']['Message'])
    sourceBucket = message['Records'][0]['s3']['bucket']['name']
    sourceKey = message['Records'][0]['s3']['object']['key'].replace('+', ' '))

    saveFileToFilesystem(sourceBucket, sourceKey)
```



- vi lambda_function.py
- zip -r Lambda-Deployment.zip * -x *.zip
- aws lambda update-function-code --function-name arn:aws:lambda:us-east-1:457117745386:function:extract-metadata-python --zip-file fileb://Lambda-Deployment.zip

