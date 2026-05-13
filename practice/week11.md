# week 11 실습

## 오늘 한 것
- pyinstaller 설치 및 빌드
- resourece_path() 함수 추가
- --add-data 옵션으로 에셋 포함
- .exe 실행 확인

## resource_path()를 써야하는 이유
- 게임을 exe 파일로 배포했을 때 이미지 및 사운드 파일들을 제대로 찾게 하기 위해서다. 단순히 "assets/plater.png" 라고 쓰면 파일을 못 찾는 경우가 생길 수 있기 때문에,
- resource_path()를 사용해서 현재 실행 환경에 맞는 정확한 절대 경로를 만들어주는 것이다.

## 빌드 명령어
- --onefile : 파일 하나로 합치기
- --windowed : 실행 시 검은색 터미널 창 숨기기
- --add-data "assets;assets" : 이미지나 사운드가 든 assets 폴더를 exe 안에 포함시키기
- --name=Mygame : 파일 이름을 Mygame으로 지정

## AI 활용 내용
- 코드를 추가했는데 오류가 발생한다. 이거 어떻게 해결해야하나 : 스크린샷보니 들여쓰기 오류 발생한거니, Tab 키 눌러서 들여쓰기하면 해결된다. -> 진짜 해결되었습니다.
