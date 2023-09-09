# 🤜묵찌빠 게임

콘솔을 통해 가위,바위,보에 해당하는 숫자를 입력한 후 무승부이거나 잘못 입력하면 게임을 이어가고, 승패가 갈린 경우 묵찌빠게임으로 이어가는데 같은 숫자이면 이기고, 승패에 따라 턴이 넘어가는 게임이다.

## 목차

1. [팀원](#1-팀원)
2. [순서도](#2-순서도)
3. [타임라인](#3-타임라인)
4. [실행 화면(기능 설명)](#4-실행화면기능-설명)
5. [트러블 슈팅](#5-트러블-슈팅)
6. [참고 링크](#6-참고-링크)
7. [팀 회고](#7-팀-회고)

<br>
## 1.팀원

| [mireu](https://github.com/mireu79)  | [uemu](https://github.com/ue-mu) |
| :--------: | :--------: |
|<img src=https://github.com/mireu79/ios-rock-paper-scissors/assets/125941932/b4a69222-b338-4a7f-984c-be5bd78dc1d8 height="150"/> |<img src=https://github.com/mireu79/ios-rock-paper-scissors/assets/125941932/2f6e81f9-f2f4-4830-982f-a513622c3fcc height="200"/> | 

<br>

## 2. 순서도

![](https://hackmd.io/_uploads/HyH-9Iu0n.png)

<br>

## 3. 타임라인
|날짜|내용|
|------|---|
|23.09.04|공식문서공부 후 STEP1 코드구현<br>`컴퓨터의 랜덤 함수구현, 사용자 입력과 컴퓨터의 패 비교 함수 구현, 게임실행함수구현, 함수명 및 출력함수 수정`|
|23.09.05|STEP1 PR작성후 피드백<br> `사용자 입력받는 함수 기능분리,컨벤션 및 들여쓰기 수정, while 네이밍 수정, if문 switch문으로 수정`|
|23.09.06 |요구사항 수정 및 개행수정 |
|23.09.07|주석 제거 및 불필요한 print문 제거 및 switch문 내의 상수제거, STPE2 코드 구현<br>`enum타입 생성, 가위바위보게임 구조체 정의 , 묵찌빠게임 구조체 정의, 랜덤함수 구현 및 전역변수 구현, 구조체 네이밍 수정, print문 형식에 맞게 수정, 개행 수정`|
|23.09.08|STEP2 PR작성, 피드백반영, READ ME작성 |
 
<br>

## 4. 실행화면(기능 설명)
- 가위바위보

| 0으로 게임종료  | 비겼을떄 |
| :--------: | :--------: | 
|<img src=https://hackmd.io/_uploads/BJYiA8ORn.gif>|<img src=https://hackmd.io/_uploads/SkfOJPuCh.gif>|
| 0으로 게임종료 |
|<img src=https://hackmd.io/_uploads/ryJXUvOAn.gif >|




- 묵찌빠




| 사용자 승리 시 | 컴퓨터 승리 시 |
| :--------: | :--------: |
| <img src=https://hackmd.io/_uploads/S1PyDPu0h.gif >|<img src=https://hackmd.io/_uploads/SJGHPvdRh.gif >|![](https://hackmd.io/_uploads/H1Aiew_An.gif)|
| 잘못된 입력과 0으로 게임종료 |
| ![](https://hackmd.io/_uploads/H1Aiew_An.gif)|
  

<br>

## 5. 트러블 슈팅
### 1️⃣ Bool타입
> Step1 가위바위보게임에서 사용자 승, 컴퓨터 승 모두 true로 반환하고 비긴 경우를 false로 반환 했기 떄문에 묵찌빠로 승자 입력 값을 받을때 사용자와 컴퓨터 둘다 받아지는 현상을 수정하기 위해 반환값을 주지 않고 승리자 턴을 enum으로 지정해서 승리자 턴을 넣었습니다.

<br>

- 수정전

 ~~~ swift
func contendUserAndComputer(_ userChoice: Int, _ computerChoice: Int) -> Bool {
    if (userChoice == 1 && computerChoice == 3) || (userChoice == 2 && computerChoice == 1) || (userChoice == 3 && computerChoice == 2) {
        print("이겼습니다.")
        return true
    } else if userChoice == computerChoice {
        print("비겼습니다.")
        return false
    } else {
        print("졌습니다..")
        return true
    }
}
~~~
 - 수정후
 
~~~ swift
mutating func compareUserPickAndComputerPick(_ userChoice: RockPaperScissorsChoice, _ computerChoice: RockPaperScissorsChoice) {
        switch (userChoice, computerChoice) {
        case (.rock, .scissors), (.paper, .rock), (.scissors, .paper):
            print("이겼습니다!")
            turn = "사용자"
            nextGame.playMukChiBbaGame()
        case (.rock, .rock), (.paper, .paper), (.scissors, .scissors):
            print("비겼습니다!")
            playRockPaperScissors()
        default:
            turn = "컴퓨터"
            print("졌습니다!")
            nextGame.playMukChiBbaGame()
        }
    }
~~~

 
### 2️⃣ Mutating
- `Cannot assign to property: 'self' is immutable 오류`
  - `turn`속성을 수정했고 속성이 현재 객체의 속성으로 선언되어 있기 때문에 오류가 발생해서 값타입 내 메서드에는 수정을 할 수 없기떄문에 mutating키워드를 사용하여 오류를 해결할수 있었습니다.



<br>

 - 수정 전
~~~ swift
  func compareUserPickAndComputerPick(_ userChoice: RockPaperScissorsChoice, _ computerChoice: RockPaperScissorsChoice) {
        switch (userChoice, computerChoice) {
        case (.rock, .scissors), (.paper, .rock), (.scissors, .paper):
            print("이겼습니다!")
            turn = .user
            nextGame.playMukChiBbaGame()
        case (.rock, .rock), (.paper, .paper), (.scissors, .scissors):
            print("비겼습니다!")
            playRockPaperScissors()
        default:
            print("졌습니다!")
            turn = .computer
            nextGame.playMukChiBbaGame()
        }
    }
~~~
 - 수정 후
~~~ swift
 mutating func compareUserPickAndComputerPick(_ userChoice: RockPaperScissorsChoice, _ computerChoice: RockPaperScissorsChoice) {
        switch (userChoice, computerChoice) {
        case (.rock, .scissors), (.paper, .rock), (.scissors, .paper):
            print("이겼습니다!")
            turn = .user
            nextGame.playMukChiBbaGame()
        case (.rock, .rock), (.paper, .paper), (.scissors, .scissors):
            print("비겼습니다!")
            playRockPaperScissors()
        default:
            print("졌습니다!")
            turn = .computer
            nextGame.playMukChiBbaGame()
        }
    }
~~~


<br>

## 6. 참고 링크
[📖 공식문서 Naming](https://www.swift.org/documentation/api-design-guidelines/)<br>
[📖 공식문서 Control Flow](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/controlflow/)<br>
[📖 공식문서 Functions](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/functions/)<br>
[📖 공식문서 Enumerations](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/enumerations/)<br>
[📖 공식문서 Properties](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/properties/)<br>
[📖 공식문서 Methods](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/methods/)


<br>

## 7. 팀 회고
- 😄우리팀이 잘한 점
    - 시간을 정하고 계획과 코드를 짜는 부분에서 의견조율과 소통이 잘됐습니다.
     - 소통을 너무 잘해주셨고, 피드백을 잘해주셔서 프로젝트 진행에 차질없이 진행할수 있었습니다.

<br>

- 😅우리팀이 개선할 점
    - 코드 컨벤셜을 꼼꼼히 확인하지 않고 커밋을 보내는 오류를 범해서 신중하게 커밋을 보내야할 필요가 있다고 생각했습니다.
