## S3 부가 기능

> AWS 기반 서버리스 아키텍처 (p226)



### 버전 관리

- 객체를 덮어 쓴 경우, 이전 버전으로 돌아갈 수 있음
- 버전 관리 기능은 활성화해서 사용
- 버전 관리를 해제할 수는 없음 ⇒ 일시 중지는 가능



### 정적 웹 사이트 호스팅

- 서버 측 코드 실행은 지원하지 않음
- HTML, CSS, Image, JavaScript와 같은 정적인 컨텐츠를 제공(서비스)



### 스토리지 클래스

> 중복성, 액세스 특징 및 가격 정책에 따라 네 가지 스토리지 클래스를 지원
>
> 요금은 저장 비용(스토리지 클래스, 리전, 저장된 데이터의 양)과 요청 비용(요청과 데이터 전송)으로 구분

- Standard
  - 데이터에 자주 액세스할 때 적합
  - 99.999999999%의 내구성을 지원
  - 저장 비용 → 10,000건의 요청당 $0.004
  - 사용 비용 → 처음 1TB의 데이터/월 사용 시 GB 당 $0.0300
- Standard_IA(Infrequent Access)
  - 자주 액세스하지 않은 데이터에 적합
- Glacier
  - 간헐적으로 접근
  - 데이터를 가져오는데 3~5시간 소요
  - 처음부터 Glacier 클래스로 객체를 생성할 수 없음 ⇐ 생명주기 관리 규칙을 사용해서 Glacier로 전환
- Reduced Redundancy
  - 99.99% 내구성 지원



---



## 웹 페이지를 통해 인증된 사용자만 S3 버킷에 파일을 업로드할 수 있도록 제한

> AWS 서버리스 아키텍처 p238

![img](8-storage.assets/4qmj1mL2PfyxlUf7NcTBUOoQO6mTJR7gznlI-P4Qw5ACwZZy-_FEilkXYiGoBZ9LE2FqrJPBzVdK1Hxp0GAJut-6p1KvnVN2u2abtRONOqhvTq5bYsaotGHkfULSXTeu50xMVBLq)

​	![img](8-storage.assets/eXh13-gGhyWAFGs5VNxtCPILeL5zFo7l-kn9smiM4E6_cIKSAIg1WPZZ1fdUl5ugRwkwzPorXPVEnwZDFAiZ8gROvU3KaPKVXffM9DMzC-Wy2YeByiwPOk54HTI812Qcmi4-zgly)

- Lambda 함수 ⇒ 사용자를 확인하고 S3에 파일을 업로드하는데 필요한 정책과 서명을 생성



### 1. upload-s3 사용자 생성 및 업로드 정책 생성

- IAM 사용자 생성

  ![img](8-storage.assets/02UncP7hDzVnRvz1zkGyRSsM02Gcv6kBL7foOVkU4MrLAwT5FAOpRWSpPG5I6hJQq1SzOqKJzj1OId9-xvczwREl7_9VTxrTP07KMmbkrL-SOu-lGdlWUg6qSgiHbOjI1L4JX3Az)

  ![img](8-storage.assets/1AmIeDVj2RZl51YL2ercZjbMO2Pwc5ayD37mkY_-E0pKg1HmZs_v7t4dC-ksjjJl3szshAiX5Esyz1AL9NK_15VmM-FvddednhB9Yl39zoLKM3doTWD3VgXGCGCGQITnD2HYwem3)

  ![img](8-storage.assets/T___PbioEAJJEDGS6Dw35edmje_EfKvWb6jOQhPnX7QZn9nyiv6Vu15oSczcw2c_j8JhCC0m1Ymum2Td_t647HXPN3QIGPMqJDEICVhYcECNeLaqJ7zuD9S2ujudGG0Y_evKIAPp)

  ```json
  {
      "Version": "2012-10-17",
      "Statement": [
          {
              "Resource": "arn:aws:s3:::serverless-video-upload-1yangsh",  // 업로드 버킷의 ARN
              "Action": "s3:ListBucket",
              "Effect": "Allow",
          },
          {
              "Resource": "arn:aws:s3:::serverless-video-upload-1yangsh/",
              "Action": "s3:PutObject",
              "Effect": "Allow",
          }
      ]
  }
  ```

  ![img](8-storage.assets/Isp9fIGs3F4JhesCnGI-itPVvJ4YseextEGcteiS18Rsj1mN_dhYiL0W759d95SKtI6jskY7cEw1jXkIY0-XKAlgnmpbr5U6CEuABiOCWbhL2W3YlMzI-ctYUYnHKhLjBLdFndgn)

  ![img](8-storage.assets/CsX513yYGbUhZ2SUll29ud1CRGUYSm095OWuZFAoqIjGMqx3ZLo38pwAQNyBk-hx7zGO8XENyY62nAciduNRpxiijzUbGrGAnFpnTo5QM72cKFH0P8xnANfDVihkTK4cD0Up9hJb)







### 2. 람다 함수 생성

- 작업 디렉터리 생성
  - `cd C:\serverless\`
  - `mkdir get-upload-policy`
  - `cd .\get-upload-policy\`
  - `npm init -y`
- 모듈 설치
  - `npm install -y async`
  - `npm install -y crypto-js`

- package.json 파일에 create, precreate 스크립트를 작성

  - `package.json`

    ```json
    				:
    "scripts": {
        "create": "aws lambda create-function --function-name get-upload-policy --handler index.handler --memory-size 128 --runtime nodejs12.x --role arn:aws:iam::457117745386:role/lambda-s3-execution-role --timeout 3 --publish --zip-file fileb://Lambda-Deployment.zip",
        "precreate": "zip -r Lambda-Deployment.zip * -x *.zip *.log node_modules/aws-sdk/*"
      },
    				:
    ```

- 람다 함수 작성

  - `c:\serverless\get-upload-policy\index.js`

    ```javascript
    // P242
    'use strict';
     
    var async = require('async');
    var crypto = require("crypto-js");
     
    // 상수를 정의 
    const C_ACL = 'private';    
    const C_NOW = new Date().toISOString();                     // 현재 시간 2021-03-19T02:04:38.030Z
    const C_DATE_STAMP = C_NOW.slice(0,10).replace(/-/g,'');    // 현재 시간을 YYYYMMDD 형식으로 변환 2021-03-19 => 20210319
    const C_REGION_NAME = 'us-east-1';  
    const C_SERVICE_NAME = 's3';
    const C_X_AMZ_DATE = C_NOW.replace(/[-:\.]/g,'');           // 20210319T020438030Z
    const C_X_AMZ_ALGORITHM = 'AWS4-HMAC-SHA256';
    const C_X_AMZ_CREDENTIAL = `${process.env.ACCESS_KEY}/${C_DATE_STAMP}/${C_REGION_NAME}/${C_SERVICE_NAME}/aws4_request`;
     
    // 반환할 오류 메시지 포맷을 정의해서 반환하는 함수
    function createErrorResponse(errCode, errMessage) {
        var response = {
            'statusCode': errCode, 
            'headers': { 'Access-Control-Allow-Origin': '*' }, 
            'body': JSON.stringify({ 'error': errMessage })
        };
        return response;
    }
     
    // 반환할 성공 메시지 포맷을 정의해서 반환하는 함수
    function createSuccessResponse(message) {
        var response = {
            'statusCode': 200, 
            'headers': { 'Access-Control-Allow-Origin': '*' }, 
            'body': JSON.stringify(message)
        };
        return response;
    }
     
    // expiration(정책 유효 기간)을 계산해서 반환하는 함수
    // 다음 날을 ISO 형식으로 반환
    // https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Date/getDate
    function generateExpirationDate() {
        var currentDate = new Date();
        currentDate = currentDate.setDate(currentDate.getDate() + 1);
        //                                ~~~~~~~~~~~~~~~~~~~~~~~~~
        //                                현재 일자 + 1 => 다음 날
        return new Date(currentDate).toISOString();
    }
     
    // 보안정책문서를 생성
    function generatePolicyDocument(filename, next) {
        var expiration = generateExpirationDate();              // 정책 유효기간
        var dir = Math.floor(Math.random()*10**16).toString(16);// 디렉터리 명으로 사용할 난수를 생성
        var key = dir + '/' + filename;                         // 키 이름을 설정
        // https://docs.aws.amazon.com/AmazonS3/latest/API/sigv4-HTTPPOSTConstructPolicy.html
        var policy = {
            'expiration': expiration,
            'conditions': [                                 
                { acl: `${C_ACL}` },                     
                { bucket: process.env.UPLOAD_BUCKET },
                [ 'starts-with', '$key', `${dir}/` ],           // 키 이름이 ${dir}/ 형식으로 시작해야 함
                { 'x-amz-algorithm': `${C_X_AMZ_ALGORITHM}`},
                { 'x-amz-credential': `${C_X_AMZ_CREDENTIAL}` },
                { 'x-amz-date': `${C_X_AMZ_DATE}` }
            ]
        };
        next(null, key, policy);                                // waterfall 함수에 따라 encode 함수가 호출
    }
     
    // 보안정책문서의 포맷을 변경 : 문자열 -> JSON -> 개행문자를 제거 -> BASE64로 인코딩
    function encode(key, policy, next) {
        var json = JSON.stringify(policy).replace('\n', '');
        //         ~~~~~~~~~~~~~~~~~~~~~~ ~~~~~~~~~~~~~~~~~
        //         JSON 형식으로 변환      개행문자를 제거
        var encodedPolicy = new Buffer(json).toString('base64');
        //                  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        //                  BASE64로 인코딩 
        next(null, key, encodedPolicy);                         // waterfall 함수에 따라 sign 함수를 호출
    }
     
    // 서명 키를 생성
    // https://docs.aws.amazon.com/ko_kr/general/latest/gr/signature-v4-examples.html
    function getSigningKey() {
        var dateKey              = crypto.HmacSHA256(C_DATE_STAMP  , "AWS4" + process.env.SECRET_ACCESS_KEY);
        var dateRegionKey        = crypto.HmacSHA256(C_REGION_NAME , dateKey);
        var dateRegionServiceKey = crypto.HmacSHA256(C_SERVICE_NAME, dateRegionKey);
        var signingKey           = crypto.HmacSHA256("aws4_request", dateRegionServiceKey);
        return signingKey;
    }
     
    // 서명을 생성
    // https://docs.aws.amazon.com/AmazonS3/latest/API/sig-v4-authenticating-requests.html
    function sign(key, encodedPolicy, next) {
        var signingKey = getSigningKey();
        var signature = crypto.HmacSHA256(encodedPolicy, signingKey);
        next(null, key, encodedPolicy, signature);
    }
     
    exports.handler = function(event, context, callback) {
        var filename = null;
        if (event.queryStringParameters && event.queryStringParameters.filename) {
            filename = decodeURIComponent(event.queryStringParameters.filename);     
        } else {
            callback(null, createErrorResponse(500, '파일명이 누락되었습니다.'));
            return;
        }
     
        async.waterfall([ async.apply(generatePolicyDocument, filename), encode, sign ], function (err, key, encoded_policy, signature) {
            if (err) {
                callback(null, createErrorResponse(500, err));
            } else {
                var result = {
                    upload_url: process.env.UPLOAD_URI,
                    encoded_policy: encoded_policy, 
                    key: key, 
                    acl: `${C_ACL}`,
                    x_amz_algorithm: `${C_X_AMZ_ALGORITHM}`, 
                    x_amz_credential: `${C_X_AMZ_CREDENTIAL}`, 
                    x_amz_date: `${C_X_AMZ_DATE}`,
                    x_amz_signature: `${signature}`
                };
                callback(null, createSuccessResponse(result));
            }
        });
    }
    ```

    

- 람다 함수 생성 및 배포

  - `npm run create`

    ![img](8-storage.assets/Pg91LZjW3bUY1kwCman6f7PrUySCpRGoaOAEWZDY-Qdv_og34cq0xCsqhWmHAJsxEkctyyrf6FJhcA5aJvcA9kzcHwr8uFHILxQKftp5rRWNZTG3hBqvmTXVHt-8VoGSuK7FU9zh)

  

- 람다 함수 실행에 필요한 환경변수를 설정

  | 키                | 값                                  |
  | ----------------- | ----------------------------------- |
  | SECRET_ACCESS_KEY | 사용자의 secret access key          |
  | UPLOAD_URI        | https://업로드버킷.s3.amazonaws.com |
  | UPLOAD_BUCKET     | 자신의 업로드 버킷                  |
  | ACCESS_KEY        | 사용자의 access key                 |

  

- 람다 함수 테스트

  - 테스트 이벤트 생성

    ```
    {
      "queryStringParameters": {
          "filename": "myvideo.mp4"
      }
    }
    ```

    ![img](8-storage.assets/HrtquRHx5QI1UixAZGFJEfS0LvPVLjeskMeVY2VCADmnsiN5kLrC28MDaqAOF71WYMpog6Nl3bXFbnsq0fepfin6YFNDD_nLmDT1xLc_F_GjRC1Brl6KUr-qzSMZQGMZawPLgvK4)





### 3. API Gateway 설정

- 24-hour-video API 생성

  ![img](8-storage.assets/KE-nSvrg6lfAIKQO1B4_Qz46gCkqfF6JUFHduTeGn1nad_TkF3ZBSeEv6iSIvsOL--3sAgyi2XkhNbjVqpTvOVeP72wjV5MFYg0HB6MQv7zd0V2_tmOPDqOHiWYc3uJS_1X4go_g)

  ![img](8-storage.assets/Bccr-7_gVKxEcDkFnldKSSDU_N95n07sqQGMRj9DyuxKzrd_jSm0qmzdvqhMhzhewgbwGAj0cFhGY4iaUChcKwusSfZa6xTJwB6-wxm3a1WthCZKeF-SIglNZjHZi55rgt3q5uXY)

  ![img](8-storage.assets/IlY_uhTTX2K2J-cuWfJeqmyHQ9uMSsuha97lB5BlyM6EEttStsZ1VBUOal9dMlJ87ONFByde_TWWwJ41ZvpE55VWxgLVyeiUSvTAM4SctCru1Ikw2uhclgDLzvEDxd6ZGZKR9HCR)

  

  

- s3-policy-document 리소스 생성

  ![img](8-storage.assets/LaDKp-njoZtbbtKBJYvAfX9XCSS4KoQDySB6ThZKgV8omcCou-y8Q-kaWaQD-18IBptIWhFY56_GxHueDWDNIMHBjJEvL0ctqY8L1vSs-F_Z1jkmOHHFilrA6jMw1TYD87fCZJ3X)

  ![img](8-storage.assets/HjFwlaU1K26yXdIQ8oGWZ_OZ6XFfOtlGCxGXlBOOveJ7VHSF0TRKrVQyPRcQOIj-WFThzrmPOhI2moMBi6GjYV0HulzkWSbONX_Zr7xc1EWkdRNrozAHGHdmpYMaKWhIOA1Lcog7)

  

  

- GET 매서드 생성

  ![img](8-storage.assets/G6LKNzD8eypzUL7xXrDdB0baRjzDaKeFeWoztcpf9-C4BEURSaOz-XE2bqyyUdo_cxoBO4n7KFRftJ-B7eZ8ZyzYK_Vt9t3mc9pEgriMlr_ugOFN7sCScs2bqf0mIQQUTRsoDqBn)

  ![img](8-storage.assets/8zerbwQ6IkRle_U1xbLNsMZoYtcQO0tBfkWAyBNSXczE8AeSltanKmm1fx7RRaZ0UjhXuuBNzfqMey7czukPYP9L3PtQm3kLjU2M_2IyGxyi5vLB_hl5ZrtdyUO51UIe_5Zrzz-A)

  

- API 배포

  ![img](8-storage.assets/rGgHEMmIlEBILAUO425QB2QjoQ7W4pMx-MT_t4RW0hk-XTRDvJm66M4BGDU3-0HdTTAS--Ro3_l61hfoHvMV5Ec_a-Q0QkeJ_zYXJkeh9_GCV-FS_Ge7QGc30EDaa2mt4502RO01)

  ![img](8-storage.assets/lKR8QKtFLPl1BH7jl6rnmPJOHPmRp7X1mIo90rofBrQwEHoQrsauxjntel3TttuA91-J8YcvULNJJlimisQwVjlWWBtfiIogFO8fNXPaUh5_D_auimN8s7ucEsOnSH39GMPHDOrW)

  ![img](8-storage.assets/bKQlNlBL-soymY8rV6HTvF5abcXD4ihvteWN2OLHscSzTqQp-H7psqsiKRQnr5LN7Ilkwo3J5mWNzDhcixTsbr7-9bj6YZcFuvBj-RcuAzRTjH1AkBiWo_leGeQLLeflEAAnr8TZ)

  

  



### 4. 웹 사이트에 업로드 기능을 추가

- `c:\serverless\24-hour-video\index.html`

  ```html
   <!-- 시작: P249 참고 업로드 버튼 및 진행률 표시 (대략 90번 라인)-->
        <span id="upload-video-button" class="btn btn-info btn-file">
          <span class="glyphicon glyphicon-plus"></span>
          <input id="upload" type="file" name="file"> <!-- 파일 선택 창 -->
        </span>
        <div class="progress" id="upload-progress">
          <div class="progress-bar progress-bar-info progress-bar-striped" role="progressbar" aria-valuemin="0" aria-valuemax="100"></div> <!-- 진행률 표시 -->
        </div>
        <!-- 끝 -->
  ```

  

- `c:\serverless\24-hour-video\js\upload-controller.js`

  ```javascript
  var uploadController = {
      data: {
          config: null
      }, 
      uiElements: {
          uploadButton: null
      }, 
      init: function(configConstants) {
          this.data.config = configConstants;
          this.uiElements.uploadButton = $('#upload');
          this.uiElements.uploadButtonContainer = $('#upload-video-button');
          this.uiElements.uploadProgressBar = $('#upload-progress');
          this.wireEvents();
      }, 
      wireEvents: function() {
          var that = this;
   
          // 파일 선택창의 내용이 변경된 경우 수행할 기능을 정의
          this.uiElements.uploadButton.on('change', function(result) {
              // 선택한 파일 정보를 가져와서 변수에 할당
              // file = {name: "수행평가 - 1조.pdf", lastModified: 1616109336996, lastModifiedDate: Fri Mar 19 2021 08:15:36 GMT+0900 (대한민국 표준시), webkitRelativePath: "", size: 5272297, …}
              var file = $('#upload').get(0).files[0];
              // 24-hour-video API의 s3-policy-document 리소스에 filename 파라미터의 값으로 선택한 파일의 이름을 전달
              var requstDocumentUrl = that.data.config.getUploadPolicyApiUrl + '/s3-policy-document?filename=' + encodeURI(file.name);
              $.get(requstDocumentUrl, function(data, status) {
                  // file: 선택한 파일 정보
                  // data: get-upload-policy 람다 함수의 실행 결과 = S3 버킷 업로드 정책 문서 
                  that.upload(file, data, that);
              });
          });
      }, 
      // 파일 선택창에서 선택한 파일을 S3 버킷으로 업로드 
      upload: function(file, data, that) {
          // 파일 선택창을 숨기고, 진행률 창을 보이게 처리
          this.uiElements.uploadButtonContainer.hide();
          this.uiElements.uploadProgressBar.show();
          this.uiElements.uploadProgressBar.find('.progress-bar').css('width', '0');
   
          // 요청 본문을 생성
          var fd = new FormData();
          fd.append('key', data.key);
          fd.append('policy', data.encoded_policy);
          fd.append('acl', data.acl);
          fd.append('x-amz-algorithm', data.x_amz_algorithm);
          fd.append('x-amz-credential', data.x_amz_credential);
          fd.append('x-amz-date', data.x_amz_date);
          fd.append('x-amz-signature', data.x_amz_signature);
          fd.append('file', file, file.name);        
          $.ajax({
              url: data.upload_url,
              type: 'POST', 
              data: fd, 
              processData: false,
              contentType: false, 
              xhr: this.progress,
              // ajax 요청을 전달하기 전에 수행해야 할 작업을 명시
              // Authorization 요청 헤더의 값을 제거
              beforeSend: function(req) {
                  req.setRequestHeader('Authorization', '');
              }
          })
          // 업로드에 성공한 경우 수행할 내용
          .done(function(response) {
              that.uiElements.uploadButtonContainer.show();
              that.uiElements.uploadProgressBar.hide();
              alert('업로드 성공');
          })
          // 업로드에 실패한 경우 수행할 내용
          .fail(function(response) {
              that.uiElements.uploadButtonContainer.show();
              that.uiElements.uploadProgressBar.hide();
              console.error(response);
              alert('업로드 실패');
          })
      }, 
      // 진행율을 표시
      progress: function() {
          var xhr = $.ajaxSettings.xhr();
          xhr.upload.onprogress = function(evt) {
              var percentage = evt.loaded / evt.total * 100;
              $('#upload-progress').find('.progress-bar').css('width', percentage + '%');
          };
          return xhr;
      }
  }
  ```

  

- `c:\serverless\24-hour-video\js\config.js`

  ```javascript
  var configConstants = {
      auth0: {
          domain: '1yangsh.us.auth0.com',
          clientId: 'Bkia8NNISRVmEw6M7lANfYmvoutaOVdD'
      },
      // user-profile API 호출 URL
      apiBaseUrl: 'https://tzecikd5j7.execute-api.us-east-1.amazonaws.com/dev',
  
      // get-file-list API 호출 URL
      getFileListApiUrl: 'https://x9p0er0szb.execute-api.us-east-1.amazonaws.com/dev',
  
      // 추가 get-upload-policy API 호출 URL
      getUploadPolicyApiUrl: 'https://6bech3wnhe.execute-api.us-east-1.amazonaws.com/dev'
  };
  ```



- `c:\serverless\24-hour-video\js\main.js`

  ```javascript
  // 즉시 실행 함수
  (function() {
      // 해당 웹 페이지 문서가 로딩되면 설정 정보를 가져와서 설정
      $(document).ready(function() {
          // user-controller.js에 선언되어 있는 userController 객체의 init 메소드를 호출
          // coonfig.js에 선언되어 있는 configConstants 객체를 인자로 전달
          userController.init(configConstants);
          videoController.init(configConstants);  
          uploadController.init(configConstants); /* 추가 */
      });
  })();
  
  ```

  

- `c:\serverless\24-hour-video\index.html`

  ```html
          <!-- (생략) -->
          <script src="https://cdn.auth0.com/js/lock/11.27/lock.min.js"></script>
          <script src="js/config.js"></script>
          <script src="js/user-controller.js"></script>
          <script src="js/video-controller.js"></script>  
          <script src="js/upload-controller.js"></script><!-- 추가 -->
          <script src="js/main.js"></script>
          <!-- (생략) -->
  ```

  

- `c:\serverless\24-hour-video\css\main.css`

  ```css
  /* 업로드 버튼 및 진행률 표시를 위한 스타일을 추가 */
   
  #upload-video-button {
    /* display: none; */
    margin-bottom: 30px;
  }
   
  .btn-file {
    position: relative;
    overflow: hidden;
  }
   
  .btn-file input[type=file] {
    position: absolute;
    top: 0;
    right: 0;
    min-width: 100%;
    min-height: 100%;
    font-size: 100px;
    text-align: right;
    filter: alpha(opacity=0);
    opacity: 0;
    outline: none;
    background: white;
    cursor: inherit;
    display: block;
  }
   
  #upload-progress {
    display: none;
  }
   
  #video-list-container {
    text-align: center;
    padding: 30px 0 30px;
  }
   
  .progress {
    background: #1a1a1a;
    margin-top: 6px;
    margin-bottom: 36px;
  }
  
  ```





### 5. 파일 업로드 테스트

![img](8-storage.assets/qImKlpNMQtQo2RY2e1VxyIlTGKu0k9EuTTH7AJza9cB5jkoewfar3x5dKf7Xa3ekbkIOJV6w8zdyvhW3S6ElmKp4XUbHAU1djpubUJBsPazH-nKXqvyFKkUB9otoNuJmO9zspF-I)

- CORS 오류 발생

  - ⇒ 24-hour-video API의 CORS 활성화 후 재배포

    ![img](8-storage.assets/SV-ncJ4OfTi48lvpcX3j76NCDM_IJc4W-AwMxYWKLcJaiEq-84p7ZTwC56vg_329pHRHSW0iETfDAes83M3DnZVPA2Py8c4DtvSFcJrITPe-d99Adat63AJ8jcMBkkGNNnHnosbq)

    ![img](8-storage.assets/-pIe4Wmorwva0x6myd0XOZF0t8WFqsUBbRot45yGcGmFQ3JJDo9hpBS6dyxynXJvPMaxPqgbAGsKeRDaDdEvF12JhGYDF9XTjfPPi9PceUdkrW5PoiCyHOtnGnZ01LhfnO3We1ja)

    ![img](8-storage.assets/2eDeh6CpQ8TXn7VO-rYm-SBtBXmGNdYvhbz7u2wpaTcJAYNznr42zTA2viPcynCWZEtuUBMDrfgVy4XaVpeIjJj9ZecVsSfyPdfcUosU5LgWg38kq_onuacZr87ErGbDFFsppjX-)

    ![img](8-storage.assets/TmgpnhruL6OaOqCagvNz0cnibxWeJ339gOBOkE0WIkbFix4L2xFdjwfr7Ycf94Uxcr1nyI9rnNFi142uKtDzV7mxBVs5r5XgBk11hOr8M_yitWtw0IgGg1Ue4qw5UZOS3BdfGoSJ)

    

  - ⇒ AccessToken 헤더를 허용하도록 CORS 설정하지 않아서 CORS 오류가 발생

    ![img](8-storage.assets/pjHwglt7IHMNBxqgvrKM2fFi5yn8c0uSgyHJTdKWMDZ3xwoqEms9ApFlidgdTOlL2gDT7RQDW_7E7j1NKTCl0ogdVmbjeM0l8_YJ5WHK50MwJcFwufG_xS_wQmhGeX-_P7X56xd5)

    - AccessToken 추가

    ![img](8-storage.assets/p6Z3fcDdhwIBx_4mj-HRUbbo090BNFke2_OULSR6o9utrJ12Skx7apoDuiWyN4rTwcLYZTSRfAkTdswkH3fYN7bF3JgG5DOsGOslLCoG49oPBr9S-M0qkJ5gy3B-BURATjVwO0so)

    ![img](8-storage.assets/nDu09BJcgVsA8WoQPNpDc_rl_ygonRcTJddp2EgCkHvdpyjwJy3XGklXx5HCr0McumgWn6E_3xqTQTjFONGFs52YVCbZ_RSS_9PA99MbTE7J564oXu6K-I2Q3GYq5jrrStCHrEB1)

    

  - "업로드 실패" ⇒ 파일을 S3 버킷에 업로드했을 때 나오는 메시지 ⇒ s3-upload-policy 람다 함수 호출(실행)은 완료되었음을 의미

  - ⇒ S3 버킷에 CORS 설정이 필요

    

  

### 6. 업로드 S3 버킷에 CORS 설정

> 참고 ⇒ https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/cors.html

- 업로드 버킷 -> 권한 -> CORS -> 편집

  ![img](8-storage.assets/jzK_bi6X-__oVqNqbzTHsrDz_vSGJww9olY22YUmvmGXjluffF8VjYIHneFvvY2Mznmp_owvAOE1tW3w3NA1RBAZHSwFBABzBfG_nqwqALJA6u4os9S74j1eocyLT2mTK4j7swxp)

  ```
  [
      {
          "AllowedHeaders": [
              "*"
          ],
          "AllowedMethods": [
              "POST"
          ],
          "AllowedOrigins": [
              "*"
          ],
          "ExposeHeaders": [], 
          "MaxAgeSeconds": 3000
      }
  ]
  ```

  ![img](8-storage.assets/bXDEqRjQrImgtvdI4RK6qJNKFgxuJ-yMJBktBzMo8YhksqOJ18bPVgNDUyPJ6y55p0VtCb0qKfR7PxHmVxvHyVj5aJNi6WQgCT6f4j6TGMJzZ0sI3jM7qOM70yjx5XGNr32EZxRI)

  

  

### 7. 파일 업로드 테스트

![img](8-storage.assets/Td_SC8v8FnSd4I0zAHL5MwYYFmYBpl0bstQY1EF999YxyenTb5sfZyzS-Jxwxn3X2qYX5Hx-CnVb8HE96EeJd25uQ_TTbq5SEo6iNzu_cnKoskZryNerha9BKP6_12uuCLYd41Ln)







