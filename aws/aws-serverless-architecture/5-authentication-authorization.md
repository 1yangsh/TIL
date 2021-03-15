<br/>

## 서버리스 환경에서의 인증(p108)

> Amazon Cognito, Auth0, 위임 토근 등으로 구현 가능

- 인증
  - Cognito, Auth0
- 서비스 간 사용자 정보 교환
  - JWT(Json Web Token)



![img](5-authentication-authorization.assets/Fnmu_IpqzjsvCmtTErQ2y8lbszHSLctsZi65YuXy8M2wdWBIemXeH39Exe_ULiCwFYHrSfqUvZgEPDC6JbRkFreAcAExZik__gk-D3UVxn3WgdbrXEBHIsfD9Zs3ST7MjYdI_Zfr)





---

<br/>

## 실습 : 24-Hour Video 사이트에 로그인/로그아웃, 사용자 프로필 기능을 추가

![img](5-authentication-authorization.assets/Mk1vHm2pP47UomGFUAqqBElmwR7NPL4SlZPwd_HcK_8cWjW-k1IYtX4mxYyf5rLzbU1jKrL3t7CjziDi5GESmp6geqzffbMsnn0dpdDF2TAux8JYhOCZN2WUqImW05TGsrXiFm39)

### 수행 순서

1. 로그인, 로그아웃, 사용자 프로필 버큰을 포함한 기본 웹 사이트를 생성
2. Auth0 이용해서 어플리케이션을 등록하고 웹 사이트에 통합 → 사용자가 Auth0를 통해 로그인하고 이를 식별하는 JSON 웹 토큰을 받을 수 있어야 함
3. 웹 사이트에서 람다 함수를 호출할 수 있도록 API Gateway를 추가
4. user-profile 람다 함수를 생성
5. user-profile 람다 함수를 호출할 수 있도록 API Gateway를 구성
6. JWT 토큰 유효성 검사를 수행하도록 API Gateway를 수정



## 24-Hour Video 웹 사이트 제작

### 1. 프로젝트 생성

- `cd C:\serverless\`
- `mkdir 24-hour-video`
- `cd 24-hour-video`
- `npm init -y`
- `npm install local-web-server --save-dev`



### 2. Visual Studio Code 실행해서 start 스크립트 추가

- `package.json`

  ```json:
  	:
  "scripts": {
      "start": "ws"
    },
  	:	
  ```

  

### 3. UI 템플릿 다운로드

- 링크 : http://www.initializr.com/

  ![img](5-authentication-authorization.assets/fxuozGOoi_DLdbzjUrgFjkX5x9flaABOWcbFJYHSKfn8v97XIDGAkS0Vmqb5LrETOsQeVzx7RVxf6w0ykT8CKWJlUgP_naTRFzFmPN_HfxRgcEpyrqkUJJdfBiGriaLW40P-n8hd)

  

### 4. 다운로드 받은 템플릿 파일을 프로젝트 폴더로 압축 해제

![img](5-authentication-authorization.assets/kdFhqN9cd021OxAzyBGa0Vmf9WpA_uS_jUUEeszDJ4-r2dPG0zK8JuGRYy49JKFe25QTvMTaDc3OKXPJs0TWahipdXJIsxTyuZkyq3e6frCRnDDJvBfDSXxhYdZ-I3S0qq1FKsGR)

![img](5-authentication-authorization.assets/p6RMbytSMGGNHkchyrlI-w-fUsklsemwlwEglGkvDXJen5Os7uCoSjsMLhhSizzTnMlWfKr03UBdsz7iJPETFdoLzY91LAIvH_FyS2JQLGRWopG7xe2lGy3_WbZ0jVF8uvyiXl2E)



### 5. 웹 서버를 실행하고 확인

- `npm start`

  - localhost:8000으로 접속

  ![img](5-authentication-authorization.assets/r8zeoF8xZ-cakDkv82rq89yPAO7Qr6QsVfnVHCU8kM9PMSJ0HshVNprW4sW5gsFTSOzIWrl8WA-wcSQ3tP-HFRSwE8HdNAHiJZxVulnICY0eZV-JWfC1ut1DRDS_Cbv2oCeMsX84)



## Auto0 구성 (p116)

### 1. https://auth0.com/ 사이트에 회원 가입

- Tenent Domain에 본인이 기억할 수 있는 이름을 입력
- REGION을 US로 설정



### 2. Application 생성

- Create Application

  ![img](5-authentication-authorization.assets/i_Xsavp-roTIsf4sD1r84tC-VF4p6sJ20Dg6br-VlclmL_n1q7GXwlnWiybTqiLeCRUaEqWPLHvZalZ2aD2sypX_rpmETY4o8Ar7k_RVKNwDaHXnISk31lpwxpOLw7SOpo6k_OBU)

  ![img](5-authentication-authorization.assets/xGhp8M45YRTgYDDyVxxGmhJhUWYnfw-RX4y3YISsHAYtZ1A7DgoojYhHYCzKtEOvOxWmbEigQJpeaQmbjoBUH69HhhuwBeHWgp8-zYggtGz8mUCHwW8WUL_5uJqatFTXahnXUIyt)

  ![img](5-authentication-authorization.assets/WyAV9euY0hv-d99uacbngpa1KFr-9f_KQzAzUUrSBQ63RVlxTq-VDUv4fBUU55PpX8i42YmN86_0ZSUP56PtA-TkpKyuVOKlx6DEEBJ7Hwgs2VPkJWsUq2zXoZkhQpicIwYI_Cvh)

  ![img](5-authentication-authorization.assets/QNNAWRKPCLkQXC0ehw6syRZNumNQh6--zO00qPmH-XyxACj0i32sFlPRx_zWWGyIM0XRlNJ9-1aeuciADnqfsXRQ-phSag58m7MKAurUFZs0AbFP8K0EDEYk9P2W51FQ-79zqc6k)

  ![img](5-authentication-authorization.assets/Ypu04QBwVh7Fpv1YPCxwlEAzI90CHoERZG1cMukeeS17tcul7tUHM4GvXnAl88ffgdQ-vGwvZPBqZovr8bJjoqCBBgVuCF6UwUuQhBuwEj9l1fHcVUrWcGdppkwYk54O6TJvhvb2)

  

  

### 3. GCP(Goole Cloud Platform)에 사용자 인증 정보를 추가

> https://console.cloud.google.com

- GCP에서 새 프로젝트 생성

  ![img](5-authentication-authorization.assets/WS0TliUQVckMqolPkHxzueQzDN9CFBrvUS_x0ZSHPxWmimF9pPrhpPkjXGNAoRGHknnlgwPknmXsPWOiSMuJjdD-jTkv_Sq-ktz7mA-TqDUiatZuEBiewnNpixFpoBh8k2KC_sSG)

  

- OAuth 동의 화면 생성

  ![img](5-authentication-authorization.assets/PB92UMsJ8UnstZXA1I7XYRT92EiGB8wecX3MMFI3Bs_QSwWvNV0wLQjMnJSp6s0ppmFK62A7ipxBNWOsvGrNm0PYYvZtK1BKsnBOp_UJ2iEP9FWsplINm6KhdHb35jy4CzI4g4im)

  ![img](5-authentication-authorization.assets/XQHg_HZT40HI_eFgd2qE7kd4PUmK2hpgwMEGGgyIzJS7fZR5AFFSzquaU9mGQUbSwbJUoZtM947J7WULUVgBLaEsaAV-PRv9d2qNyfDi3f68hopMMWL9VbHl8vOR5KLpnKt_oQuC)

  

  

  ![image-20210315145224689](5-authentication-authorization.assets/image-20210315145224689.png)

  - 이후부터로 디폴트 상태에서 [저장 후 계속]으로 진행



- 사용자 인증 정보 생성

  ![img](5-authentication-authorization.assets/R7cvoRk-WrYR-bZajYMhJ2Cgf4pcTwYhPxAZa67JDr9m7H0BRHQs9d6nKbx28iCMYdv1-Ph8StK83qAIhBXYFYDSBXoGeTtfmai4PGG8LhP5RRcqs9zlaVcJY_cUTJn7bDgeqgiE)

  ![image-20210315150006996](5-authentication-authorization.assets/image-20210315150006996.png)



### 4. Auth0에 Social Connection으로 Google-oauth을 추가하고 테스트

- Auth0 - Connection - Social

  ![img](5-authentication-authorization.assets/7OFVFrQYobpp0Nzp9X5kfnVCii1ddgHzLyE61ul43asvnYAFBCqAYQeZWNil4mJPC5qaX58Ndd0GUSRbF8njgBTp04zqJGzW9iQsQGL-F64TNQi1SFhKCrqiOM2NPkUBvOvR1esd)

- GCP 사용자 인증 정보 생성을 통해서 만들어진 Client ID와 Client Secret을 입력

- 추가한 후 Try Connection

  ![img](5-authentication-authorization.assets/7xXr9_mP55AUPsfnekPSbqCDsJgVOXsVzIDXP3HFXVnatiShNX0gX4EustW-FlsePdLp5bbJax2HbZXUixw9N1sCRWbveU1g6cWvsVOfKxLVq7rRDpThMqrxwuGS3VmXFKf5TKto)

  ![image-20210315151336325](5-authentication-authorization.assets/image-20210315151336325.png)

  

  

### 5. Facebook for Developers 사이트에서 Facebook 로그인 앱 추가

> https://developers.facebook.com/apps/

- Facebook for Developers 앱 만들기

  ![img](5-authentication-authorization.assets/lgj_oMop3dh2ks1uH0p7sTSIJRtA83SNQgdhdrjNkIi_64VktSSgE_aBlfXRspCjFRkq1RaPlUhIe-cVGKZQUEfBG8MS-SB6_HF2pUtnW48-mKp8-Y0xdEOuKGjuxkJphgh2o_aN)

  ![img](5-authentication-authorization.assets/1d7xAKBbcZ0mwxSeJcn89jyF-iqEK5vj2F7tOmKppkbtPfIlbePFw73YZ7oLvyLub5n6ZA2GE4dGDa9f09fYR0tZjai3E8KqlVxCp2SOEdLRI8oLhnv2AM2wlMMMWcLIHeMVeXqc)

  ![image-20210315154846760](5-authentication-authorization.assets/image-20210315154846760.png)

  ![img](5-authentication-authorization.assets/rKgtSGBfrlyx_qEE_QmSufKXDF_8_h-Jm91-QXvk6wuBS1iqt8ViYyGoCGxT7GB_NI8wb8ylvxNtguBZj--DlwP6VHJl9E4drY4yqxdvjb8MwWfhrWJf_5-XJK-SFLbR0x5-bKUa)

  ![img](5-authentication-authorization.assets/kgutD5NnUdE136jBaN7fBvOQKadjHKqZhzVr6y9f95vFfDEKAp9vkfRbUpDJ-aGGjm-uMv79OOai-UWgJyqaKkzMekgcT87VABdaejdUUTLrG5Rj50C8826wkazJ0-hM9UOl0Enb)

  ![img](5-authentication-authorization.assets/_SK2sERbrShCMIcgwcmIBbt81Oh5ymCqCkBLh7dTh3FTdL5LCvs-b-M836d9ibgo59bINp8s3DmhwttfqF9koQURacRxllVI8UPlXxoOTKL9WFSSye-ycs4nroRIvNKX-NzK9vye)

  ![image-20210315155350386](5-authentication-authorization.assets/image-20210315155350386.png)

  

  

  

  



### 6 Auth0에 Social Connection으로 Facebook 추가 테스트

![img](5-authentication-authorization.assets/mYini4kh57JLFZNQ7RrvWCVAgyrO8_qfxmbD3iu3A-n4fOEivRaRfpLUawVnrzxWwohHC4Cv_TqTMsah8mdHEr0MeBsb8PK6hsw134AnETjacUpOsXHkZdtgi-nuUmU7xBPaCjE7)



-  Client ID와 Client Secret을 입력

- Try Connection

  ![img](5-authentication-authorization.assets/OfjCacizLrxvXn5ClacqlhceMm15SSng8rl5Y98IndWqwzp9SoffZFCTZcfdMXWU_dfV_lSBYVGUKXp1aO5VjDBUGz4XgMxeyitowiJEZ8dfMf06s7uB-bw3sd_aAwrOYuRoMyOB)

  ![image-20210315160124308](5-authentication-authorization.assets/image-20210315160124308.png)

  

  

  

  ---

  

## 웹 사이트에 Auto0를 연결 (p119)

### 1. Auto0 Lock 스크립트, Domain, Client ID를 확인

- Universal Login 확인

  ![img](5-authentication-authorization.assets/8CY2K93-U1qpaILeDvmNr_hTitfula3KSwXqdsF0iqGAHgPYOJ3xU_e1V2Ve5mjYJMb36R5ghhGBiUSZeyi2Ybk3ctfT3bdXIvYwuKroUi_5EYriLb80q9FM3_HoFXcgh2HE-B1b)

  - <script src="https://cdn.auth0.com/js/lock/11.27/lock.min.js"></script>



### 2. Auto0 Lock 스크립트 및 로그인/로그아웃/프로필 버튼 추가

- Auto0 Lock

  - 로그인, 회원가입 등의 대화상자를 제공하는 Auth0의 무료 위젯

- `c:\serverless\24-hour-video\index.html`

  ```html
        :
          <div id="navbar" class="navbar-collapse collapse">
            <div class="navbar-form navbar-right">
              <button id="user-profile" class="btn btn-default">
                <img id="profilepicture" />&nbsp;<span id="profilename"></span>
              </button>
              <button id="auth0-login" class="btn btn-success">Sign in</button>
              <button id="auth0-logout" class="btn btn-success">Sign out</button>
            </div>
            <!--
            <form class="navbar-form navbar-right" role="form">
              <div class="form-group">
                <input type="text" placeholder="Email" class="form-control">
              </div>
              <div class="form-group">
                <input type="password" placeholder="Password" class="form-control">
              </div>
              <button type="submit" class="btn btn-success">Sign in</button>
            </form>
            -->
          </div><!--/.navbar-collapse -->
          :
  		:
          <script src="js/vendor/bootstrap.min.js"></script>
  
          <script src="https://cdn.auth0.com/js/lock/11.27/lock.min.js"></script>
          <script src="js/user-controller.js"></script>
          <script src="js/config.js"></script>
          <script src="js/main.js"></script>
  
          <script src="js/main.js"></script>
  		:
  ```

  

- js 디렉토리 안에 3개 파일 생성

  ![img](5-authentication-authorization.assets/fSIH8-8T6XfFMc6vnhHdzHxWiP1zp_GGwxAG86BhNS238MVDj9iTFZ5YxQCNFl_t3jJwsHwow-mMlgDAW1IemJqIoftVWK9jF8qC1kS3YJ3_K8PgBBdXYAn0mCIBkKTVuF_cbOwi)

  

- `user-controller.js`

  ```javascript
  // user-Controller 객체를 선언
  var userController = {
      data: {
          auth0Lock: null,
          config: null
      }, 
      // HTML 문서에서 제어할 요소들(버튼, 이미지, 라벨, ... 등)
      uiElements: {
          loginButton: null,
          logoutButton: null, 
          profileButton: null, 
          profileNameLabel: null,
          profileImage: null
      }, 
      // 설정 정보와 제어할 요소들을 초기화
      init: function (config) {
          var that = this;
  
          // HTML 문서에서 id 속성의 값이 auth0-login인 요소를 가져와서 loginButton 변수에 할당
          // <button id="auth0-login" class="btn btn-success">Sign in</button>
          this.uiElements.loginButton = $('#auth0-login');
          this.uiElements.logoutButton = $('#auth0-logout');
          this.uiElements.profileButton = $('#user-profile');
          this.uiElements.profileNameLabel = $('#profilename');
          this.uiElements.profileImage = $('#profilepicture');
          
          // config.js의 포함되어 있는 설정 정보를 변수에 할당
          this.data.config = config;
   
          var auth0Options = {
              auth: { 
                  responseType: 'token id_token'
              }
          };
          this.data.auth0Lock = new Auth0Lock(config.auth0.clientId, config.auth0.domain, auth0Options);
          
          this.configureAuthenticatedRequests();
          
          var accessToken = localStorage.getItem('accessToken');
          if (accessToken) {
              // 사용자의 profile을 조회
              this.data.auth0Lock.getProfile(accessToken, function (err, profile) {
                  if (err) {
                      return alert('프로필을 가져오는데 실패했습니다. ' + err.message);
                  }
                  // 사용자 프로필 조회에 성공하면 프로필 정보를 showUserAuthenticationDetails 함수로 전달
                  that.showUserAuthenticationDetails(profile);
              });
          }
          // 이벤트 핸들러를 정의
          this.wireEvents();
      },
      // 로컬 스토리지에 저장된 IdToken, accessToken을 Authorization, AccessToken 요청 헤더의 값으로 설정
      // => 요청 헤더의 값으로 설정되려면, 로컬 스토리지에 해당 값들이 존재해야 함
      configureAuthenticatedRequests: function() {
          $.ajaxSetup({
              'beforeSend': function (xhr) {
                  console.log(xhr);
                  xhr.setRequestHeader('Authorization', 'Bearer ' + localStorage.getItem('idToken'));
                  xhr.setRequestHeader('AccessToken', localStorage.getItem('accessToken'));
              }
          })
      }, 
      // 전달받은 프로필 정보를 사용자 화면에 출력
      showUserAuthenticationDetails: function(profile) {
          // 프로필 정보 여부를 참, 거짓으로 설정 => profile이 있으면 true 없거나 정의되지 않으면 false (bool타입으로 표현)
          var showAuthenticationElements = !!profile;
          // 프로필 정보가 존재하면 사용자 이름과 사진을 출력
          if (showAuthenticationElements) {
              this.uiElements.profileNameLabel.text(profile.nickname);
              this.uiElements.profileImage.attr('src', profile.picture);
          }
          // 프로필 정보가 존재하면 login 버튼을 감추고 logout 버튼과 profile 버튼을 나타나게 처리
          this.uiElements.loginButton.toggle(!showAuthenticationElements);
          this.uiElements.logoutButton.toggle(showAuthenticationElements);
          this.uiElements.profileButton.toggle(showAuthenticationElements);
      }, 
      // 특정 이벤트에 반응하는 함수를 정의
      wireEvents: function() {
          var that = this;
          // auto0 lock에서 제공하는 로그인 창에서 authenticated 이벤트가 발생하는 경우
          // 수행할 함수를 정의
          this.data.auth0Lock.on('authenticated', function(authResult) {
              // 로그인에 성공하면 accessToken, idToken 값을 로컬 스토리지에 저장
              console.log(authResult);
              localStorage.setItem('accessToken', authResult.accessToken);
              localStorage.setItem('idToken', authResult.idToken);
              
              // 로그인에 성공하면 사용자 정보로 조회
              that.data.auth0Lock.getUserInfo(authResult.accessToken, function (error, profile) {
                  // 사용자 정보 조회에 성공하면 반환받은 profile 정보를 showUserAuthenticationDetails 함수로 전달
                  if (!error) {
                      that.showUserAuthenticationDetails(profile);
                  }
              });
          });
  
          // login 버튼을 클릭했을 때 처리 -> auto0 lock에서 제공하는 로그인 화면을 실행
          this.uiElements.loginButton.click(function(e) {
              that.data.auth0Lock.show();
          });
  
          // logout 버튼을 클릭했을 때 처리 
          // -> 로컬 스토리지에 저장된 accessToken, idToken을 삭제
          // -> logout, profile 버튼을 숨기고 login 버튼을 나타나게 처리 
          this.uiElements.logoutButton.click(function(e) {
              localStorage.removeItem('accessToken');
              localStorage.removeItem('idToken');
              that.uiElements.logoutButton.hide();
              that.uiElements.profileButton.hide();
              that.uiElements.loginButton.show();
          });
      }
  };
  ```

- `config.js`

  ```javascript
  var configConstants = {
      auth0: {
          domain: '본인의AUTH0의도메인',
          clientId: '본인의AUTH0의클라이언트ID'
      }
  };
  ```

- `css/main.css`

  ```css
  var configConstants = {
      auth0: {
          domain: '본인의AUTH0의도메인',
          clientId: '본인의AUTH0의클라이언트ID'
      }
  };
  ```

- `js/main.js`

  ```javascript
  (function() {
      $(document).ready(function() {
          userController.init(configConstants);
      });
  })();
  ```

- 작성한 후 auth0 callback url로 등록해놓은 `127.0.0.1:8000` 접속, Sign in 시 로그인 창 확인

![image-20210315164423521](5-authentication-authorization.assets/image-20210315164423521.png)

####  <a href="https://auth0.com/docs/libraries/lock/lock-api-reference">Auth0 Lock API Reference 보기</a>



- 로그인에 성공하면 아래와 같이 프로필과 로그아웃 버튼이 출력

![image-20210315174646202](5-authentication-authorization.assets/image-20210315174646202.png)

![image-20210315174718605](5-authentication-authorization.assets/image-20210315174718605.png)



