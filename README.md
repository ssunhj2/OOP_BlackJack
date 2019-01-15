# OOP_BlackJack


okky에서 fender님의 글을 보고 시작하게 된 프로젝트.  
아래 주소로 이동하면 fender님의 글을 볼 수 있다.  
https://okky.kr/article/358197  

## 프로젝트 목적
* 객체지향적 설계 연습
* 구현보다 설계에 가중치를 둠.
* 요즘 관심있는 TDD까지 적용해보고싶음.


## 진행방식
* 처음 적용할 규칙은 다른것을 추가하지 않은 가장 기본 규칙으로 설계한다.
* 규칙을 추가하며 점점 업그레이드 하는 방식으로 진행한다.   


### 규칙 (기본규칙)
#### 요약 
딜러와 그 외 참가자들의 대결.  
카드 숫자의 합이 21이하이면서 21에 가장 가까운 사람이 승리하게 된다.    

#### 세부규칙
* 사용되는 카드는 총 52장이다.  
모양은 하트,다이아몬드,스페이드,클로버 4가지이다.  
숫자는 2~10 영문은 K, Q, J 다.
* 숫자가 써있는 카드는 써있는 숫자 그대로 계산한다.
* K, Q, J는 10으로 계산한다.
* 에이스는 소유자가 1 또는 11 어느쪽으로도 계산 가능하다.
* 카드 숫자의 합이 21을 초과하게 되면 '버스트'로 딜러의 결과와 관계없이 무조건 돈을 잃는다.


#### 게임진행 
1. 딜러 이외의 사람들은 배팅금액을 정한다. (배당율은 건 금액만큼 받는다.)  
ex)100원을 배팅했을때, 이기면 200원을 얻고, 지면 100원을 잃는다.
2. 딜러가 자기 왼쪽부터 카드를 1장씩 돌리고, 이것을 다시 반복한다. 이 작업이 끝나면 모두 카드를 2장씩 갖게 된다.
3. 딜러의 첫번째 카드는 공개하지 않고, 그 외의 모든 카드는 공개한다.(딜러의 카드는 한장만, 다른 참가자의 카드는 모두 공개된다.)
4. 참가자들은 카드의 합계가 21보다 작은경우, 딜러에게 카드를 추가로 받거나, 받지 않을 수 있다.  
이때, 받을 수 있는 카드의 수는 제한이 없다.
5. 참가자들의 카드추가가 모두 끝난 후,   
딜러의 카드 합계가 16점 이하면 1장을 추가하고, 17점 이상이면 카드를 추가할 수 없다.
6. 딜러와 점수를 비교해서 동점이면 무승부, 딜러보다 높으면 이기고, 낮으면 진다.
7. 무승부인 경우 딜러가 배팅액을 가져가게 된다.
8. 버스트인 경우, 딜러가 이기게 된다.


### 설계시 고려할 점 (요즘 개발하면서 중점적으로 신경쓰던점들)
* 객체의 메소드는 하나의 역할(행동)만 한다. / 각 기능을 최소단위로 잘게 나눈다.(자신의 역할만 충실히 하도록 한다.)
 * 카드를 받고 공개한다고 하면 '카드를 받는다.', '카드를 공개한다.' 메소드를 각각 정의하여 행위를 최소단위로 나눠준다.
* 종속성, 결합도를 낮춘다.
* 객체지향적으로 설계한다.
* 객체나 메소드이름은 직관적으로 알 수 있는 이름으로 한다.
* 객체지향의 특징(추상화, 캡슐화, 다형성, 상속성)을 최대한 적용한다.


### 객체
* 카드
* 카드뭉치
* 딜러
* 시스템
* 참가자


### 객체의 속성 및 행동(요구조건)
* 카드
  * 하트, 다이아몬드, 스페이드, 클로버 중 1개의 모양을 가진다. (4가지 모양)
  * 숫자 2~10, K,J,Q, 에이스 등 숫자를 가진다. (13개 숫자)  

* 카드뭉치
  * 총 52장의 카드를 가진다.
  * 52장의 카드 중 랜덤하게 1개를 뽑아준다.(한번 나온 카드는 또다시 나올 수 없다.)
  * 카드를 섞는다.  
  
* 딜러
  * 카드를 받는다.
  * 받은 카드를 소유한다.
  * 처음 카드를 1장만 공개한다.
  * 나머지 카드를 공개한다.
  * 카드의 합계가 16점 이하면 1장을 추가한다. 17점 이상이면 받을 수 없다.  
  * *딜러는 배당금을 따로 저장하지않는다.*  
  
* 시스템 (딜러가 규칙진행도 하려고 했으나, 역할을 단순화하기 위해 시스템을 따로 뺐다.)
  * 참가자들의 배팅액을 저장한다.
  * 참가자들와 딜러의 점수를 계산한다.
  * 승자를 선정한다.
  * 배당금을 승자에게 준다.  
  
* 참가자
  * 카드를 받는다. 
  * 받은 카드를 소유한다.
  * 카드를 추가한다.
  * 카드를 공개한다.
  * 카드를 받지 않는다.
  * 금액을 보유한다.
  * 배팅한다.
  * 배당금을 받는다.  
    
      

## STEP 01. 밑그림 설계도(인터페이스 작성)  
#### 인터페이스 먼저 작성하는 이유  
그림을 그릴때 밑그림을 그려 전체 모양을 구상하고, 그 다음 구체적으로 그려나가기 시작한다.  
클래스는 객체의 설계도이고, 추상클래스는 미완성 상태의 설계도다.   
인터페이스는 추상클래스의 일종이고, 구현된게 없는 밑그림 상태의 설계도다. (추상클래스보다 추상도가 높다.)  
가장 밑그림부터 시작하기 위해 인터페이스를 설계를 가장 먼저 시작하겠다.  

#### 인터페이스 특성
* 인터페이스는 추상메서드와 상수만을 멤버로 가진다. 
* 인터페이스는 추상클래스처럼 몸통을 갖춘 메서드를 정의할 수 없다.  
* 인터페이스는 그 자체로는 인스턴스를 생성할 수 없으며, 상속을 통해 구현부를 완성한다.

#### 기타 특성
* 모든 멤버변수는 public static final 로 정의하며, 생락가능하다.
* 모든 메서드는 public abstract 로 정의하며, 생략이 가능하다. (jdk 1.8부터 static메서드와 default메서드도 가능)


#### 설계  
연습을 위해 위의 기타특성의 생략 가능한 문법들 생략 안하고 진행.  
현재 단계에서는 구체적인 구현이 아니라 밑그림을 작성하도록 한다.

~~~ 
// 카드
interface Card
{
   public static fianl int CARD_PATTERN_CNT = 4; // 카드 무늬 수
   public static final int CARD_NUMBER_CNT = 13;  // 카드의 숫자 수
   
   public static final int HEART = 4;
   public static final int DIAMOND = 3;
   public static final int SPADE = 2;
   public static final int CLOVER = 1;
   
} 
~~~

~~~ 
// 카드뭉치
interface CardDeck
{
   public static final int CARD_TOTAL_CNT = 52; // 카드 총 개수
   
   public abstract Card pick(); // 카드를 뽑는다.
   public abstract void shuffle(); // 카드를 섞는다.
   
} 
~~~

~~~ 
// 딜러
interface Dealer
{
   public abstract boolean isAcceptCard(); // 카드 합계에 따라 카드를 받아야할지 말아야할지 확인
   public abstract void ownCard(Card card); // 카드를 소유한다.
   public abstract Card openCard(int index); // index 번째 카드를 공개한다.
   
} 
~~~

~~~ 
// 시스템
interface RuleManager
{
   public abstract void acceptDevidends(); // 참가자들에게 배당금을 받는다.
   public abstract int calculateScore(List<Card> cardList); // 점수를 계산한다.
   public abstract Player deviceWinner(); // 승자를 선정한다.
   public abstract void giveMoney(Player winner); // 승자에게 배당금을 계산하여 준다.
   
} 
~~~

~~~ 
// 참가자
interface Player
{
   public abstract void betting(); // 배팅한다.
   public abstract void acceptCard(int cardCnt); // 카드를 받는다.
   public abstract void passCard(); // 카드를 받지 않는다.
   public abstract void ownCard(Card card); // 카드를 소유한다.
   public abstract List<Card> openCard(); // 카드를 공개한다.
} 
~~~

 
