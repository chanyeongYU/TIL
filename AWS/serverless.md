

##### 유저 생성

- user name: lambda-upload
- 프로그래밍 접근 방식
  - 액세스 키 및 비밀키 생성



##### aws configure

- 액세스 키, 비밀 키 입력
- region: us-east-1(버지니아)



##### 권한 설정

- lambda-upload -> 권한
- 정책 이름: labmda-policy
- 정책
  - GetFunction
  - UpdateFunctionCode
  - UpdateFunctionConfiguration
- 모든 리소스



##### IAM 역할 생성

- 역할 이름: lambda-s3-execution-role
- 역할
  - AWSLambdaExecute
  - AmazonElasticTranscoder_JobsSubmitter



##### 동영상 업로드용 S3 버킷(video-upload)과 변환 파일 저장용 S3 버킷(video-transcoded)을 생성

- us-east-1

- S3 버킷(video-upload)
  - 이름: serverless-video-upload-chan
- S3 버킷(video-transcoded)
  - 이름: serverless-video-transcoded-chan



##### 람다함수 생성(transcode-video)

- 이름: transcode-video
- 런타임: Node.js 12
- 역할: lambda-s3-execution-role



##### Elastic Transcoder 설정

- 파이프라인 생성
  - 이름: 24-Hour Video
  - 입력 버킷: serverless-video-upload-chan
    - 역할: Default
  - 작업 결과 및 썸네일 버킷: serverless-video-transcoded-chan
    - 스토리지 클래스: Standard



##### npm 설정(transcode-video)

- `npm init -y`

- `npm aws-sdk`

- index.js 작성

  ```javascript
  'use static';
  
  var AWS = require('aws-sdk');
  var elasticTranscoder = new AWS.ElasticTranscoder({
      region: 'us-east-1'
  });
  
  exports.handler = function(event, context, callback) {
      console.log('Welcome');
      var key = event.Records[0].s3.object.key;
      var sourceKey = decodeURIComponent(key.replace(/\+/g, ' '));
      var outputKey = sourceKey.split('.')[0];
  
      var params = {
          PipelineId: '1603787588532-wgvh0o',
          Input: { Key: sourceKey },
          
          Outputs: [
              {
                  Key: `${outputKey}/${outputKey}-1080p.mp4`,
                  PresetId: '1351620000001-000001'
              }, 
              {
                  Key: outputKey + '/' + outputKey + '-720p' + '.mp4',
                  PresetId: '1351620000001-000010'
              }, 
              {
                  Key: outputKey + '/' + outputKey + '-web-720p' + '.mp4',
                  PresetId: '1351620000001-100070'
              }
          ]
      };
      elasticTranscoder.createJob(params, function(err, res) {
          if (err) {
              callback(err);
          }
      });
  };
  ```

  

##### transcode-video 배포

- package.json 수정

  ```json
  {
    "name": "transcode-video",
    "version": "1.0.0",
    "description": "",
    "main": "index.js",
    "scripts": {
      "test": "echo \"Error: no test specified\" && exit 1",
      "deploy": "aws lambda update-function-code --function-name arn:aws:lambda:us-east-1:327187711308:function:transcode-video --zip-file fileb://Lambda-Deployment.zip",
      "predeploy": "zip -r Lambda-Deployment.zip * -x *.zip *.log"
    },
    "author": "",
    "license": "ISC",
    "dependencies": {
      "aws-sdk": "^2.779.0"
    }
  }
  ```

- `npm run deploy`



##### S3-Lambda 연결

- serverless-video-upload-chan -> 속성 -> 이벤트 -> 모든 객체 생성 알림 추가
  - lambda: transcode-video
- serverless-video-upload-chan 버킷에 비디오 업로드하면 serverless-video-transcoded-chan에 비디오 생성



##### SNS-S3 연결

- SNS 주제 생성

  - 이름: transcoded-video-notifications

  - 액세스 정책 -> 고급

    - JSON 수정

      ```json
      "Condition": {
          "ArnLike": {
              "AWS:SourceArn": "arn:aws:s3:::serverless-video-transcoded-chan"
          }
      }
      ```

- S3(serverless-video-transcoded-chan) -> 속성 -> 이벤트 -> 모든 객체 생성 알림 추가

  - SNS TOPIC:  transcoded-video-notifications



##### 람다함수 생성(set-permissions)

- 이름: transcode-video
- 런타임: Node.js 12
- 역할: lambda-s3-execution-role



##### npm 설정(set-permissions)

- `npm init -y`

- `npm aws-sdk`

- index.js 작성

  ```js
  'use strict';
   
  var AWS = require('aws-sdk');
  var s3 = new AWS.S3();
   
  exports.handler = function(event, context, callback) {
      var message = JSON.parse(event.Records[0].Sns.Message);
      var sourceBucket = message.Records[0].s3.bucket.name;
      var sourceKey = decodeURIComponent(message.Records[0].s3.object.key.replace(/\+/g, ' '));
   
      var params = {
          Bucket: sourceBucket,
          Key: sourceKey, 
          ACL: 'public-read'
      };
      s3.putObjectAcl(params, function(err, res) {
          if (err) {
              callback(err);
          }
      });
  };
  ```



##### transcode-video 배포

- package.json 수정

  ```json
  {
    "name": "set-permissions",
    "version": "1.0.0",
    "description": "",
    "main": "index.js",
    "scripts": {
      "test": "echo \"Error: no test specified\" && exit 1",
      "deploy": "aws lambda update-function-code --function-name arn:aws:lambda:us-east-1:327187711308:function:set-permissions --zip-file fileb://Lambda-Deployment.zip",
      "predeploy": "zip -r Lambda-Deployment.zip * -x *.zip *.log"
    },
    "keywords": [],
    "author": "",
    "license": "ISC",
    "dependencies": {
      "aws-sdk": "^2.779.0"
    }
  }
  ```

- `npm run deploy`



##### Lambda-SNS 연결

- SNS(transcoded-video-notifications) -> 구독 생성
  - 프로토콜: AWS Lambda
  - 엔드포인트: set-permissions



##### 객체 ACL변경 권한 생성

- Role(lambda-s3-execution-role) -> 인라인 정책 추가
  - 정책 이름: s3-acl-policy
  - 서비스: S3
  - 작업: PutObjectAcl
  - 모든 리소스



##### 버킷(serverless-video-transcoded-chan) public접근 허용

- serverless-video-upload-chan에서 파일 이름 을 바꾸면 serverless-video-transcoded-chan에 새로운 파일 생성
  - 새로운 파일은 접근 가능



##### 웹사이트 생성

- `mkdir 24-hour-video && cd 24-hour-video`

- `npm init -y`

- `npm install local-web-server --save-dev`

- http://www.initializr.com/ 에서 부트스트랩 버전 UI 다운 후 작업 디렉토리에 압축 해제

- package.js 수정

  ```js
  {
    "name": "24-hour-video",
    "version": "1.0.0",
    "description": "",
    "main": "index.js",
    "scripts": {
      "start": "ws",
      "test": "echo \"Error: no test specified\" && exit 1"
    },
    "keywords": [],
    "author": "",
    "license": "ISC",
    "devDependencies": {
      "local-web-server": "^4.2.1"
    }
  }
  ```

  

##### 사용자 웹 페이지를 만들고 auth0.com과 연동하여 SNS 로그인 기능을 구현합니다. 

- index.html 수정 (navbar)

  ```html
  <div class="navbar-form navbar-right">
      <button id="user-profile" class="btn btn-default">
          <img id="profilepicture" />&nbsp;<span id="profilename"></span>
      </button>
      <button id="auth0-login" class="btn btn-success">Sign in</button>
      <button id="auth0-logout" class="btn btn-success">Sign out</button>
  </div>
  ```

- index.html 수정 (script)

  ```html
  <script src="https://cdn.auth0.com/js/lock/11.27/lock.min.js"></script>
  <script src="js/config.js"></script>
  <script src="js/user-controller.js"></script>
  ```

- config.js

  ```js
  var configConstants = {
      auth0: {
          domain: 'uc0.us.auth0.com',
          clientId: '4z8TuVfm4KLg2X7NAIHKTFVQX4vAAfjF'
      }
  };
  ```

- main.js

  ```js
  (function() {
      $(document).ready(function() {
          userController.init(configConstants);
      });
  }());
  ```

- user-controller.js

  ```js
  var userController = {
      data: {
          auth0Lock: null,
          config: null
      }, 
      uiElements: {
          loginButton: null,
          logoutButton: null, 
          profileButton: null, 
          profileNameLabel: null,
          profileImage: null
      }, 
      init: function (config) {
          var that = this;
          this.uiElements.loginButton = $('#auth0-login');
          this.uiElements.logoutButton = $('#auth0-logout');
          this.uiElements.profileButton = $('#user-profile');
          this.uiElements.profileNameLabel = $('#profilename');
          this.uiElements.profileImage = $('#profilepicture');
   
          this.data.config = config;
          this.data.auth0Lock = new Auth0Lock(config.auth0.clientId, config.auth0.domain);
          var idToken = localStorage.getItem('userToken');
          if (idToken) {
              this.configureAuthenticatedRequests();
              this.data.auth0Lock.getProfile(idToken, function (err, profile) {
                  if (err) {
                      return alert('프로필을 가져오는데 실패했습니다. ' + err.message);
                  }
                  that.showUserAuthenticationDetails(profile);
              });
          }
          this.wireEvents();
      },
      configureAuthenticatedRequests: function() {
          $.ajaxSetup({
              'beforeSend': function (xhr) {
                  xhr.setRequestsHeader('Authorization', 'Bearer ' + localStorage.getItem('userToken'));
              }
          })
      }, 
      showUserAuthenticationDetails: function(profile) {
          var showAuthenticationElements = !!profile;
          if (showAuthenticationElements) {
              this.uiElements.profileNameLabel.text(profile.nickname);
              this.uiElements.profileImage.attr('src', profile.picture);
          }
          this.uiElements.loginButton.toggle(!showAuthenticationElements);
          this.uiElements.logoutButton.toggle(showAuthenticationElements);
          this.uiElements.profileButton.toggle(showAuthenticationElements);
      }, 
      wireEvents: function() {
          var that = this;
          this.data.auth0Lock.on('authenticated', function(authResult) {
              console.log(authResult);
              that.data.auth0Lock.getUserInfo(authResult.accessToken, function (error, profile) {
                  if (!error) {
                      localStorage.setItem('userToken', authResult.accessToken);
                      that.configureAuthenticatedRequests();
                      that.showUserAuthenticationDetails(profile);
                  }
              });
          });
          this.uiElements.loginButton.click(function(e) {
              that.data.auth0Lock.show();
          });
          this.uiElements.logoutButton.click(function(e) {
              localStorage.removeItem('userToken');
              that.uiElements.logoutButton.hide();
              that.uiElements.profileButton.hide();
              that.uiElements.loginButton.show();
          });
      }
  };
  ```

- main.css

  ```css
  #auth0-logout {
     display: none;
  }
   
  #user-profile {
     display: none;
  }
   
  #profilepicture {
     height: 20px;
     width: 20px;
  }
  ```



##### 새 역할 생성 (api-gateway-lambda-exec-role)

- 이름: api-gateway-lambda-exec-role
- 정책: AWSLambdaBasicExecutionRole



##### 람다함수 생성(user-profile)

- 기존 역할 사용: api-gateway-lambda-exec-role



##### npm 설정

- `mkdir user-profile && cd user-profile`

- `npm init -y`

- `npm install jsonwebtoken`

- `npm install request`

- package.json 수정

  ```json
  {
    "name": "user-profile",
    "version": "1.0.0",
    "description": "",
    "main": "index.js",
    "scripts": {
      "deploy": "aws lambda update-function-code --function-name arn:aws:lambda:us-east-1:327187711308:function:user-profile --zip-file fileb://Lambda-Deployment.zip",
      "predeploy": "zip -r Lambda-Deployment.zip * -x *.zip *.log",
      "test": "echo \"Error: no test specified\" && exit 1"
    },
    "keywords": [],
    "author": "",
    "license": "ISC",
    "dependencies": {
      "jsonwebtoken": "^8.5.1",
      "request": "^2.88.2"
    }
  }
  ```

- index.js 생성

  ```js
  "use strict";
  
  var jwt = require('jsonwebtoken');
  var request = require('request');
  
  exports.handler = function (event, context, callback) {
      console.log(event);
  
      if (!event.authToken) {
          callback('Could not find authToken');
          return;
      }
  
      if (!event.accessToken) {
          callback('Could not find access_token');
          return;
      }
  
      var id_token = event.authToken.split(' ')[1];
      var access_token = event.accessToken;
  
      var body = {
          'id_token': id_token,
          'access_token': access_token
      };
  
      var options = {
          url: 'https://' + process.env.DOMAIN + '/userinfo',
          method: 'GET',
          json: true,
          body: body
      };
  
      request(options, function (error, response, body) {
          console.log("Response0: " + JSON.stringify(response));
  
          if (!error && response.statusCode === 200) {
              console.log("Response1: " + JSON.stringify(response));
              callback(null, body);
          } else {
              callback(error);
          }
      });
  };
  ```

- `npm run deploy`



##### 람다 환경변수 설정(user-profile)

- 환경 변수 편집
- key: DOMAIN     value: uc0.us.auth0.com
- key: AUTH0_SECRET     value: Dn_FyyaUJ_xmSRz_fepsJFhKNXxBWRgis7SzPX6fOVl2gv4voW5L98n4UjqfOQv3



##### API Gateway 생성 및 설정

- 이름: 24-hour-video

  - 리소스 이름: user-profile
  - 메소드: GET메소드 생성
    - Intergration type: Lambda
    - region: us-east-1
    - lambda function: user-profile
  - CORS 활성화
    - Access-Control-Allow-Headers에 AccessToken 추가

- 매핑

  - 리소스(user-profile) -> GET -> Integration Request -> Add Mapping Templates

  - application/json

    ```json
    {
        "authToken": "$input.params('Authorization')",
        "accessToken": "$input.params('AccessToken')"
    }
    ```

- API 배포

  - 새 스테이지
  - 이름: dev

- user-controller.js 수정

  ```js
  var userController = {
      data: {
          auth0Lock: null,
          config: null
      }, 
      uiElements: {
          loginButton: null,
          logoutButton: null, 
          profileButton: null, 
          profileNameLabel: null,
          profileImage: null
      }, 
      init: function (config) {
          var that = this;
   
          this.uiElements.loginButton = $('#auth0-login');
          this.uiElements.logoutButton = $('#auth0-logout');
          this.uiElements.profileButton = $('#user-profile');
          this.uiElements.profileNameLabel = $('#profilename');
          this.uiElements.profileImage = $('#profilepicture');
   
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
              this.data.auth0Lock.getProfile(accessToken, function (err, profile) {
                  if (err) {
                      return alert('프로필을 가져오는데 실패했습니다. ' + err.message);
                  }
                  that.showUserAuthenticationDetails(profile);
              });
          }
          this.wireEvents();
      },
      configureAuthenticatedRequests: function() {
          $.ajaxSetup({
              'beforeSend': function (xhr) {
                  xhr.setRequestHeader('Authorization', 'Bearer ' + localStorage.getItem('idToken'));
                  xhr.setRequestHeader('AccessToken', localStorage.getItem('accessToken'));
              }
          })
      }, 
      showUserAuthenticationDetails: function(profile) {
          var showAuthenticationElements = !!profile;
          if (showAuthenticationElements) {
              this.uiElements.profileNameLabel.text(profile.nickname);
              this.uiElements.profileImage.attr('src', profile.picture);
          }
          this.uiElements.loginButton.toggle(!showAuthenticationElements);
          this.uiElements.logoutButton.toggle(showAuthenticationElements);
          this.uiElements.profileButton.toggle(showAuthenticationElements);
      }, 
      wireEvents: function() {
          var that = this;
   
          this.data.auth0Lock.on('authenticated', function(authResult) {
              console.log(authResult);
              localStorage.setItem('accessToken', authResult.accessToken);
              localStorage.setItem('idToken', authResult.idToken);
   
              that.data.auth0Lock.getUserInfo(authResult.accessToken, function (error, profile) {
                  if (!error) {
                      that.showUserAuthenticationDetails(profile);
                  }
              });
          });
          this.uiElements.loginButton.click(function(e) {
              that.data.auth0Lock.show();
          });
          this.uiElements.logoutButton.click(function(e) {
              localStorage.removeItem('accessToken');
              localStorage.removeItem('idToken');
              that.uiElements.logoutButton.hide();
              that.uiElements.profileButton.hide();
              that.uiElements.loginButton.show();
          });
          this.uiElements.profileButton.click(function(e) {
              var url = that.data.config.apiBaseUrl + '/user-profile';
              $.get(url, function(data, status) {
                  console.log('data', data);
                  console.log('status', status);
              });
          });
      }
  };
  ```

- config.js 수정

  ```js
  var configConstants = {
      auth0: {
          domain: 'uc0.us.auth0.com',
          clientId: '4z8TuVfm4KLg2X7NAIHKTFVQX4vAAfjF'
      },
      apiBaseUrl: 'https://wd49awj4w6.execute-api.us-east-1.amazonaws.com/dev'
  };
  ```

  

##### 로그인에 성공하면 SNS에 등록된 프로필 사진과 이름, 그리고 video-transcoded 버킷에 저장된 동영상 목록을 출력



##### 동영상을 재생합니다. 