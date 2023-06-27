# 제네릭(Generic)의 탄새 유래가 어떻게 될까?
Go 언어는 강타입 언어이기 때문에 타입 관의 형변환을 잘 해줘야 합니다.
그러면 현재 제네릭이 없어도 Go생태계는 잘 돌아가고 있는데 뭐가 문제일까요?

한번 재가 가상 이야기를 써보면 뭐가 문제인지 이야기 해보도록 하겠습니다.

2023년 06월 27일 19:17분 쯤

코딩을 하고 있는 한모씨에게 친구 김모씨가 접근하여 의뢰를 요청합니다.

"야~ 한모씨야 Go언어를 이용해 최소값을 구하는 함수좀 만들어줘~"

김모씨의 의로 요청 사항을 들은 한모씨는 '엥? 이 쉬운걸' 이라는 표정을 지으며 의뢰를 수락하게 됩니다.
그는 빠르게 에디터를 켜서 최소값을 구하는 함수를 작성했습니다.

```go
// 최소값을 구하는 함수
func min(a, b int) int {
    if a > b {
        return b 
    } 
    return a
}
```

한모씨는 김모씨에게 다가가 다음과 같이 말합니다.

"야~ 너무 쉽다야~ 벌써 다했어 ㅋㅋ"

"아 미안, 내가 어떤 자료형으로 해달라고 구체적으로 말을 안해줬네 ㅎㅎ 다시 정정할게 나는  정수와 실수를 인자로 대입하면 최소값을 반환하는 함수를 만들어줘"

"ㅇㅋ"

이 말을 들은 한모씨는 잠시 고민하더니 좋은 아이디어가 떠오른지 바로 자리에 돌아가 코딩을 하게 된다.

"흠.. 일단 함수를 두개 만들면 되나? 이렇게 하면 되겠다!"

```go
// int 자료형 대상으로 최소값을 구하는 함수
func minInt(a, b int) int {
    if a > b {
        return b 
    }
    return a
}

// float64 자료형 대상으로 최소값을 구하는 함수
func minFloat64(a, b float64) float64{
    if a > b  {
        return b
    }
    return a
}

```

한모씨는 또 김모씨한테 다가가 다음과 같이 말합니다

"야.. 다했어 이제 됬지?"

"앗 뭐야 함수가 두개 잖아?? 하나로 못해ㅠㅠ? 이러면 너무 햇갈리고 인자값으로 대입할 때 형변환을 해줘야 하잖아?"

"그냥 그렇게 해 ㅎㅎ.."

"싫어."

"뭐? 싫다고? 열심히 만들어 왔잖아? 뭐 그러면 방법이 있어?"

"있지! 빈 인터페이스를 이용하면 되잖아!"

"빈 인터페이스는 대소 비교 연산을 할수 없는데요... 만약 형변환 시켜서 한다고 해도 코드가 너무 길어지고 중복되는 코드가 많이 발생해" 자료형

"앗.. 어떻하지"

"너무 불편하다." 

"쌉 ㅇㅈ"


이렇게 해서 둘은 Go 언어에 대한 온갖 욕을 퍼부으며 막을 내립니다.

자 이처럼 Go 프로그래밍 언어는 어떤 함수를 제작하려고 할 때 만드자고 하는 함수가 많은 자료형을 다룬다면 각각에 대응되는 함수를 제작해야만 한다는 단점이 있습니다.

하지만 제네릭(generic)을 사용하면 위 문제를 손쉽게 해결할 수 있습니다.

# 제네릭(generic)은 무엇인가?
제네릭(generic)이란 자료형을 일반화(generalize) 시키는 것을 의미합니다.

# Go언어에서 제네릭(generic)을 어떻게 사용할까?
위 가상 스토리에서 문제가 된 min함수를 제작해 보도록하겠습니다.

먼저 Go언어에서 제네릭을 작성해기 위해선, 아래와 같은 형식을 지켜 작성해야 합니다.
```go
type Integer interface {
    ~int8 | ~int16 | ~int32 | ~int64 | ~int
}

type Float interface {
    ~float32 | ~float64
}

func min[T Integer | Float](a, b T) {
    if a > b {
        return b
    }
    return a
}
```

하나하나, 천천히 알아보죠

먼저 min 함수 구현 부분에 [T Integer | Float]라는 표현식이 있습니다. 
위 표현식은 타입 파라미터라고 부르며, T라는 타입을 지정하는 표현식 입니다. (그러므로 T라는 타입을 지정해 매개변수 정의에 사용할 것을 확인할 수 있습니다.)

그러면 Integer와 Float는 무엇일까요??
바로 위 자료형 선언 부분에서 알 수 있습니다. 
바로 타입 제한자 입니다형

타입 제한자는 말 그대로 타입을 지정하는 것을 뜻합니다.
가령, 아래의 코드가 있습니다.

```go
// YAMMI라는 타입 제한자 (YAMMI는 아무 뜻도 없습니다. 그냥 옆에 음류스가 맛있어서 YAMMI로 했습니다.)
type YAMMI interface {
    int | int8
}
```

이렇게 타입 제한자를 선언하게 되면 YAMMI는 int와 int8를 제한하는(즉, int와 int8를 포함한다.) 타입을 만들 수 있습니다.

- 다음에 ㅠㅠ 시간이 없다.


# --- 그냥 내가 신나서 막 적은거 ---
```go
package main

import "fmt"

func print[T any](a T) {
	fmt.Println(a)
}

func main() {
	var a int = 10
	print(a)
	var b float32 = 3.14
	print(b)
}

// empty interface를 사용하면 되지 왜 Generic을 사용하는가?
/*
	먼저, 빈 인터페이스는 대소 비교를 지원하지 않는다. 이 말인 즉슨
	매개변수를 값을 가공시킬 수 없다는 이야기 이며 형변환을 시켜 할 수도 있지만 매우 귀찮은 과정을 거쳐야 한다.

	그렇기에 제네릭을 사용하면 쉽게 해결할 수 있다.
	그러면 type paremeter에 [T any]로 하면 해결되는 것인가?

	아니다 -> any 타입 같은 경우 구조체, 문자열, 슬라이스 등 말 그대로 any 타입이 들어오기 때문에 대소 비교를 할수 없다. 당연한 이야기다.
	하지만 제네릭을 사용하면 된다. any를 사용하는것이 아닌 대소 비교가 되는 타입을 V로 지정해 주면 된다.

	[T int | int16 | float32 | float64 | int64] 이렇게 말이다.

	어! 그러면 해결됬다 스므수한 제네릭을 사용할 수 있다.
	하지만 프로그래밍은 중복된 것을 싫어한다.
	무슨 말이냐 하면, int관련 타입을 type Parameters에 선언하려면 같은 코드를 반복해야 한다.

	너무 싫다. -> 이것을 해결하기 위해서 "타입 제한자"라는 놈이 탄생했다.


	그렇기에 타입 제한자는 아래처럼 사용한다.

	type 타입 제한자 이름 interface {

	}

	어! 근데 inteface다. 엥? interface데?
	여기서 중요한 점은 타입 제한자를 선언할 때 inteface로 선언해준다는 점이다.
	그러면 왜 interface인가? 엄밀히 따지면 타입 제한자는 interface의 역활을 하지 않는다.
	하지만 어떤 타입인지 제안하고 약속하기 때문에 마치 inteface의 역활을 하는거와 같다고 해서
	타입제한자를 선언할 때  interface로 서술한다.

	참고 : golang은 새로운 용어를 만들기 싫어한다. 철학 중에 단편함이 있기 때문이다.

	그러면 한번 타입 제한자를 선언해보자

	type Integer interface {
		int | int8 | int16 | int32 | int64
	}

	그런 뒤 타입제한자 Integer를 Type Parameters에 사용하면 된다. 아래처럼 이용하면 된다.

	func min[T Integer](arg0, arg1 T) {

	}

	그리고 | 는 매우 유연하다

	만약 타입 제한자 Integer, Float를 만들었다고 하면 (Integer와 Float의 타입제한자 구성요소는 알잘딱으로 생각하길 바란다.)
	func min[T Integer | Float](arg0, arg1 T) {
	...
	}

	이렇게 할 수 있다.
	근데 저 위에도 귀찮다면

	type Numeric interface {
		Integer | Float
	}

	이렇게 하면 된다!
	그러면 이런 타입 제한자를 상시 만들어둬야 하나...
	패키지가 이미 있다!
	golang.org/x/exp/constraints 이거다

	자 위에서 소개한 패키지의 type Signed 타입 제한자의 구현을 보면

	type Signed interface {
		~int | ~int8 | ~int16 | ~int32 | ~int64
	}
	이렇게 되어 있다.

	어? 앞에 ~는 무엇일까?
	이걸 알기 위해선 하나의 예시를 봐야 한다.


	golang은 type을 이용해 별칭을 해줄 수 있다.

	type myInt int

	이렇게 하면 myInt와 int는 엄연히 다른 int라고 볼 수 있다.
	이런 상황에서 myInt를 인자로 넣어 사용하게 된다면? 에러 난다.

	그렇기에 ~를 자료형 앞에 붙여줘서 가령, ~int 이렇게 해줌으로써  base기초, 토대가 int타입도 포함한다는 이야기다.
	이러면 myInt의 base는 int이기 때문에 별칭 타입도 포함된다고 말할 수 있다.

	즉, ~는 별칭 때문에 제네릭이 나오면서 생성된 개념이며 
	타입에 ~를 붙이면 그 타입에 베이스가 되는 타입도 포함한다는 이야기다. 
	
	Very Anwsome 하다. 좋다 golang
	
	---- 정리 ----
	제네릭을 왜 필요한가? 
	-> Golang은 강타입 언어이기 때문에 여러 타입을 지원하는 함수들을 구성하기 너무 힘들었다.
	-> 그러므로 제네릭 타입을 이용해서 여러 타입에서 동작하기 위해 Golang에 태어났다.
	
	(중략, interface{}를 이용하면 된다. 하지만 interface{}은 연산자가 잘 지원되지 않는다. 이런것들)
	
	Okey, 제네릭을 이용해 타입으로 인해 생성된 힘든점을 해결할 수 있었다. 그런데 type Parameter에 따라 연산이 되지 않는다. 
	-> 타입 제한자를 이용하면 된다.
	-> constarints 패키지를 이용하면 된다.
	
	--- 기본 타입 제한자들 ---
	
	any -> 모든 타입 가능
	comparable -> ==, != 타입들만 정의해 놓은 타입 제한자



*/
```

```go

package main

import "fmt"

type myStruct struct {
	Name string
}

func Map[F, T any](s []F, f func(F) T) []T {
	rst := make([]T, len(s))
	for i, v := range s {
		rst[i] = f(v)
	}
	return rst
}


func main() {
	doubled := Map([]int{1, 2, 3}, func(v int) int {
		return v * 2
	})

	fmt.Println(doubled)
	fmt.Printf("%T", doubled)
}

/*
타입 제한자는 매개변수 타입으로 설정해줄 수 없다. 타입 제한자는  Type Parameter로 설정해줘야 한다.
여기서 그냥 Interface는 매개변수로도 가능하고, Type Parameter로는 가능하지 않다.


그러면 타입 제한과 인테페이스를 융합할 수 있을까? 가능하다

type Stringer interface {
	~int8 | ~int16 | ~int32 | ~int64 | ~int
	String() string
}

그러면 위놈은 인터페이스 일까? 타입제한자 일까?
타입제한자 이다. 왜냐하면 타입제한자는 함수의 매개변수 설정을 하지 못하기 때문이다.

그래서 이것을 이용해서 별칭을 하나 만들고
그 별칭의 리시버를 만든 뒤

제네릭 함수에 대입하여
타입의 자유에 벗어날 수 있고
interface의 기능도 유지할 수 있다.


Example)

package main

import (
	"fmt"
	"hash/fnv"
)

type ComparableHasher interface {
	comparable // ==, !=
	Hash() uint32
}

type MyString string

func (s MyString) Hash() uint32 {
	h := fnv.New32a()
	h.Write([]byte(s))
	return h.Sum32()
}

func Eqaul[T ComparableHasher](a, b T) bool {
	if a == b || a.Hash() == b.Hash() {
		return true
	}
	return false
}

func main() {
	var s1 MyString = "Hello"
	var s2 MyString = "World"
	fmt.Println(Eqaul(s1, s2))
}

Generic을 사용하면 코드를 줄일 수 있지만, 많이 사용하면 가독성이 줄어든다.
예를 들면, math.Abs, math.Sin 같은 메서드에서 사용하면 Generic을 탁월한 효과를 낼 수 있다.

그러므로 많은 타입을 사용하는 함수를 제작해야 한다면 Genenric을 고려해야 한다. 



*/

```





# 타입 연사자와 interface를 이용하여 마법부리기

# Generic을 언제 사용해야 하나?

 