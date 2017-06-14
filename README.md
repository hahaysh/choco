Microsoft Build Tour의 Serverless 세션에서 시연하였던 일명 “안촉촉한 초코칩” 데모 자습서를 간단히 만들어 보았습니다. 
이 자습서를 진행하려면 우선 Microsoft Azure 구독이 필요하며, 구독이 없으신 분이라면 https://azure.microsoft.com/ko-kr/free/ 에서 30일간 240,000 한도 내에서 사용하실 수 있는 체험 계정을 신청하실 수 있습니다. 음성 파일을 내보내기 위해서 DropBox 계정을 필요로 하지만 필수는 아니며 One Drive로 파일을 저장하거나 혹은 Storage Explorer(http://storageexplorer.com/) 등을 이용하여 Azure의 Blob Storage에 저장된 파일을 직접 확인할 수 있습니다.

## Azure Logic App 생성
1.	좌측 메뉴에서 ‘새로 만들기’ 를 누른 후, 논리 앱(Logic App)을 검색한 후,  결과 리스트에서 Logic App을 선택합니다.
2.	‘만들기’를 선택합니다.
3.	‘이름’란에 choco 라고 입력합니다. ‘리소스 그룹’이하에 ‘새로 만들기’가 선택되어 있는지 확인하고 choco-rg 라고 입력합니다. 위치에서 한국에서 가장 가까운 일본 동/서부 중 하나를 선택합니다.(한국 데이터 센터에서는 아직 Logic App을 지원하지 않지만 근시일 내에 지원할 것으로 예상합니다.)
4.	하단에 ‘대시보드에 고정’을 선택하고, ‘만들기’를 누릅니다.

![create logic app](/images/createblob.png)
 
## 저장소 계정 생성
1.	좌측 메뉴에서 ‘리소스 그룹’을 선택한 후, 이전 단계에서 생성한 choco-rg를 누릅니다.
2.	상단 메뉴에서 ‘+추가’를 누릅니다. 
3.	‘검색’ 창에서 ‘storage account’를 검색한 후, ‘Storage account – blob, file, table, queue’를 선택합니다.
4.	‘만들기’를 누릅니다.
5.	‘Name’ 하단에 적절한 이름의 저장소 주소를 생성합니다. 이 값은 Globally unique한 URL이어야 하며, 이번 자습서에서는 ‘chocostorage’라는 이름을 사용합니다.
6.	‘복제’ 이하에서 ‘LRS(로컬 중복 저장소)’를 선택합니다.
7.	‘대시보드에 고정’을 선택한 후 ‘만들기’를 누릅니다.

##	Blob Container 생성
1.	대쉬보드에서 ‘chocostorage’를 선택합니다.
2.	‘서비스’ 하단의 Blob을 선택합니다.
3.	상단 메뉴에서 ‘+컨테이너’를 선택합니다. 
4.	이름 하단 입력란에 ‘audio’라고 입력한 후, 확인을 누릅니다. 

##	Facebook Trigger 구성
1.	대쉬보드에서 choco 라는 이름으로 생성된, 논리 앱(Logic App)을 선택합니다.
2.	‘템플릿’ 하단의 ‘비어 있는 논리 앱’을 선택합니다.
3.	논리 앱 디자이너에서 facebook 을 검색합니다.
4.	‘트리거’ 이하에서 ‘Facebook – 내 타임라인에 새 게시물이 있는 경우’을 선택합니다.
5.	‘로그인’ 버튼을 눌러 Facebook에 로그인 합니다.
6.	‘빈도’는 분으로 간격은 1로 수정합니다.
7.	‘+ 새 단계’를 누릅니다.
8.	‘작업 추가’를 누릅니다.

##	음성 변환
1.	검색 창에서 ‘음성’을 검색합니다.
2.	‘작업’ 하단에서 ‘Microsoft Translator – 텍스트를 음성으로 변환’을 선택합니다.
3.	텍스트 우측의 입력란을 선택하고 ‘피드 항목 상태 메시지’를 선택합니다.
4.	언어 코드 우측의 리스트에서 ‘Korean(Korea)’를 선택합니다.
5.	‘+ 새 단계’ 를 누릅니다.
6.	‘작업 추가’를 누릅니다.

##	음성 파일 저장
1.	검색 창에서 “Azure Blob Storage”를 검색합니다.
2.	‘작업’ 하단에서 ‘Azure Blob Storage – BLOB 만들기’를 선택합니다.
3.	‘연결 이름’  우측에 ‘audio storage’라고 입력합니다.
4.	‘저장소 계정’하단에서 ‘chocostorage’를 선택합니다.(1단계에서 생성한 저장소 계정 이름을 입력합니다.)
5.	‘만들기’를 누릅니다.
6.	‘폴더 경로’ 우측에 /audio라고 입력 합니다.
7.	‘Blob 이름’ 우측 입력란에서  ‘피드 항목 게시물 ID’를 선택한 후, 연이어 .wav라고 입력합니다.
8.	‘Blob 콘텐츠’ 우측 입력란에서 ‘오디오 파일’을 선택합니다.
9.	‘작업 추가’를 누릅니다.

![create audio file](/images/createblob.png)

##	DropBox 저장
1.	검색 창에서 dropbox를 선택합니다.
2.	‘작업’ 하단에서 ‘Dropbox – 파일 만들기’를 선택합니다.
3.	‘로그인’ 버튼을 눌러 Dropbox에 로그인 합니다.
4.	‘폴더 경로’ 우측 입력란에 / 를 입력합니다. 
5.	‘파일 이름’ 우측 입력란에서 ‘BLOB만들기’아래에 있는 Name을 선택합니다.
6.	‘파일 콘텐츠’ 우측 입력란에서 ‘오디오 파일’을 선택합니다.

##	저장 및 실행
1.	논리 앱 디자이너 상단의 ‘저장’을 누른 후, ‘실행’을 누릅니다.
2.	페이스북에 로그인한 후 음성으로 변경할 문장을 입력합니다. ‘안촉촉한 초코칩 나라에 살던 안촉촉한 …’
3.	Logic App 디자이너로 돌아오면, 다음과 같이 Logic App이 수행되었음을 확인할 수 있습니다.
 
##	음성확인
1.	DropBox에 접속하여 생성된 .wav 파일을 수행하여 음성을 확인합니다.

![result](/images/result.png)

감사합니다.