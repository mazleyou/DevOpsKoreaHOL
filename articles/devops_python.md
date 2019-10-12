## index
1. devops 프로젝트 생성
2. python app repository 연동
3. Azure portal 에서 function app 생성
4. devops 에서 pipelines 설정
5. CI/CD 테스트

## devops 프로젝트 생성
1. project name 입력 후 private로 프로젝트 생성

## python app repository 연동
1. devops 왼쪽 하단의 Project Setting 메뉴 선택
2. Board >> Git Connections >> Connect your GitHub account 메뉴 선택
3. Github 로그인을 통해 Azure Board 동기화
4. Github 의 python app 프로젝트 지정후 Save
5. Github Connection 페이지에서 Github과 연동이 되었는지 확인

## Azure portal 에서 function app 생성
1. 하단의 commend를 azure portal cli에서 입력
'az group create --name "RG명" --location westeurope
az storage account create --name "SA명" --location westeurope --resource-group "RG명" --sku Standard_LRS
az functionapp create --resource-group "RG명" --consumption-plan-location westeurope --os-type Linux --name "functionapp명" --storage-account "SA명" --runtime python'

## devops 에서 pipelines 설정
1. devops에서 pipelines 선택
2. code 위치선택에서 use the classic editor 선택
3. github 선택
4. empty job 선택
5. 상단 메뉴탭에서 triggers 선택
6. enable continuous integration 체크
7. 상단 메뉴탭에서 Tasks 선택
8. Agent job 1 선택 후 Agent pool을 ubuntu 16.04 선택
9. "+"버튼 클릭 후 Use python version 추가 
10. "+"버튼 클릭 후 Azure Cli 2개 추가 
11. python version 3.6으로 수정
12. 첫번째 azure cli 에서 구독 선택 후 Inline script로 변경 하기의 소스 입력
'curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
sudo mv microsoft.gpg /etc/apt/trusted.gpg.d/microsoft.gpg

sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-ubuntu-$(lsb_release -cs)-prod $(lsb_release -cs) main" > /etc/apt/sources.list.d/dotnetdev.list'
sudo apt-get update

sudo apt-get install azure-functions-core-tools'
13. 첫번째 azure cli 에서 구독 선택 후 Inline script로 변경 하기의 소스 입력
'cd python-functions
func azure functionapp publish "functionapp명" --python --build-native-deps'
14. Save & queue 클릭

## CI/CD 테스트
1. repository의 소스 수정 후 commit
2. 변경된 소스가 azure function app 에 반영이 되어있는지 확인

