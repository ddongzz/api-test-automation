Petstore API E2E 테스트 자동화

개요
Petstore API를 활용하여 데이터 생성부터 검증, 수정, 삭제까지 사람의 개입 없이 진행되는 무인 자동화 테스트 시나리오입니다. 
단편적인 API 호출 테스트를 넘어, 비즈니스 로직의 E2E(End-to-End) 흐름을 검증하는 데 목적이 있습니다.

기술 스택
API Testing Tool : Postman
Scripting : JavaScript (Postman Tests)
Data Format : JSON

테스트 시나리오 (CRUD 체이닝)
환경 변수(`petid`)를 활용하여 각 단계의 데이터가 자동으로 다음 단계로 전달되도록 데이터 흐름(Chaining)을 구현했습니다.

1. POST (생성) : 새로운 반려동물 데이터 생성 및 응답된 ID를 환경 변수에 저장
2. GET (조회) : 변수에 저장된 ID로 데이터를 조회하여 정상 생성(200 OK) 검증
3. PUT (수정) : 해당 ID의 상태 및 이름 데이터 업데이트 검증
4. DELETE (삭제 - Teardown) : 테스트 격리성(Test Isolation)을 유지하기 위해 생성했던 임시 테스트 데이터 삭제
5. GET (삭제 확인) : 삭제된 ID를 다시 조회하여 404 Not Found 상태 코드가 정상 반환되는지 최종 검증

핵심 구현 및 문제 해결
동적 변수 할당 : 하드코딩을 배제하고, Response Body에서 값을 파싱하여 환경 변수에 동적으로 할당.
테스트 격리(Isolation) 확보 : 테스트 실행 횟수에 상관없이 항상 멱등성(Idempotence)을 유지하도록 시나리오 마지막에 DELETE 요청을 통한 Teardown 로직 구성.
HTTP 헤더 제어 : 415 Unsupported Media Type 에러 해결을 위해 `Content-Type: application/json` 헤더 강제 할당 처리.

실행 방법
1. 이 저장소의 `.json` 파일 2개(Collection, Environment)를 다운로드합니다.
2. Postman 좌측 상단의 `Import` 메뉴를 통해 두 파일을 모두 가져옵니다.
3. Postman 우측 상단 드롭다운에서 `QA_Study` 환경을 선택합니다.
4. Collection 메뉴에서 `Run collection`을 선택하여 전체 시나리오를 일괄 실행합니다.
