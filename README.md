장르: 2D 플랫 포머 게임
소개: 
장애물과 몬스터 들을 피해 과일 아이템을 먹거나 몬스터를 퇴치하여 점수를 획득하고 골인 지점까지 통과하는 게임 

맵: 타일 맵

아이템: 
-	사과 : score + 30
-	파인애플 : score + 40
-	멜론 : socre +50
지형지물
-	나무 가시 : 충돌 시 목숨 -1
-	트램펄린 : 충돌 시 점프 
-	톱니바퀴 : 충돌 시 목숨 – 1
-	포탈 : 충돌 시 출발 지점으로 이동 
-	깃발 : 다음스테이지로 이동
-	트로피 : 게임 종료 
몬스터
-	가면 외계인 : 일반 몬스터로 한번에 밟아서 퇴치 
-	개구리 닌자: 목숨이 두 개 이므로 총 두 번 밟아야 퇴치 
 
게임 시작 시 로비화면

![image](https://github.com/user-attachments/assets/19df1bd2-6e1c-4a8b-b5a9-12a1e4440ef3)

게임 진행 첫 화면 

![image](https://github.com/user-attachments/assets/4639648d-0d5a-4011-8841-4929dbb737d5)

전투 장면 

![image](https://github.com/user-attachments/assets/fefe843e-5591-4cc3-a1e9-593b744d8ae5)


사망 장면
 
![image](https://github.com/user-attachments/assets/7a2ad189-fd72-4e61-8d74-93495ba32dec)



2. 조작법 

방향 키: 좌, 우 이동
스페이스바 : 점프 

3. 구현 내용
A. 카메라
- 플레이어의 상속을 통해 플레이어를 타겟으로 지정하여 플레이어 이동시 따라다니게 하였습니다. 
B 플레이어 구현
- 플레이어 모션 
 (1) 플레이어의 이동은 Horizontal 을 받고 키 입력에 따라 속도를 받아 이동합니다.

![image](https://github.com/user-attachments/assets/5e360052-ad2d-4ad8-b09f-011910c24d79)

 
 (2) 플레이어 점프는 Jump(Spacebar)키를 입력 받으면 rigidbody의 addforce를 이용하여 Vector2.up에 점프 파워를 곱하여 점프가 가능하게 하였고 점프상태에서 연속 점프를 방지 하기 위해 애니메이터의 파라미터 bool값을 활용 하였습니다. 

 ![image](https://github.com/user-attachments/assets/bc54a3c5-1791-4462-b167-4bba8dbb5a5c)

 
 (3) 플레이어 공격은 점프한 상태에서 몬스터 머리를 밟으면 공격처리를 하도록 하였습니다. 
이 과정에서 플레이어는 충돌체의 정보를 받고 충돌체의 레이어가 몬스터 레이어 이면 플레이어 자신이 낙하중이고, 자신의 y좌표가 충돌체의 y좌표보다 높을 시 공격처리를 하여 무조건 플레이어가 몬스터 상단에서 밟았을 시 피격이 가능하도록 하였습니다. 

![image](https://github.com/user-attachments/assets/6a5e9ee6-796e-4b9f-9a2b-1cc8748ec5e9)

 
몬스터 공격 시 플레이어는 몬스터의 transform을 받아 몬스터가 살짝 튕겨져 나가게 하여 타격감을 살리는 애니메이션 또한 출력하게 하였습니다. 또한 몬스터를 구분하기 위해서 contains를 활용하여 몬스터의 이름으로 종류를 구분 하였습니다. 

![image](https://github.com/user-attachments/assets/8eb512fd-cbd3-4775-a8f4-ebe6174411e9)

 
(4) 플레이어는 장애물,몬스터와 공격상태가 아닌 일반상태일 때 충돌 시 목숨이 감소하며 목숨은 게임메니저에서 관리를 하도록 하였습니다. 
플레이어가 공격을 받았을 때 플레이어의 투명도를 높여 무적상태를 표현하였고 공격당한 반대방향으로 튕겨나가게 하여 타격감을 살렸습니다. 

![image](https://github.com/user-attachments/assets/324a4ef1-1ecc-410b-b0b4-c3f45e3096b1)

 
(5) 플레이어는 OnTriggerEnter2D를 이용하여 충돌하는 객체의 태그 값을 비교하고 판단하여 그에 맞는 상황을 연출 하였습니다. 

![image](https://github.com/user-attachments/assets/ef6d25f1-4e03-498c-8872-e474e399427c)

 
- 애니메이션

 ![image](https://github.com/user-attachments/assets/3cb0a812-d9cd-4233-90c4-700c0ecef5e4)

 
(1) 이동과 점프 상태에 따라서 애니메이션을 부여하고 상태에 맞는 애니메이션을 출력하도록 하였습니다 좌 우 반전은 스크립트 내부에서 스프라이트 플립을 통해 방향에 맞는 애니메이션이 나오도록 하였습니다. 

![image](https://github.com/user-attachments/assets/bc3e54c8-c84d-44f0-bef4-2d05647ad5af)

 
(2) 피격 애니메이션은 트리거 형태로 지정해 주고 어떤 상황에서도 공격을 입으면 바로 애니메이션이 출력되게 하였고 피격 시 레이어 분리를 통해 2초 동안 무적 상태에 들어가며 플레이어가 투명해졌다가 다시 돌아오도록 설정하였습니다. Invoke 함수를 통해 함수가 2초뒤에 재실행 되게 활용 하였습니다. 
(무적)

![image](https://github.com/user-attachments/assets/20bef705-90dc-41ed-8015-b911a1a274bb)

 
(무적해제)

![image](https://github.com/user-attachments/assets/bc7a1855-a1a4-40ae-83b9-3bde0e9121a5)

 
- 사운드 
 (1) AudioSource를 컴포넌트로 설정해주고 상황에 맞는 AudioClip을 받고 각 상황에 맞게 효과음을 낼 수 있도록 하였습니다

![image](https://github.com/user-attachments/assets/adfa4668-6e62-4603-b5b6-428dd01d5d4a)

 
 
C 몬스터 구현 
 - 몬스터 모션 
 (1) 몬스터 또한 플레이어와 같은 방법으로 벡터를 받아 이동하게 구현하였습니다. 몬스터는
 Raycast 를 통해 이동방향이 낭떠러지인지 구분하고 앞에 타일이 없다면 이동하던 방향을 자동으로 바꿔 반대방향으로 이동하게 하였습니다. 낭떠러지가 아닐 시에는 5초에 한번씩 Idle Move 상태를 랜덤하게 행동하도록 설정하였습니다. 
( Raycast를 통해 이동방향 타일 확인)

![image](https://github.com/user-attachments/assets/a9d49467-93a6-4fac-8205-9679a64d9643)

 
(몬스터 랜덤하게 이동)

![image](https://github.com/user-attachments/assets/03019c48-2a11-42f2-9efc-f7f5ac9d3d7c)

 
- 몬스터 피격 
 (1) 플레이어와 비슷한 방법으로 몬스터 또한 피격 시 투명도를 높여 몬스터 피격상태로 바뀌고 피격 애니메이션을 출력합니다. 몬스터 1은 피격 시 2초 뒤에 사망 상태로 들어가게 Invoke를 활용하였습니다.

![image](https://github.com/user-attachments/assets/e9aec4bd-4bab-4c3a-87e3-04e52c48cdee)

![image](https://github.com/user-attachments/assets/4dcd6bbe-7ebc-4b75-b68e-89f7b97b478d)

 
“개구리 닌자” 몬스터는 목숨이 두개를 갖고 있기 때문에 두 번 공격을 받아야만 사망하게 됩니다 그래서 체력이 0 일시에 사망하도록 하였고 그 이전에는 플레이어와 동일한 방식으로 레이어를 구분하여 공격받은 상태에서는 2초동안 무적이 되게 됩니다. 
 
D 게임 메니저 구현 
- 스테이지 관리 
 (1) 게임매니저에서 배열로 스테이지를 관리합니다. 플레이어가 각 스테이지 목적지에 도착 시 NextStage()함수를 호출하면 게임 매니저에서 각 스테이지의 활동 상태를 true, false로 교체하면서 스테이지 전환을 구현하였습니다. 

 ![image](https://github.com/user-attachments/assets/91728df9-ad1a-4b15-841c-0b17e756f9c7)

 ![image](https://github.com/user-attachments/assets/563a3148-673c-4fd1-8959-fade10a4c68d)


 
-획득 스코어 
(1) 게임매니저에서 플레이어가 얻은 점수들을 관리하고 각 스테이지에서 얻은 포인트를 다음 스스테이지 옮길 때 0으로 바꾸고 모아 놓은 총 포인트를 total 포인트로 관리를 합니다. 
E 아이템 구현 
- 아이템 종류
(1) 아이템은 총 3종류이며 각 아이템마다 습득 할 수 있는 점수를 다르게 설정하였습니다. 
(2) 아이템들은 기본적으로 Idle 상태의 애니메이션을 보여주고, Trigger를 통해 플레이어와 충돌 시 SetActive를 false로 바꿔 플레이어가 먹은 것 처럼 보이게 했습니다. 
(3) 트렘폴린 아이템 
트램폴린 아이템은 플레이어가 트램펄린 안으로 들어가면 플레이어에게 AddForce를 통해 위로 향하게 하였습니다. 트램펄린 아이템 또한 기본적인 상태에 있다가 트리거로 충돌하게 되면 하늘을 향해 솟아오르는 애니메이션을 출력하게 하였습니다.

![image](https://github.com/user-attachments/assets/9d47ce63-7283-4951-8ad4-eae93462bcdc)

 
F 맵 
- 타일 맵 (Platform)

- ![image](https://github.com/user-attachments/assets/8f027e45-9dc8-4346-9375-1d3f255c458c)

 
맵은 타일맵으로 만들었으며 장애물과 다른 레이어를 사용하여 구분하였습니다. 
 
맵 경사면의 충돌부분에서 오류가 있었지만 스프라이트 에디터의 Custom Physics Shape 기능을 활용하여 해결하였습니다. 

![image](https://github.com/user-attachments/assets/40957105-24ee-4d6d-b20a-bbd3157e9c09)

 
 
G UI (User Interface) 
게임메니저에서 UI를 관리합니다. 
- 로비
(1) 로비 UI 는 게임 스타트 버튼을 생성하여 버튼을 누르면 게임매니저 안의 GameStart함수를 호출하여 게임에 들어갈 수 있게 설정하였습니다. 
(2) 게임 테마에 어울리는 우주인배경으로 로비 UI를 제작하였습니다. 

- 인게임
(1) 좌측 상단에 플레이어 모양의 스프라이트가 존재하고 플레이어가 데미지를 입을 때 마다 한개씩 투명해지도록 설정하여 남은 목숨을 보여주게 하였습니다.

![image](https://github.com/user-attachments/assets/11e267e8-76d4-4117-b755-25c67be76397)

 
(2) 우측 상단에는 모아온 포인트가 보이도록 하였습니다. 
(3) 가운데에는 스테이지가 변경 될 때마다 몇번 째 스테이지인지 표시하도록 하였습니다. 
 
- 엔딩 
(1) 플레이어가 사망하면 retry버튼이 활성화 되고 버튼을 누르면 다시 맨 처음 으로 돌아가게 하였습니다. 
 
(2) 최종 트럼피까지 도달하였을 떄 EXIT버튼과 Retry버튼을 만들어 다시 게임을 할지 게임에서 나갈지 선택할 수 있게 하였습니다. 
 



 
 
