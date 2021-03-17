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





# 추가 : Kakao Social Login 기능 추가

> https://developers.kakao.com/

![img](5-authentication-authorization.assets/AE70UpXSLAsNq2C1NJ-9LZkV6BUCltotZ70xi-TVngeBpuXSEemd4hkSfbF8h9C_ddV_gnpaaxG-ICEHiBSbSaAxvdM_ibiVBqKK0QoyK2cn8m_TjIV8hhlumAMaoQzKP03m7l2Y)

![img](5-authentication-authorization.assets/6nHH_gJNi4zEZdIANYiVR39YIi14IlC-vLf-uFgum4RZPaiDKxDPZ83myoxtxu2CwzC8dDvlnKHOhpnI5oHtB4uXUk_kv-KxZWi2efVPXwJAYnTMa8PAgYLI1kezbwhmWTePmoXL)

![img](5-authentication-authorization.assets/wflsJakknp1RehhOHMLNsRa5FucPqM7GJ0Mc7MAdk35wUePlve5jhi9zwmyPpiEfssO5_apPnNdMu1ndwJtuH6G3lmw6qZssgx5FXrzVKMQ47EvvjT6eJ1Mj5dEMyiWbOzNzbfTI)

![img](5-authentication-authorization.assets/aoM1BSmbPkvkrvU6LiEDGKnlXBmPXBqkDQuEi-lz4GurJ9xd1Xjiu2GvfYOXeEWZ4PiGFtgIjlPJ20-hHssACDvKxl8x5XPLmsOTifb2YLBJdcKn2Hy4j5HVDW-IOEdv1rPQLanp)

![img](5-authentication-authorization.assets/NzeG9OkF6odbDVsZ-rNszmLkDLB1c2y1x0mUPZ4cakkVOzbIZmoe0grwNQWqcDQeLLoBDEweAaB6GjMLmbJMHDcPL8MjnhgSm_nhKHxZEAcuLrw4F93kxOejCrsY4V92UlfiDIJh)

![img](5-authentication-authorization.assets/rEBqMPWg2jV1wOEFDtOa79NzGcv5btV8sLaB7i69kS18FosbvMrHjVNZ-DXCdeUIKn_lVSUVDb90EUjaz4230ok_F7FLEwM4IWcyYI30YXPzxtE-tOXIYcZd-vZEPvFHHzE1HnLH)

![img](5-authentication-authorization.assets/iYGkpFLJz3QLevbOs5oVtDRFVpTNcM_i4DGaBTY0uzq0r2ILx5XosEyKMpp6f0D08iziVtyp4qa8Y1w1ft7Zco3b8eVjcpRlox-rhQX1XqilDRsK7KHUIyoSH2WhwPfzn2tfwAK8)

- 실습 : Github 등 다른 소셜 플랫폼 연동



---



### Storage ⇒ HTML5에 추가된 기능

> https://developer.mozilla.org/ko/docs/Web/API/Web_Storage_API/Using_the_Web_Storage_API

- 브라우저에서 쿠키를 사용하는 것보다 훨씬 직관적으로 **key/value 데이터를 안전하게 저장**할 수 있는 메커니즘을 제공

- **sessionStorage**는 페이지의 **세션이 유지되는 동안 사용**할 수 있는 각 origin별로 별도의 스토리지를 관리합니다. (페이지 리로딩 및 복원을 포함한, 브라우저가 열려있는 한 최대한 긴 시간 동안)

- **localStorage**도 같은 일을 하지만, **브라우저가 닫히거나 다시 열리더라도 유지**합니다

  ![img](5-authentication-authorization.assets/6rnuXCeYqrdENZJaAqkYtQU_403lPhWLUbykpKu830x5-WLgomnUgyHQ0TSpio65fl0YN3D6wI6Wfvau8KEr2WHHqwLr88LvhJrwMgQG5HMgmtdbsrpiQQydY8TMmX6Ez3w9wbyd)

<br/>

---

<br/>

## AWS와 통합 

> 웹 사이트에서 JWT(Json Web Token)를 승인하고 유효성을 검증한 후 auth0.com으로 사용자에 대한 추가 정보를 요청하는 람다 함수를 작성



## JWS(JSON Web Token) 토큰 검증 후 사용자 프로필 조회 (p126)

![img](5-authentication-authorization.assets/8taqw_UI8NcgoZFNBKhLRpxj7qUNn7qZa-vChJ9wV5vcHZCgQU0O6sNLg6rARWdq1gv-w2r9zUp88RtxEY_5ValMh0X-EFKghp9OAPEWT4iRzHNRO0Gxdt6MAkQwHPgVB5vEDuZv)



### 1. auth0.com에 등록된 사용자 프로필 정보를 반환하는 람다 함수를 생성

- api-gatewat-lambda-exec-role 역할을 생성

  ![img](5-authentication-authorization.assets/UhlKWPJltz7TW3iBtrSn2pUpTjQDNLwu5amZkOz4BKN4oIVhi0Hxx7O7xQfQFx6HI8ZNlTSF4Fxb1HeypIXLjZyiM2LbgzvCZY1cf2LB4P2Z9Eu8RUFLdn_Ss9BNWpWDQktM9roM)

  ![img](5-authentication-authorization.assets/HEjalcYh42ss6YOHuRWbjjis_bRtw-2Xzi72qaU3ninXoGix0c3emlLr1QcvZwe2BkI_VlkAXDZYs6DvU4rk0iakqsogwgTxxzb2sv8H_XxXU_v5dBK-A3VFvzL8FHTKQ4LnA7xd)

  ![img](5-authentication-authorization.assets/M8MrD4y1-N5vtx_fdxEvhGpReNtZN7mkcAFRLNdhTolqmB56HUA2qrQbQ_wjbWuunY-iA8Z-NhUw9iuyyHuoaRr1-n2YDFJ8YfuMo32vPAnhKJXVVnOLWq1cxfTaboZVoye3X_wC)

  

  

- 람다 함수 생성

  - JSON Web Token 검증
  - auth0 엔드포인트를 호출해서 사용자 정보를 조회
  - 웹 사이트에 응답을 전송

  ![img](5-authentication-authorization.assets/QJNsbwQ2bmhbTi-oirr0QLZv3h-XoioECZsaQs6QZFI3_ZArjIDLSxQnh3kmoQNiJh43id3ZGGWfnueO-guPzznsQIcLGpmD5DgNRkUG7o0P-3rNafdGhXL1sCCUa9RKS2qIgenY)

  ![img](5-authentication-authorization.assets/QJNsbwQ2bmhbTi-oirr0QLZv3h-XoioECZsaQs6QZFI3_ZArjIDLSxQnh3kmoQNiJh43id3ZGGWfnueO-guPzznsQIcLGpmD5DgNRkUG7o0P-3rNafdGhXL1sCCUa9RKS2qIgenY)

  

  

- 람다 소스 코드 작성

  - `cd C:\serverless`
  - `mkdir user-profile`
  - `cd user-profile`
  - `npm init -y`
  - `npm install jsonwebtoken`
  - `npm install request`

  

- package.json 파일에 deploy, predeploy 스크립트를 추가

  - `package.json`

  ```json
  "scripts": {
      "predeploy": "del Lambda-Deployment.zip & zip -r Lambda-Deployment.zip * -x *.zip *.log node_modules/aws-sdk/*", 
      "deploy": "aws lambda update-function-code --function-name [user-profile-lambda function ARN] --zip-file fileb://Lambda-Deployment.zip"
    },
  ```



- index.js 파일 생성 및 소스코드 추가 (p130)

  - `index.js`

  ```javascript
  'use strict';
  
  var jwt = require('jsonwebtoken');
  var request = require('request');
  
  /*
  event = {
      authToken: "...",
      accessToken: "bearer [string.string.string]";
  }
  */
  
  exports.handler = function(event, context, callback){
      console.log(JSON.stringify(event));
  
      // event 객체에 authToken, accessToken 존재 여부를 확인
      // 만약, 존재하지 않으면 리턴
      if (!event.authToken) {
          callback('Could not find authToken');
          return;
      }
      if (!event.accessToken) {
          callback('Could not find access_toekn');
          return;
      }
  
      // authToken은 값을 공백문자를 기준으로 분리한 후에 두번째 값을 id_token 변수에 할당
      // authToken은 Bearer 값.값.값 형식을 가짐
      var token = event.authToken.split(' ')[1];
  
      var secretBuffer = 
      new Buffer(process.env.AUTH0_SECRET);
      jwt.verify(token, secretBuffer, function(err, decoded){
          if(err){
              console.log('Failed jwt verifacation: ', err, 'auth: ', event.authToken);
          }
          else {
              var body = {
                  'id_token': token
              };
  
              /*  환경변수 DOMAIN의 값을 auth0.com에 설정되어 있는 애플리케이션 도메인
                  auth0.com에 사용자 프로필 정보를 조회에 필요한 값을 설정 
                      => 로그인 시 절달받은 access token을 요청 파라미터로 전달
                  https://auth0.com/docs/api/authentication#user-profile */
              var options = {
                  url: 'https://'+ process.env.DIMAIO + '/tokeninfo',
                  method: 'POST',
                  json: true,
                  body: body 
              };
              
              // auth0.com으로 사용자 프로필 정보를 조회
              request(options, function(err, response, body){
                  if (!error && response.statusCode == 200) {
                      // 정상적으로 조회한 경우 호출한 곳으로 프로필 정보를 반환
                      callback(null, body);
                  }
                  else {
                      callback(error);
                  }
              });
          }
      })
  };
  ```

  

- 람다 함수 배포
  
  - `npm run deploy`



- 환경변수 등록

  ![img](5-authentication-authorization.assets/_U6AfNIjFWs-iePkEMyCceQBoeUJhZrgAVi_NAWiOa6C-o2z_Y6iRRFWmm2eljJ61N1f8Aek50VYMJuOsCPW4zcGH89nTtAhfAh-u33HxEC2cBD5dASExDtincCn2YXceAjVoNHh)

  - 값 추가

    | 키           | 값            |
    | ------------ | ------------- |
    | DOMAIN       | AUTO0 도메인  |
    | AUTH0_SECRET | Client Secret |



- 람다 함수 테스트

  - 소셜 로그인 후 브라우저 개발자 도구를 이용해서 localStorage에 저장된 accessToken과 idToken 값을 추출

    ![img](5-authentication-authorization.assets/ZmL7lb4UUj-CjrZcdNH_8XT-N49UMOvcPeiD60IbQFUdVjFHj2oM9ZXada_K6oKEeR-jzAqpOh0CV1u79aD7jM0jCU0CWUf2TTeIw_72f6YTsxRW2lT49vg7Z1QScrzg58Af7gmi)

  - 람다 함수 테스트

    ![img](5-authentication-authorization.assets/qLKFhfL1U4kwN6F5NYKQqWYdl4EjtZxHiKV5kv4CuhzFL1PLResCCokJ4W8mBT8aMKk10yucNLSJdPGhURSCqGi5sejhI6BSgSv0lByU7irAC4s4G5LEMXON5RvQuHsa_tIdgZts)

    ![img](5-authentication-authorization.assets/KmswPsMM721eNuWDScjRaMzZOitUUlh_6trm2uqRt2N2_VyT91LNvwzsiOOGu-6LFQE_S2OAmSAbS9OD7PnEs6JnS-YKFSb9wW6jMHYuEGqiOsz5b-XjQ1XZDP8Ow80SfjInFipI)

    ![img](5-authentication-authorization.assets/zhsGwimdJRS7qyRWzR6ZEBai2wqMhtD6AHDbbxbA9CM3PkM0lp_nv5de6pyIijG6xpQ-8sgbcT0Ht0WRdRFVpWR4D4FJt5pf1prqRKslyPjtOTyIMT7vxny3DMAcuMYDBmqH4jN4)

    

  - 참고: 테스트 케이스를 만드는 스크립트

    ![img](5-authentication-authorization.assets/DoroafBTjkLm1ps2wPKP3TcVg3AY2lRXM5edZsxy3X63AH7KMDAyE0cQsyP8Z0rwY22tuq0lFp0d0xcyU0Xfwk_2fefIWzNVfXI7mQS5Cm5tIGlD9BElgj36HqyX3l1dMvhiJ_mI)

    - 테스트 수행 시 응답(response)에 사용자 프로필이 출력되는 것을 확인





### 2. API Gateway 설정

> 웹 사이트의 요청을 받아서  user-profile 람다 함수를 호출

- REST API 구출

  ![img](5-authentication-authorization.assets/whut8DnQejbHLKClsA6QScf-BM8XtZcYwJGf4z-bsSao9ot2v6LTijiQiL-EKTq9PgLlffNRG2cTpXqBglhpKmgjaqr577jsp8c_pf3xSRhrOW_a8P-J3-mGtmPgDEjss4oPAhdq)

  ![img](5-authentication-authorization.assets/HTEh5xRAfsFd5I83_9F22zZHlhMthBqqYrctkYPtSIhgwsgc2XLaR-vH6hAo__NpbH_jGNsmd0DYu_fWUOQCcPotyw92gvA48rUWD1fiFcKBlAzVCPtkGQi-m1f9byc5LGupUo88)



- 리소스 생성

  ![img](5-authentication-authorization.assets/T01VyRdelo3EazVs7EkK2o3CwvcbjB_rTEuIrN-XnljUdC29jEwOFAGtYf-dRErewh2bzqnAhpq57ORY9GEuMzhWSJ8Sokk9vt5NcOKlP0jUkIPfI0wEIrLM-Dka-888vrC_gCRl)

  ![img](5-authentication-authorization.assets/Umi8gEWU7pfqk1AxcPze8u--9QCE7uC-VZzl83cQzs877C9owe6dRNZFQ0k-DBCEJ3u-GUpKzYFNGdK1g3kDrsMOprN0nNwFeXlXcT3Kry5KcxwdvOc6z_wETvi-vUsW9JY3baH0)



- 메서드 생성

  > 해당 리소스에 접근하는 방식 (GET, POST 등)

  ![img](5-authentication-authorization.assets/bqvqoHJD48sqxBCZCe0B7mQEAx8MmAZkt_VujvtXaBeW5p26GoOh5ndJ7Ib00cC5r23zTm6HUAfM3yk0_e5hqvtbF3lUKnm4CDM08XuKz2GQKcYfqLQe0vyQ6eCdWcQh7idL-phn)

  ![img](5-authentication-authorization.assets/b2IkNtzAwDUiyhgLZcLEi5mpJpNojaajExC1oD8dedP2mwgw4Z8mYIrG-TzfIOWCTBGAsGMF0JCrKZjE9GFx_k-n8GGDY2lkhEmN_mjtGmcyDuQOIO-1Mft2ZesoZJaCDolQV6Zg)

  ![img](5-authentication-authorization.assets/uWS-APy2rZNS0kXcyVZvXEHxrulzHGO6U_YOcKP4jinOpEzLNw_vDleDUqLcf1oWk29UhEFDe_ADn7Em5si_mKQZ21FK1bSw-7WROddD_k5AwTtUetT-90xz3QQXHk-uAfFTqO_V)

  ![img](5-authentication-authorization.assets/sP2ULf97xZPdGbVcxjz74xn9tQomXaRd8d5g_-tUvl5HQ10wushmk4aasysW69MOdAvR-g3sBE6TplANniJq2BjAVQt5ILT1MqvEmE1MOzM2vVZmCXcdm2l9HFLovGm3cVbCVRxa)

  ![img](5-authentication-authorization.assets/_ECaGzABMDuFAlL7rCg6zcnU0ZmvMg-l5cm9jBoRU-2amESmEfxDstsg-2EH31SX-2zKDvptBE4Rf5VBRGj6i58rmt5KIquSEXvTLkdyzyeGijwxQHSjxvdjBTKOs4grzx6ELmgC)

  - OPTIONS 항목 확인

    ![img](5-authentication-authorization.assets/X6D-4NCGFxdML7u6ONwonMatHmDx2THLu9gpTfHdKpMJfJwgcgRD_nTc4vLZdMTPKqjcI8Yn97PAXOtEky9brq68BFnaJL43hoaz_3LoBnoIKwqWJfSRLEPDtyHqjPra2DtTM13S)



- 매핑 생성

  > 맵 사이트의 Autorization 헤더를 통해 전달된 JWT 토큰을 람다 함수에서 사용할 수 있도로 event 객체의 값으로 설정
  ```
  웹 브라우저  -----------------------> API Gateway ----------------------> user-profile 람다 함수
  (웹 사이트)
              Authorization: JWT_TOKEN              event = { authToken: JWT_TOKEN, … }
              ~~~~~~~~~~~~~~~~~~~~~~~~              ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
                        |                                             |
                        +---------------- 맵핑(mapping) --------------+
  ```

  ![img](5-authentication-authorization.assets/m3AzYiZMHuSjpYTjSR95rFBxXbhICckXEytzXso5wen32jCgwcXNlUhtCIT7TCnN-GHsdwsPhVokYolisArwE-gJlp9tb3Kb_DCTsz-1CTFahRyBJB79BIJbgOoSP8w9N9JUWz5g)

  ![img](5-authentication-authorization.assets/5J_fOdky7IbZfQn374-WHcB5VcZRqzC4PotymGpQxtmdC5EIzzbllp85PKde_wsQpqU6WOQxFM-TdSFThTMcavaBqBLOY4am4Da-kUP9yeI6yIIRpZ3BxDa-ZmeU7aZmwVE6fjsf)



- API 배포

  > 해당하는 API를 외부해서 호출 가능

  ![img](5-authentication-authorization.assets/gkDu7rrXQcCvBczeqaVS6TFNpNrM1s4KWYQ4pDBAAZDnKZy5UsN_7V6lVovCyECBLZKg_wGCvBP6p7ZUgNQS00QIW-jsESpoKVqwBMDwE_lmgDfPdqLMLp2QbjOGW3n3LyARXKmI)

  ![img](5-authentication-authorization.assets/AIEdlmsLEt2Kk-Unsk04a3pw5goMaKQrGGHKq0cWy0WYXS3e8377VLRBqHTbQmKfPSdq02t_Ng6nzEuPe6YZSk2byhJVHYsI4gxE9iaFJUe8q_TqTnfKqi9j5N6_awYJ8DeqfGes)

  ![img](5-authentication-authorization.assets/pM7v_fw6yr9TpDQ-UTl8iCP3umBrp6uql9KwXeuQpyn_SU5ivrQDYv7KOidR286DnaVUjV-iHWifqAHECczm0rikglh-7vDrD6tcUe0eiKU6qO6fUnk24Qx0PTTYkPouGMmESKpD)

  

  ![img](5-authentication-authorization.assets/pM7v_fw6yr9TpDQ-UTl8iCP3umBrp6uql9KwXeuQpyn_SU5ivrQDYv7KOidR286DnaVUjV-iHWifqAHECczm0rikglh-7vDrD6tcUe0eiKU6qO6fUnk24Qx0PTTYkPouGMmESKpD)

  - API Gateway 및 리소스 URL로 접근하면 오류를 반환 ⇒ 내부 수행에 필요한 값이 전달되지 않기 때문

  

  

### 3. API Gateway를 통해서 람다 함수를 호출(실행)하도록 웹 페이지를 수정 (p138)

> 프로필 보기 기능을 추가 구현

- config.js 파일에 API Gateway 호출 URL을 등록

  - `/24-hour-video/js/config.js`

    ```javascript
    var configConstants = {
        auth0: {
            domain: '1yangsh.us.auth0.com',
            clientId: 'Bkia8NNISRVmEw6M7lANfYmvoutaOVdD'
        },
        apiBaseUrl: 'https://tzecikd5j7.execute-api.us-east-1.amazonaws.com/dev' // 추가
    };
    ```

- user-confroller.js 파일에 Show Profile 버튼을 클릭했을 때 동작을 추가 (이벤트 핸들러를 추가)

  - `c:\serverless\24-hour-video\js\user-controller.js`

    ```javascript
    },
        // API Gateway 호출 URL
        apiBaseUrl: 'https://vfhp67njsk.execute-api.us-east-1.amazonaws.com/dev'
    };
    // 특정 이벤트에 반응하는 함수를 정의
        wireEvents: function() {
            var that = this;
     
            // P138
            // Show profile 버튼 클릭 이벤트 핸들러
            this.uiElements.profileButton.click(function(e) {
                // user-profile 리소스 호출 URL
                var url = that.data.config.apiBaseUrl + '/user-profile';
     
                // 지정된 URL 주소로 GET 방식의 요청을 전달
                $.get(url, function(data, status) {
                    console.log('data', data);
                    console.log('status', status);
                }).fail(function(e) {
                    console.log(e);
                });
            });
     
            //          :
    ```

    

### 4. 사용자 프로필 조회 테스트

- 프로필 클릭

  -  -> 콘솔창에 오류 메시지를 확인

  ![img](5-authentication-authorization.assets/XcSAwnSdmvOwDRwUdDYng2gKHAsyQVdIkP0jIb942xpape34FIRX09SWmoOnDzpP_wU_7Au5bqTFQFTBm3AC-CLBSXQS8qbgt6Rn80HZdxYexscD5NM1fPfoftjZekx5IOcEwePO)

  ![img](5-authentication-authorization.assets/nXERWrlEdeGA8NNRySv5VeJXsaSAhUIUv48QhovVnmfNeK4_UzbO8T0C2-1khoJUgwGjUqssTQuQQHJ-59p61govz1-MeGhC5Co2ubD6ZPANnG4RnBIZczRiuxmmjUiWV3AHNrsk)

  ![img](5-authentication-authorization.assets/eohpJsbAuLEmPc8bM21SwlWGHxQjzE9eHlGU4ZVKD-EQDKts-Sw-P5K0UMs5qb_I98dOTLVEUMk1WEP6GYAZWRoCImqQGzioSLWUaHqxGMGNM0naSPfOGXmtrFb_65LpDemhQw18)



- 본 요청(main request)에 AccessToken이라는 요청 헤더가 포함되어 있음
  - 기원이 다른 서버(API 게이트웨이)에서 해당 헤더를 처리할 수 있는지 여부를 먼저 확인(preflighted request)
- 프리플라이트 요청 결과에 access-control-allow-headers에 AccessToken이 포함되어 있지 않음 ⇒ Request header field accesstoken is not allowed by Access-Control-Allow-Headers in preflight response. 오류가 발생
- 해결 방안 
  - ⇒ 기원이 다른 서버(API Gateway)에서 Access-Control-Allow-Headers 응답 헤더에  AccessToken을 추가



#### Access-Control-Allow-Headers 응답 헤더에 AccessToken을 추가

![img](5-authentication-authorization.assets/wO-oNBHwgp57Uw_MWeRpkVfx05Bgd5A7PYc7M7JeIzy5RF0hLDybGonYr1F05ine1V3h7jNyTadzHiKG3rC-pp8KC6mF-UHkhnU-87VX7bUBSfNeWGbA25PxpkTjGHF2-cKeexqv)

![img](5-authentication-authorization.assets/rfBI6sjT98T85rIIvO5FbBjvJlepjScaUJ6T0LyUN-9AcqIEG6gJJwj_pcDAt6vMxkACiNA58RHN6JjGefO15gA3QKHJG274AwkalW1_fubNjpuUzj9Wh8CoAyR2Ck2t2_WZpu9c)

![img](5-authentication-authorization.assets/v7EaHmVl4499JsWUu9MvIfbDhzM5Q0O5aYo4qs4mBUYdsvR6gRoWm7rjpdBGmxW4WAcJKsPggYktVthcwbVAN4FjApgVaKwAJPyrB9vd8emSuxHhFPNfSvRKFlg2jz4sgqVtYLOZ)



#### 프로필 요청 테스트

![img](5-authentication-authorization.assets/lrou_qm8Tf9oQA8QR4MkNHRQr9ztwlLTYvFsK05jkZN1OtKyRJqd5d1cXG1jVIrnAkLKJZ6G9hfBG1DxH5AOGXo7jGaOJaUX6kc6oAC4LEK7Otb5fBetkDCTba6atK3jXB4xPSuh)

![img](5-authentication-authorization.assets/B7O4UGW0saglEbtXatLAUpOWte04x5FdO4UIaI9TjGy9yw2OXRLgiYi6JgJ21XqBzFP37CLVd57cPL1uAAfGbuf6hUUVflW83dmUxK3dTziEcYHjvQqI881HEVe1iXTxbOWDDCmG)





​	

---









### CORS(교차 기원 자원 공유)

> https://developer.mozilla.org/ko/docs/Web/HTTP/CORS

- 웹(Web) = 공유 ⇒ 교차 기원 요청이 가능

- 다른 기원에서 오는 이미지를 쓸 수 있게 가능

  - 기원 = 스킴(프로토콜) + 호스트(도메인, IP) + 포트

  ![img](5-authentication-authorization.assets/rEhMpsByvv8lI9fiVcYLNRjf7VZMeSWr-MtHWLswcgXbhOkSRU1Ur2eNtEap7KgxejIf0x9VHGx-20lGtjK_Uba0TF4SBLoEOfqsCaJU7cSKIdV4Xi55uTHzIzSORbKDGRTJeUog)

  ![img](5-authentication-authorization.assets/85ajlMCbc-WdVJcEfOEP358iIXIxBzAqr9ejnA7hG83QHETfu6cl6KiTpwqI-wx8A-oTjwi6jnIeG0VDSH_T29n_cJl9IvQS5Win7tOaBpYQHhwMb_XUcSbushEvNfdJl6Q8-nZ6)







### HTTP 요청은 기본적으로 교차 기원 요청(Cross Origin Request)이 가능

> 기원이 다른 js 파일, 이미지 파일을 가져와서 서비스(보여지고 실행) 가능

- http://127.0.0.1:8000/cors_test.html

  ```html
  <html>
  <head></head>
  <body>
      <img src="http://127.0.0.1:8000/tile.png" height="200">
      <img src="https://s.pstatic.net/static/www/img/uit/2020/sp_shop_bffdc9.png" height="200">
   
      <script src="http://127.0.0.1:8000/js/main.js"></script>
      <script src="https://nid.naver.com/login/js/bvsd.1.3.4.min.js"></script>
   
  </body>
  </html>
  ```

  ![img](5-authentication-authorization.assets/0tcjU7Nw4FbzFsc2PzgKP9IdZxubGY4ZupsuRTAV-Vunywe6gwVBC0w2bJVdSLHdZNB5lNW6WMmSVBpEVTrFvtXv4rn6pyJja2ntegSVUhSc7EXWNqx78XtxEkVGaxNKznQ-7Stn)

```
http://127.0.0.1:8000/cors_test.html
http://127.0.0.1:8000/tile.png
http://127.0.0.1:8000/js/main.js
https://s.pstatic.net/static/www/img/uit/2020/sp_shop_bffdc9.png	 ⇐ 기원이 다름에도 불구하고 가져와서 사용이 가능
https://nid.naver.com/login/js/bvsd.1.3.4.min.js
```



#### 스크립트 내에서 교차 기원 요청은 보안 상의 이유로 제한 ⇒ SOP(Same Origin Policy) : 동일 기원 정책

> ajax 통신 = XMLHttpRequest 객체를 이용한 비동기 통신

- SOP ⇒ XMLHttpRequest 객체를 사용해서 가져오는 리소스는 해당 웹 애플리케이션과 동일한 기원으로 제한

#### CORS(Cross Origin Resource Sharing: 교차 기원 자원 공유)

⇒ SOP를 완화하는 정책

⇒ Access-Control-Allow-Origin 응답 헤더를 이용해서 자원 사용 여부를 허가



#### 단순 요청(simple request)

![img](5-authentication-authorization.assets/TzQPkJ5xkuvEIuDSB1dovKTArIzDz-m8vgjF0u92KQZ5CIF59Sc7Pafxxr7mqe_s2fxoOJukIHEejES_NPqb9OBywxPN3AkJ8clIvibl8YfALfRR9L20R-Ex2qhWN6UVIOBOGPlw)



#### 프리플라이트 요청(preflighted request)

![img](5-authentication-authorization.assets/7QOw0zdMLxl4osLJ9KWMP4ls-Hl0VLly_oJXdW0m-3gVuucWQMtXhvwK-ScE7TrDJLTiO0UO8rKnccTzKGAUxijaXsVjUSFxgzNrwgCBO3bjI2QhTBKaL7jvm-xVUM8AG1Kb5q9Y)



---

<br/>



## API Gateway의 권한 부여자를 이용한 권한 부여(p139)

>  API Gateway의 권한 부여자를 이용해서 JWT 토큰을 검증한 후 유효한 토큰인 경우에만 사용자 프로필을 조회하는 람다 함수를 실행할 수 있는 권한을 부여

- 참고 ⇒ https://auth0.com/docs/tokens/json-web-tokens/validate-json-web-tokens



### 1. JWT 토큰을 검증하는 custom-authorizer 람다 함수를 생성

- 람다 함수 생성

  ![img](5-authentication-authorization.assets/s3I1yvzBeItepOGioZRE1WQY_HbjroaU325Fv2XngbmmSdbnN2PwDQwmDvFApbBtd0tdEUK-YbGTS4bmaMKHmvWA8OcTOfdpEwfWR6v8R-MCH-eyVosFY0C-OM-1Qjmzbvm6nIkW)

  

- 작업 디렉터리 생성 및 필요 모듈 설치

  - `cd C:\serverless`
  - `mkdir custom-authorizer`
  - `cd custom-authorizer`
  - `npm init -y`
  - `npm install jsonwebtoken`



- package.json 파일에 predeploy, deploy 스크립트 추가

  - `package.json`

    ```json
    "scripts": {
        "predeploy": "del Lambda-Deployment.zip & zip -r Lambda-Deployment.zip * -x *.zip *.log node_modules/aws-sdk/*", 
        "deploy": "aws lambda update-function-code --function-name custom-authorizer-람다함수ARN --zip-file fileb://L
    ```



- 람다 함수 작성

  - `index.js`

    ```javascript
    // P141
    'use strict';
     
    var jwt = require('jsonwebtoken');
     
    // generatePolicy 함수를 정의
    /* 정책 문서 형식을 생성
        {
            "principalId": "...", 
            "policyDocument": {
                "Version": "2012-10-17",
                "Statement": [
                    {
                        "Effect": "Allow",
                        "Action": "*",
                        "Resource": "*"
                    }
                ]
            }
        }
    */
    var generatePolicy = function (principalId, effect, resource) {
        var authResponse = {};  // authResponse 객체 선언
        authResponse.principalId = principalId;
        if (effect && resource) {
            var policyDocument = {};
            policyDocument.Version = '2012-10-17'; // 정책 문서 형식 버전
            policyDocument.Statement = []; 
     
            var statementOne = {};
            statementOne.Action = 'execute-api:Invoke'; 
            statementOne.Effect = effect;
            statementOne.Resource = resource;
     
            policyDocument.Statement[0] = statementOne;
            authResponse.policyDocument = policyDocument;
        }
        return authResponse;
    };
     
    // 핸들러 함수를 정의
    exports.handler = function(event, context, callback) {
        if (!event.authorizationToken) {
            callback('Could not find authorizationToken');
            return;
        }
     
        // JWT 토큰의 앞부분(Bearer)을 제거
        // "authorizationToken": "Bearer eyJhbGciOiJ~~~cCI6IkpXVCJ9.eyJnaXZlbl~~~pKNDQuZSJ9.mioxKcb1~~~W1LTk5_anGo"
        var token = event.authorizationToken.split(' ')[1];
     
        // auth0.com에서 제공한 Client Secret을 환경변수로부터 읽어와서 변수에 할당
        var secretBuffer = new Buffer(process.env.AUTH0_SECRET);
        // JWT 토큰을 검증
        jwt.verify(token, secretBuffer, function(err, decoded) {
            if (err) {
                console.log('Failed jwt verification: ', err, 'auth:', event.authorizationToken);
                callback('Authorization Failed');
            } else {
                var policy = generatePolicy('user', 'allow', event.methodArn);
                console.log(policy);
                callback(null, policy);
            }
        });
    };
    ```



- 람다 함수 배포
  - `npm run deploy`



- 람다 함수 실행에 필요한 환경 변수 설정

  - 구성 -> 환경변수 편집

    | 키           | 값                              |
    | ------------ | ------------------------------- |
    | AUTH0_SECRET | AUTH0_SECRET값( AUTH0에서 확인) |

    

- 람다 함수 테스트

  ![img](5-authentication-authorization.assets/aRf7dhRx6irPiFThl0ZcP29gFs45GB6b7W0N-t0ip7pPZzhRX-S6gabBrd_0Wa7ho4hhsuLIj1BffHn5E79ThGZ2PcdjviAIJQlgdBsvzORK6rD1L_wO-DFEHlNOoYm6eExCocet)

  - 로컬 스토리지에 저장된 idToken 값에 "Bearer "를 추가한 값을 "authorizationToken" 값으로 설정

    ```
    {
       "authorizationToken": "Bearer xxxxxxxxxxxxxxxxxxxxx.yyyyyyyyyyyyyyyyyyyy.zzzzzzzzzzzzzzzzzzz"
    }
    ```

    ![img](5-authentication-authorization.assets/dK5yEpD4nP82gK5inUbFmYJIFaVintU2kabpFX71K5wuBOMsPzXY_5vdx1esL54vYWR3nmcr5qJMixik0vgyWAPV4ARLmkvEuw31uKWC19SQiWoeZRg2dGhgXfX2AiXnMZptQMmi)

  - 테스트 결과 

    ![img](5-authentication-authorization.assets/DEUmQD19LO0_J-yAjjSdn_Ea7CV3h7ShLjOIarZNSwX0BsD4kNkEXR58hdy7uoudw-fzm5F_s2UxTNHE0Ed6mpfzehXuYhpK8ydlUj-_DDxgjaACyXqvc9ldJfUVYbz5_hBeEYxl)



-  https://jwt.io 사이트를 통해서 JWT 토큰을 수작업으로 검증

- auth0.com 서명에서 사용하는 알고리즘을 변경

  ![img](5-authentication-authorization.assets/zZzP4Ud8eCFOHkxYNwaeLNyP7486aKHLowFuszTHyraK9q7uYLnWiKXETUlKq5JXLUaYkgh4XMmIdUhSLPf_75xfv6LEiopuR6l4uAF5cxLi8hZV4keFJLhTcYmqn6SOUdBjW1_z)

  ​															:

​	![img](5-authentication-authorization.assets/t2peEeRkF9LUD-pNCza_wez51O3PM87F_DsL02rJmrq5UJZH0CNikZjRd1pnIInvYMqPlS-aGwtdcHYISWzanYbQnoSscqhu_RUC09nm0NI84WwL81P9YNmmA6EGrYNkORdJDQmI)

![img](5-authentication-authorization.assets/bUs3ttWvv4EQvs6rIzZkr_4r6wB_a38AO5oq-Ah4ADyw6xs7uviA7K41H0-5-WJbUu3mh-Uo1Nj8P81OJ3NuHHKfj9M8kx6EeJAAX02NkaE0gLFK4f_yIvpTSdxN_MOkWG64Wd10)



### 참고 : Specify the algorithm used to sign the JsonWebToken: 

- HS256: JWT will be signed with your client secret. 
  - ⇐ 비밀키 암호화 방식 
- RS256: JWT will be signed with your private signing key and they can be verified using your public signing key. 
  - ⇐ 공개키 암호화 방식 





- 다시 로그인해서 발급받은 idToken을 추출해서 테스트

  ![img](5-authentication-authorization.assets/Iirv8egT-ndQRIwBcDsZb3havoYAfSi672dvEA3BlzT0c_BBEuzeigAsPdoapKyJWji8Ud_7m4a_BfzlKW9rLyBMIa_k4Ia3x-5-98wGnfGsuOG8QAhbjemwODzN9P21xvjSuPgn)





- API Gateway에 권한 부여자를 생성

  ![img](5-authentication-authorization.assets/Wx8DtfTK2xRWbDzjisr02XGcxOOkIvJ2t6eWgZUjVpHWRsbx9MURmkUyw9kSzJWJ1Cpy_dvTzsv7uz5U26oPoSPsONi7mpzzCtoqkAWYQyScXJS0TNt_JZrEaFIWRhL3uze1MDV-)

  ![img](5-authentication-authorization.assets/ZY9Pat4usGjwBlpagDUgjOPOF8HGmsxnJ5uwqEUXuP_uh45-MAFBs6cJRHb6nhF5H6hy9CCMhdl14V93yB_pyiIgLv0KcjM3X8gQ5Q04_JrAaoICACu08FbicLL8p8U1cAZuZZtY)

  ![img](5-authentication-authorization.assets/SVIPar3MWQDCw2H7Fo_ONCXZ4Uh6_CUeUhUxMhzzec4eccbNKM8GXSFIQTCWRS1vCj8_prXaUXkKTM7bxwzGmgYxCGkizZRmyJ78e547LbB_Epf8p9ZMBaWgYgm0AURohQzaMZCp)

  

  

- 사용자 정의 권한 부여자를 /user-profile - GET 메서드에 연결

  - 주의 : API Gateway 대시보드로 이동했다가 리소스 메뉴로 이동

  ![img](5-authentication-authorization.assets/ySht5elBSb1-YK4RhWLlM5maD7mkfRXqS7JST-2Jq8dcRoDZ0JFQyzWj9v56Yp5PQfQmPUw95hPOkM0KLchR3Ul7Z6folzgmT8ypV6XxGf8GKOksS_k_L4HURdf6L96csONdZsZh)

  ![img](5-authentication-authorization.assets/TFX_AwW2dN2GEwNJy55YiGAxqVWh_UaUXeI49ew9ru2uqICJpsuTuBjVJ-5A7gwieOg5impoMncmv1R8j3YN3yJ3JGZTzxSPr3zsMQ7FSffooqLEFXO1VfWzqQdr0U32RknoKMtg)





- 로그인 후 프로필 버튼을 클릭했을 때 사용자 정보가 콘솔에 출력되는지 확인

  - 요청 응답 해더

  ![img](5-authentication-authorization.assets/fdNPUoG-ARxzhTfsIIhS4IZIYy3iLiHeCWdbUln6IxLsrMn-2sZe71_E5B3KoMfMsFV9FztU9kodSj7wKal3YyejkAvh6yKGhbpfwZImrixoL725NtkHoM7r_F4Uh95kcI0cbDSj)

  - 브라우저 콘솔

  ![image-20210317112301904](5-authentication-authorization.assets/image-20210317112301904.png)

  - custom-authorizer 로그

  ![img](5-authentication-authorization.assets/dVz4O_zB7emhAAD_uykAKMzzyX1tr-jdPOHoRatOVewn68Xh7d-3tlOr_TUrOZKCYhm1TjckepDI2Sbg4uvmoFvPcvOPd2D0vapy8hE_PmFm0q6IflxBLsw7hVVd70OGvEkij0rM)

  - user-profile 로그

  ![img](5-authentication-authorization.assets/Cte6D-tM6zH8K_6r3yZkUBm1zJ24x57oFUreXSib_WxXD0Ba86IlDg-cN3BMee-49Tm08yUqwhTXW06NJKUClMuRiNU-15TfD3-OHcjI4moramgszDZxTd2LXs2UqLdfFIEy1Vme)

  

- 개발자 도구를 이용해서 idToken 값을 수정(조작)했을 때 오류가 출력되는 것을 확인

  > 요청/응답 헤더 ⇒ 4xx번 대 오류와 5xx번 대 오류가 발생하는 경우 CORS 관련 헤더를 설정하지 않음
  - 요청 응답 헤더

    ![img](5-authentication-authorization.assets/RezaMMQ7ahfA3KYcBOjNdIawJ_8G_psUxJJOfkhRN22dAzxGA_5fGCP3OGUeWPbN8_IjmTGQO_KfUDPopQK6U8Ftx8LXCQyIJdK4De3wga8aczot3xgkwkC99uXvdl1aY5xb14pU)

  - 브라우저 콘솔 로그

    ![img](5-authentication-authorization.assets/cCrH6sFvwS-XDlyqxiNf0O-imDpy5nQvLUNc1sM3VDCZVW4Xwg8rnrqwtajVe86LilknTMjkYOYr1H8ZhRwSRx9X-IMhmovgiXWp0PhI2iIu0AmQVsGjwSaUmE9A7YHvgVz67yK0)

  - CloudWatch에 user-profile 로그 그룹이 생성되지 않음 ⇒ user-profile 람다 함수가 호출(실행)되지 않음

    ![img](5-authentication-authorization.assets/CthJKV89OaQae6gjUPkZ34HFwPw5WwX5ZmBNlGD54Ezf9DrKTRGlVroeyti7RVtOsgZcxrH67D6pRY6jkCj-OWXRnd5vBY7BhdZHI8VCAsUOOZwVRTAVAJcZaHOF7bznX2aVVVKs)

    ![img](5-authentication-authorization.assets/RezaMMQ7ahfA3KYcBOjNdIawJ_8G_psUxJJOfkhRN22dAzxGA_5fGCP3OGUeWPbN8_IjmTGQO_KfUDPopQK6U8Ftx8LXCQyIJdK4De3wga8aczot3xgkwkC99uXvdl1aY5xb14pU)

  - 브라우저 콘솔 로그

    ![img](5-authentication-authorization.assets/cCrH6sFvwS-XDlyqxiNf0O-imDpy5nQvLUNc1sM3VDCZVW4Xwg8rnrqwtajVe86LilknTMjkYOYr1H8ZhRwSRx9X-IMhmovgiXWp0PhI2iIu0AmQVsGjwSaUmE9A7YHvgVz67yK0)

  - CloudWatch에 user-profile 로그 그룹이 생성되지 않음 ⇒ user-profile 람다 함수가 호출(실행)되지 않음

    ![img](5-authentication-authorization.assets/CthJKV89OaQae6gjUPkZ34HFwPw5WwX5ZmBNlGD54Ezf9DrKTRGlVroeyti7RVtOsgZcxrH67D6pRY6jkCj-OWXRnd5vBY7BhdZHI8VCAsUOOZwVRTAVAJcZaHOF7bznX2aVVVKs)

  - customer-authorizer 람다 함수의 로그

    ![img](5-authentication-authorization.assets/QlEJwoHeEkeIoPEdMwgSwUeYWZWTDaS1sgPCZ6zKZaQ1jm4ZOBIu7_z4dy-4IwNF52uO_jpjWFVwRyL7wJ-D7aQIOMsFjYSx7_HqI_4dZGt7MINkHgsQIr4vNKuh_IjQq-IKClj5)



- **4xx번 대 오류와 5xx번 대 오류가 발생  CORS 관련 헤더를 설정하도록 CORS 설정**

  ![img](5-authentication-authorization.assets/toVd1Y-XHL3EuUpx-u4dkXQQzWgLfo3P5VgUhc2FoRjXBt9hKXAP2VZwHyEAqYL0CR3P0scRjM5HfKDc70C5KdtlwGI0PqS1ykgy3gaM928TmdKNKc90e-mQjNzixSkxuNO70aYu)

  ![img](5-authentication-authorization.assets/Cl6BnjmA9wYp4pgQFLaQyzSreABL14YwX2Hvaykeu0atW70tX7oxkix1Nd9ZOttdJOFEEQxeNY3m1W33o_5DXv6TPG1t1l_A69fthKRbiKVA8IHdbnjveIy_MDzQDjQ1Eov8Jm-r)

  ![img](5-authentication-authorization.assets/KWcwCXyfReFFIm4XKneFXbo4eIBtdju4VLbqP_z6BY9kuoCdx09t7UQAuHK4kuj24dq4FPZqC2l-_FrUCLisrvLMJfTPrFutZy_Wq9ph_HWbT3GD2RGk0TaTBRFZdPgTZzou7Q0D)

  ![img](5-authentication-authorization.assets/HSI-kkpyP6nPJusS3JMjo3BZR7_qkQGnlNt5Tav0M9obSjpGPfLDJ4ji-Txr6QSqmVyGbnF_jK5Amzd1ejsGbc6OjVevlxmK4HDGjtnb9_AbqEJ1lRqvSP1PxT7usQktjDq2tmUn)

  ![img](5-authentication-authorization.assets/9_jPsyOPfkC9nW6MNqoeYKza2UWGl2xfGEOIz_XiFnlJhsTxJNckBkjo9-qrC3NZniZalIbjTQ854jjswA6PS0DPWh8H7IVVLAFQmbEOfSkMNcZDhfvHPrJKWPB-60eFZaEzihuK)

  ![img](5-authentication-authorization.assets/hhTeAY1c1DnvQwGv0RPtgEPbRCA1zr2Cq1acHSxwlzH3zaPb0_jG8amInTWxq8nec4DKCqTvezI8lQW_KHr5yANEuqA1MUlPyc3gC9nhqm9LEe8Nv7ds7Xx_t25n1OsV5NeaGOqY)







