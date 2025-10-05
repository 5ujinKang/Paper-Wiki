요약 : 체크포인팅 → 이미지 저장 / 이미지가 컨테이너들 사이에서 공유 
⇒ speedy restoration

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4d40f194-4938-4a3d-a02f-d9b86860f5f7/Untitled.png)

- 체크포인팅 →(확장)→ 메모리 페이지 restore

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7dcccfbf-0547-4722-bc5a-d6b3bf962cbb/Untitled.png)

- page fault 관련 내용
: 체크포인팅 하기 전에 Garbage Collection을 trigger ⇒ compact heap 으로 만들기⇒ object들끼리 서로 가까이 위치 ⇒ 더 적은 page faults
