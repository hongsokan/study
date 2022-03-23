# Jenkins
## 프로젝트 폴더 및 Item 설정
1. 새로운 Item에서 프로젝트 폴더 만들기
- "Enter an item name"에 프로젝트 폴더명 입력하고 "Folder" 선택 및 "OK"
- 생성하면 권한관리 페이지로 이동. 본인 계정에 모든 권한 체크
2. 프로젝트 만들기.
- 본인 폴더로 간 후 New Item 클릭.
- "Enter an item name"에 프로젝트 이름 입력하고 "Freestyle project" 선택 및 "OK"
## 프로젝트 Maven 설정
1. Jenkins - 프로젝트 - 구성에서 "Invoke top-level Maven targets" 메뉴에서 "Maven Version" 입력(ex. maven3.6)
2. "Goals"에 clean install 입력
## Gitlab 연동
1. 본인이 만든 폴더에서 좌측 "Credentials" 클릭
2. 본인의 폴더 클릭
3. "Global credentials(unrestricted)" 클릭
4. "Add Credentials" 클릭하여 Credentials 추가
5. 본인의 Gitlab 계정 정보 입력하고, 해당 Credentials의 ID를 임의로 입력 후 "OK"
6. 프로젝트의 구성으로 이동하여, "소스 코드 관리" 항목에 "Git" 선택 후 본인의 Git Repository 주소 입력 및 Credential 추가
7. Gitlab 통해 Jenkins에 빌드 하려면 "구성" - "빌드유발" 항목에서 "Build when a change is pushed to Gitlab" 항목 체크
8. 빌드유발 항목의 "고급"에서 "Secret token" 항목 "Generate" 클릭하여 Secret token 값 저장.
9. Gitlab 로그인 이후 본인 Gitlab 프로젝트에서 "Settings" - "Integrations" - URL에 본인 Jenkins 프로젝트 주소 입력 및 Jenkins에서 받은 Secret Token 값 입력.
10. "Push events" 항목에 체크하고 "Add Webhook". 생성된 Webhook에 "Push events" 옵션 추가.


## Jenkin-Sonarqube 연동
1. Sonarqube 로그인 이후 본인 프로파일에서 "My Account" 클릭
2. "Generate Tokens"에서 임의로 명칭을 입력하고, "Generate" 클릭 및 토큰 저장
3. Jenkins에서 구성창으로 들어가 "Build"의 "Add build step" 클릭하여 "Execute Sonarqube Scanner" 추가
```
# "SonarQube Installation" 입력
# "JDK" 입력
# Analysis properties 입력
sonar.projectKey=[프로젝트키]
sonar.projectName=[프로젝트명]
sonar.login=[토큰값]
sonar.sources=src              /*소스파일 위치 명시*/
sonar.java.binaries=target/classes           /*binary 위치 java일 경우 예입니다.*/
```
```
#참고 프로퍼티
sonar.host.url=https://cody-cqs.autoever.com/         /*소나큐브 url 설정*/
```