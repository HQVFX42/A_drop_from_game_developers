# Contents
- [OOP(Object-Oriented Programming)](#oop)
- [static](#static)
- [interface](#interface)
- [auto](#auto)
- [Class](#class)
    - [Constructor & Destructor](#constructor--destructor)
- [동적할당](#동적할당)
- [Pointer](#pointer)
- [Reference vs Copy](#reference-vs-copy)
- [Type Casting](#Type-Casting)
- [메모리](#메모리-관리)

---

## **1. 구조체(Struct)와 클래스(Class)의 차이**
### **기본 접근 제한 지정자**
- **구조체(Struct)**:
  - 기본 접근 제한자가 `public`입니다.
  - 모든 멤버 변수와 함수는 외부에서 바로 접근 가능합니다.

```cpp
struct Person
{
    int Age;       // public
    float Height;  // public
};
```

- **클래스(Class)**:
  - 기본 접근 제한자가 `private`입니다.
  - 멤버 변수는 외부에서 바로 접근할 수 없으며, `public`으로 명시적으로 선언해야 접근 가능합니다.

```cpp
class Person
{
    int Age;       // private
    float Height;  // private

public:
    char Name[26]; // public
};
```

### **실무적 활용**
- 구조체는 단순히 데이터를 묶어 관리할 때 사용합니다.
- 클래스는 데이터와 기능(메서드)을 함께 캡슐화하여 객체 지향 프로그래밍(OOP)을 구현할 때 사용합니다.

## **2. 멤버 함수(Member Function)**
### **멤버 함수의 동작 원리**
- 클래스 내부의 멤버 함수는 호출 시 자동으로 `this` 포인터를 전달받습니다.
- `this` 포인터는 현재 객체의 주소를 가리키며, 이를 통해 객체의 멤버 변수에 접근합니다.

```cpp
class Person
{
public:
    void AddAge()
    {
        this->Age++; // this 포인터를 통해 Age에 접근 (암시적)
    }
};
```

### **`this` 포인터의 특징**
- `this`는 암묵적으로 전달되며, 현재 호출된 객체를 가리킵니다.
- `const` 멤버 함수에서는 `this`가 읽기 전용(`const`)으로 전달됩니다.

```cpp
void PrintInfo() const
{
    // this->Age 수정 불가 (읽기 전용)
}
```

## **3. 데이터 캡슐화와 접근 제한**
### **캡슐화의 필요성**
- 중요한 데이터(예: 게임 캐릭터의 이름, 계좌 정보 등)를 보호하기 위해 `private`로 설정하여 외부에서 직접 변경하지 못하도록 합니다.

```cpp
class BankAccount
{
private:
    FString Owner;   // 외부에서 접근 불가
    int32 Balance;

public:
    void Deposit(int32 Money)
    {
        Balance += Money;   // 안전한 자금 관리
    }
};
```

### **접근 제한 지정자 사용법**
- `public`: 외부에서 자유롭게 접근 가능.
- `private`: 외부에서 직접 접근 불가. 내부 함수로만 접근 가능.
- `protected`: 상속받은 클래스에서만 접근 가능.

## **4. 구조체와 클래스의 메모리 할당**

### **구조체 메모리**
- 구조체는 멤버 변수들이 순서대로 메모리에 배치됩니다.

```cpp
struct Person
{
    int Age;
    float Height;
    char Name[26];
};
```

메모리 배치:
```
[Age][Height][Name[0]~Name[25]]
```

### **클래스 메모리**
- 클래스도 구조체와 동일한 방식으로 메모리에 배치되지만, 접근 제한자는 런타임 동작에 영향을 미치지 않습니다.

## **5. 함수 인자로 객체 전달하기**

### **값 전달 방식**
- 객체를 값으로 전달하면, 복사본이 생성되어 원본 데이터는 변경되지 않습니다.

```cpp
void ModifyPerson(Person P)
{
    P.Age = 30; // 복사본만 변경됨
}
```

### **참조 전달 방식**
- 참조나 포인터를 사용하면 원본 데이터를 직접 수정할 수 있습니다.

```cpp
void ModifyPerson(Person& P)
{
    P.Age = 30; // 원본이 변경됨
}
```

포인터 사용 예:

```cpp
void ModifyPerson(Person* P)
{
    P->Age = 30; // 원본이 변경됨
}
```

## **6. 클래스 내부의 멤버 함수 활용**

#### **멤버 함수 구현**
- 클래스 내부에 멤버 함수를 정의하면, 해당 객체의 데이터를 더 쉽게 관리할 수 있습니다.

```cpp
class Person
{
public:
    void AddAge()
    {
        Age++;   // 나이를 증가시키는 함수
    }
};
```

### **객체별 동작 관리**
- 각 객체는 독립적인 데이터를 가지며, 동일한 멤버 함수를 호출해도 각자의 데이터만 변경됩니다.

```cpp
Person John, Jane;
John.AddAge(); // John의 나이만 증가
Jane.AddAge(); // Jane의 나이만 증가
```

## **7. 디스 포인터(this Pointer)와 const**

### **디스 포인터란?**
- 디스 포인터(`this`)는 현재 호출된 객체의 주소를 가리키며, 멤버 함수 내부에서 암시적으로 사용됩니다.

### **const 멤버 함수와 디스 포인터**
- `const` 멤버 함수에서는 디스 포인터가 읽기 전용(`const`)으로 전달됩니다.

```cpp
void PrintInfo() const
{
    // this->Age 수정 불가 (읽기 전용)
}
```

디스 포인터 선언 예제:

```cpp
Person* const this; // 디스 포인터는 상수 포인터로 선언됨 (주소 변경 불가)
```

## **8. 팁 및 주의사항**
1. **캡슐화**: 중요한 데이터는 반드시 `private`로 설정하고, 필요한 경우에만 `getter/setter`를 제공합니다.
2. **참조와 포인터**: 성능 최적화를 위해 큰 객체를 함수 인자로 전달할 때 참조 또는 포인터를 사용하는 것이 좋습니다.
3. **코드 가독성**: 명확한 변수명과 주석을 통해 가독성을 높이고, 불필요한 복잡성을 줄입니다.

---

## OOP
### 절차적 프로그래밍?
- 프로그램 설계 시 기능 구현을 위해 프로시저(함수)를 중점으로 사용하여 구조/로직을 설계하는 방법

### 객체지향(OOP)?
- 프로그램 설계 시 프로그램을 수많은 객체로 나누고 이 객체들의 상호작용으로 설계하는 방법

### 장점
- 코드 재사용이 용이하다, 객체를 만들어두면 재사용할 수 있다
- 클래스 기능별로 분할하였기 때문에 모듈화도 용이하다
- 상속을 통해 높은 확장성을 가질 수 있다
- 즉, 유지보수성이 뛰어나다

### 주요 특성
- Inheritance (상속성)
    - 자식 클래스가 부모 클래스의 변수, 함수를 물려받는 것
    - 자식 클래스는 자식만의 특성을 오버라이딩을 통해 구현한다
    - Why?
        - 다형성과도 연관되는 질문으로, 부모와 자식을 다형성으로 엮어놓으면 부모에서 추상적인 형태를 주고,  
        자식에서 세밀화하여 유연하게 코드를 작성할 수 있으며 생상성, 유지보수성 또한 올라간다 (무분별한 복붙을 막을 수 있는 효과)
        - 추후 기능개선 시 `교체`가 가능하다
        - 복붙으로 이루어진 코드들은 제각기 다른 코드이므로 개선이 필요할 때 구현을 따로따로 해줘야 하므로 유지보수가 어렵다
    ```cpp
    class Player
    {

    };

    class Socereress : public Player
    {

    };

    class Paladin : public Player
    {

    };

    Socereress sorceress;
	Player* p1 = &sorceress;
	Paladin paladin;
	Player* p2 = &paladin;
	// 포인터로 담지 않으면 자식클래스의 더 많은 정보가
	// 부모로 이동할 시 손실이 일어날 수 있다

	Socereress* sorceressPtr = (Socereress*)p1;
	// 포인트로 저장하면 데이터가 손실되는 것이 아니라
	// Casting을 통해 원복이 가능하다
    ```

- PolyMorphism (다형성)
    - 겉은 동일하지만 상황에 따라 다르게 다양한 형태를 가지는 특성
    - 객체지향에서 다형성은 여러가지 형태를 가질 수 있는 능력을 의미
    - CPP에서의 다형성은 상속에 의한 다형성, Overloading, Overriding, Template에 의한 다형성 등으로 구현된다
    - `Overloading?`
        - 같은 이름의 함수라도 Parameter 타입이 다르거나, 개수가 다르면 다른 함수로 정의하여 사용할 수 있음
        - return 타입은 Overloading과 상관이 없다. return 타입으로는 함수를 구분할 수 없기 때문
    - `Overriding?`
        - 상속관계에서 부모의 메서드를 자식이 재정의해서 사용하는 것
    - `Template?`
        - 클래스, 함수를 타입에 독립적이게 만드는 도구
            - 템플릿을 사용하면 여러 타입에 대응되는 단 하나의 객체나 함수를 만들 수 있다
            - cpp의 기능으로, 함수와 클래스가 제네릭형과 동작할 수 있게 도와준다
            - 함수나 클래스가 개별적으로 다시 작성하지 않고도 각기 다른 수많은 자료형에서 동작할 수 있게 한다
        - 템플릿의 타입은 컴파일 타임에 결정되어 인스턴스화 하기 때문에  
        컴파일 타임에 컴파일러가 실제 코드 구현부를 모두 볼 수 있어야 한다
        - 인스턴스화는 컴파일러가 컴파일 시에 요구되는 타입으로 클래스 정의 코드를 생성해내는 것
            - 즉, 타입을 지정하여 그 타입을 사용하는 클래스를 만들어 냄
            - ex) 만약 float 타입으로 템플릿 클래스를 사용했다면 컴파일 타임에 컴파일러가 float 타입의 클래스를 만들어 냄
            - 그렇기 때문에 템플릿 클래스의 함수 구현은 헤더파일에 들어가야 한다
            - 혹은 헤더에서 템플릿 정의부분 아래에 cpp파일을 include하는 방법도 있지만 cpp파일을 빌드 리스트에 넣지 말아야 한다
    ```cpp
    class Pawn
    {
    public:
        virtual void Move() { std::cout << "Pawn Move" << std::endl; }
    };

    class Character : public Pawn
    {
    public:
        // 동적 바인딩 (함수 호출이 런타임에 결점 됌 ex) 가상함수, 오버라이딩
        virtual void Move() override { std::cout << "Character Move" << std::endl; }
    };

    // 정적 바인딩 (함수 호출이 컴파일 시점에 결정 됌 ex) 함수 오버로딩, 연산자 오버로딩
    void MovePawn(Pawn* p) { p->Move(); }

    int main()
    {
        Character c;
        MovePawn(&c);   // Character Move 출력
                        // 오버라이딩 하지 않을 시(virtual 미사용) Pawn Move 출력
    }
    ```

- Encapsulation (캡슐화, 은닉성)
    - 프로그램의 세부구현을 감추는 것
    - 접근지정자 : private을 통해 변수와 객체를 감추고, public으로 선언된 메소드로만 접근을 허용하여 제한하는 것
    - 상속접근지정자 : 다음 세대한테 어떻게 물려줄지를 정함
    - Why?
        - 의도하지 않은 접근을 제한하기 위해 (= 설계한 클래스를 다른 사람이, 혹은 본인이 잘못 쓰는 것을 방지)
        - 이러한 코드 사용자는 프로그램의 내부 구현에 신경 쓸 필요없이 메소드를 통해 원하는 기능을 호출
    - 객체들끼리 서로의 구현과 상태를 상관하지 않고 단지 사용하게만 함으로써 의존성을 낮추는 역할을 한다
    ```cpp
    // private 멤버변수를 public 멤버함수(getter, setter)를 통해 접근
    class Weapon : public Item
    {
    public:
        Weapon();
        virtual ~Weapon();

        virtual void PrintItemInfo() override;

        void SetAttackPower(int attackPower)
        {
            _attackPower = attackPower;
        }
        int GetAttackPower() const
        {
            return _attackPower;
        }

    private:
        int _attackPower = 0;
        int _durability = 0;
    };
    ```

## explicit
- 암시적으로 변환이 되는건을 방지하기 위해 생성자나 변환 연산자 앞에 사용

## static
- 특정 객체와 무관하게 만드는 키워드
    - 즉, 글로벌하게 사용가능해지고 static 함수 선언시 일반 멤버 변수는 건드릴 수 없다
```cpp
class UserManager
{
public:
	static UserManager* GetInstance()
	{
		static UserManager instance;
		return &instance;
	}

	void AddUser()
	{
		userCount++;
		std::cout << "User added. Total users: " << userCount << "\n";
	}
	void RemoveUser()
	{
		if (userCount > 0)
		{
			userCount--;
			std::cout << "User removed. Total users: " << userCount << "\n";
		}
		else
		{
			std::cout << "No users to remove.\n";
		}
	}

private:
	int userCount = 0;
};
```

## interface
**특징과 활용**
- 순수 가상 함수로만 구성
- 클래스의 뼈대 역할 수행
- 다중 상속 가능

**구현 방법**
- virtual 키워드와 =0 사용
- 모든 멤버 함수는 public 및 virtual로 선언
- 구현 클래스에서 모든 함수 오버라이딩 필수
```cpp
class Player
{

};

class IFly	// abstract class
{
public:
	virtual void Fly() = 0;	// pure virtual function
};

class Assassin : public Player, public IFly
{
public:
	virtual void Fly() override
	{
		std::cout << "Assassin flies!\n";
	}
};

void FlyTest(IFly* fly)
{
	//fly->Fly();
}

int main()
{
	Assassin assassin;
	FlyTest(&assassin);	// 인터페이스 클래스 다중 상속으로 casting 가능
	//assassin.Fly();
}
```

## auto
**기본 특징**
- C++11에서 도입된 자동 타입 추론 키워드
- 선언과 동시에 반드시 초기화가 필요
- 컴파일러가 초기화 값을 기반으로 타입을 자동으로 결정

**주의사항**
- 초기화 없이 선언만 하는 것은 불가능
- 한번 타입이 결정되면 다른 타입으로 변경 불가능
- 코드의 가독성을 위해 명확한 상황에서만 사용 권장

## Class

### Class?
- CPP에서 객체를 구현하기 위해 사용하는 방법으로 변수와 메서드를 가진 사용자 정의 자료형
- 객체의 상태/특성을 정의하는 일종의 설계도이며, 객체화했을때(인스턴스) 객체가 된다
- 정적/동적 할당으로 실제 메모리에 해당 자료형으로 만든 영역이 할당되었을 때 하나하나를 객체(인스턴스)라고 한다

### Instance?
- 클래스를 기반으로 만들어져 실제로 메모리에 할당된 것

### Byte Padding
- 구조체나 클래스를 실제로 메모리에 올릴 때, 컴파일러가 성능 향상을 위해 추가적인 메모리를 할당하여 끼워 넣는 것
- 내부에 선언된 가장 큰 자료형의 크기로 정해진다
    - 만약 구조체 안에 구조체가 들어있다면, 해당 구조체 안에서 가장 큰 자료형의 크기로 정해진다
    ```cpp
    struct Coord // 8byte
    {
        short U;
        int V;
        // short(2byte) + 2byte / int(4byte)
    }

    struct Texture // 16byte
    {
        char Name;
        short colour;
        Coord coord;
    }
    /**
     * Coord의 제일 큰 타입이 int이므로, 4byte 기준으로 패딩
     * char + short + 1byte / short + 2byte / coord(8byte)
     */
    ```
- 소켓 프로그래밍 중 패킷 구조체를 구현할 때 #pragma pack(push, 1)을 쓰는 이유도 패딩에 있다
- 패킷을 받은 쪽은 패킷을 읽어서 캐스틍일 통해 값을 복구해야 하는데,  
패딩이 들어가 있으면 잘못된 크기를 가지고 캐스팅을 하게 되므로 잘못된 값이 복구되기 때문

### Constructor & Destructor
- `Constructor`?
- `Destructor`?
    - 소멸자에 virtual 키워드를 사용해야 하는 이유는, 부모 클래스 포인터로 자식 클래스 객체를 삭제할 때 자식 클래스의 소멸자가 제대로 호출되도록 하기 위해서입니다.
    **부모 클래스의 소멸자가 virtual이 아니면, 부모 클래스 포인터로 자식 객체를 delete할 때 부모 소멸자만 호출되고 자식 소멸자는 호출되지 않습니다.** 
    이 경우 자식 클래스가 동적으로 할당한 리소스(메모리 등)가 해제되지 않아 메모리 누수와 예기치 않은 동작이 발생할 수 있습니다.  

    - 소멸자를 virtual로 선언하면, **실제 객체 타입에 맞는 소멸자(자식 → 부모 순서)**가 호출됩니다.  
    즉, 자식 소멸자가 먼저 실행되고 그 다음 부모 소멸자가 실행되어 정확하고 안전하게 자원이 해제됩니다
    ```cpp
    class Player
    {
    public:
        Player()
        {
            std::cout << "Player()" << std::endl;
        }
        virtual ~Player()
        {
            std::cout << "~Player()" << std::endl;
        }
    };

    class Pet
    {
    public:
        Pet()
        {
            std::cout << "Pet()" << std::endl;
        }
        virtual ~Pet()
        {
            std::cout << "~Pet()" << std::endl;
        }
    };

    class Assassin : public Player
    {
    public:
        Assassin()
        {
            _pet = new Pet();
            std::cout << "Assassin()" << std::endl;
        }
        virtual ~Assassin()
        {
            std::cout << "~Assassin()" << std::endl;
            delete _pet;
        }

    private:
        Pet* _pet;
        //Pet _pet2;	// 포인터 타입이 아니면 자동으로 생성자 호출
    };

    int main()
    {
        Player* player = new Assassin();

        delete player;
    }
    ```

- Exapmle
```cpp
    using namespace std;

    struct A
    {
	    int n;
	    A(int n = 1) : n(n) { cout << "Constructor" << '\n'; }
	    ~A() { cout << "Destructor" << '\n'; }
	    A(const A & a) : n(a.n) { cout << "Copy" << '\n'; } // user-defined copy constructor
	    A(A &&) { cout << "Move" << '\n'; }
    };

    struct B : A
    {
	    // implicit default constructor B::B()
	    // implicit copy constructor B::B(const B&)
    };

    A F1(A a)
    {
	    return a;
    }

    A F2(A & a)
    {
	    return a;
    }

    int main()
    {
	    A a1(7);
	    A a2(a1); // calls the copy constructor
	    A a3 = F1(a2);
	    A a4 = F2(a3);

	    cout << "---" << '\n';

	    B b;
	    B b2 = b;
	    A a5 = b; // conversion to A& and copy constructor

	    cout << "---" << '\n';

	    return 0;
    }
```

<br> 위 코드의 출력 값은 아래와 같다

```cpp
    Constructor // A a1(7);에서 a1 객체 생성
    Copy        // A a2(a1);에서 a1을 a2로 복사
    Copy        // F1(a2)에서 a2를 함수 매개변수로 복사
    Move        // F1의 반환값을 a3로 이동
    Destructor  // F1 함수의 지역 변수(매개변수) 소멸
    Copy        // F2(a3)의 반환값을 a4로 복사
    ---         // 구분선 출력
    Constructor // B b;에서 B의 기본 생성자 호출 (A의 생성자도 호출됨)
    Constructor // B b2 = b;에서 B의 암시적 복사 생성자 호출 (A의 생성자도 호출됨)
    Copy        // A a5 = b;에서 B를 A로 변환 후 복사
    ---         // 구분선 출력
    Destructor  // a5
    Destructor  // b2
    Destructor  // b
    Destructor  // a4
    Destructor  // a3
    Destructor  // a2
    Destructor  // a1
```

<br> 주요 포인트:
- F1은 값으로 전달받고 반환하므로, 복사 후 이동이 발생한다
- F2는 참조로 전달받지만 값으로 반환하므로, 반환 시 복사가 발생한다
- B 클래스는 A를 상속받아 암시적 생성자와 복사 생성자를 가진다
- B에서 A로의 변환 시 복사 생성자가 호출된다
- 소멸자는 생성된 객체의 역순으로 호출된다

### 동적할당
- 함수 malloc / free
- 연산자 new / delete
- 차이점은 malloc / free는 생성자 소멸자 호출이 안되고 new는 된다

## Pointer
- 메모리 주소를 저장하는 변수

### this pointer?
- this 포인터는 cpp에서 클래스의 멤버에 함수가 호출될 때, 해당 함수를 호출한 객체 자신을 가리키는 포인터

## Smart pointer
**Unique_ptr**
- 객체의 유일한 소유권 보장
- 자동 메모리 해제 기능
- 소유권 이전 시 MoveTemp() 사용

**Shared_ptr**
- 여러 포인터가 하나의 객체 공유 가능
- 참조 카운트 기반 자동 메모리 관리
- 스레드 안전성 보장

**Weak_ptr**

## Reference vs Copy
**Reference**
- 변수의 별칭(alias)으로 작동
- 직접 메모리 참조 방식 사용
- NULL 초기화 불가능

**Copy와의 차이점**
- 레퍼런스는 추가 메모리 공간을 소모하지 않음
- 복사자는 값 복사로 인한 메모리 소모 발생
- 함수 호출 시 레퍼런스 값 복사가 발생하지 않음

### 복사 생성자
- 복사 생성자는 자신과 같은 클래스 타입의 객체에 대한 참조를 인수로 받아서 자신을 초기화하는 생성자 

```cpp
// 복사 생성자는 클래스 객체를 인수로 받습니다. Pet(const Pet& other)

class Player
{
public:
	Player()
	{
		std::cout << "Player()" << std::endl;
	}
	virtual ~Player()
	{
		std::cout << "~Player()" << std::endl;
	}
};

class Pet
{
public:
	Pet()
	{
		std::cout << "Pet()" << std::endl;
	}
	virtual ~Pet()
	{
		std::cout << "~Pet()" << std::endl;
	}

	Pet(const Pet& other)
	{
		std::cout << "Pet(const Pet& other)" << std::endl;
	}
};

class Assassin : public Player
{
public:
	Assassin()
	{
		_pet = new Pet();
		std::cout << "Assassin()" << std::endl;
	}
	virtual ~Assassin()
	{
		std::cout << "~Assassin()" << std::endl;
		delete _pet;
	}
	Assassin(const Assassin& other)
	{
		std::cout << "Assassin(const Assassin& other)" << std::endl;
		_hp = other._hp;
		_pet = new Pet(*other._pet);	// deep copy
	}

public:
	int _hp = 100;

private:
	Pet* _pet;
	//Pet _pet2;	// 포인터 타입이 아니면 자동으로 생성자 호출
};

int main()
{
	Assassin a1;
	a1._hp = 200;

	std::cout << "-------------------------" << std::endl;

	//Assassin a2;	// 기본 생성자
	//a2 = a1;		// 복사 연산자

	//Assassin a3 = a1;	// shallow copy
	Assassin a3(a1);	// 복사 생성자
}
```

- 문제는 Assassin에서 Pet을 포인터로 들고 있을 때 발생하게 된다
    - 기존 얕은복사로 포인터만 복사되어 Pet을 가리키고 있는 하나의 주소 값을 여러명의 Assassing이 가지게 되는 현상이 발생
    - Assassin 소멸시 _pet을 delete하게 되면 같은 객체를 delete 하게 되어 크래쉬 발생
    - 이렇게 복사하는 것을 `얕은 복사`라고 한다.  
    값을 복사하는 것이 아니라 값을 가리키는 포인터를 복사하는 것  
    객체 대입을 얕은 복사로 진행하면 문제가 생길 수 있다  
    따라서 깊은 복사로 객체 대입을 진행해야 한다
- `깊은 복사`란? 값 자체를 복사하는 것을 말한다  
    - 클래스에서는 이런 깊은 복사를 가능케 하는 복사 생성자를 정의할 수 있다   

---

## Type Casting
- 형변환의 종류
    1. **static_cast**
    1. **dynamic_cast**
    1. const_cast
        - const_cast는 포인터나 참조의 상수성(constness)만 제거할 뿐, 원래 상수로 선언된 객체의 값을 안전하게 변경할 수 있다는 보장을 하지 않는다  
        - 예를 들어, const int a = 10;처럼 **진짜 상수(const로 선언된 변수)**에 대해 const_cast로 값을 변경하려 하면,  
        컴파일러가 a의 값을 코드 전체에서 상수로 취급해 최적화할 수 있으므로, 실제로 값을 변경해도 반영되지 않거나 정의되지 않은 동작(Undefined Behavior)이 발생할 수 있다
        - 반면, 비상수 변수(예: int x = 5;)를 const 포인터로 가리키고 const_cast로 상수성을 제거하면, 값을 정상적으로 변경할 수 있다
    1. reinterpret_cast
- Type casting example
```cpp
class Player
{
public:
	virtual ~Player() {}
};

class Assassin : public Player
{

};

class Paladin : public Player
{

};

class Wolf
{

};

int main()
{
	// 1. static_cast : 타입 원칙에 상식적인 형변환
	// int <-> float
	// Player* -> Assassin*

	int hp = 10;
	int maxHp = 100;

	//float hpPercent = (float)hp / maxHp;	// C-style cast
	float hpPercent = static_cast<float>(hp) / maxHp;

	Assassin* assassin = new Assassin();
	Player* player = assassin;

	//Assassin* assassin2 = (Assassin*)player;	// C-style cast
	Assassin* assassin2 = static_cast<Assassin*>(player);

	// 2. dynamic_cast : 런타임에 타입을 확인하는 형변환
	// 상속관계에서의 안전한 변환
	// 다형성을 활용하는 방식 (virtual)
	// RTTI (Run Time Type Information)
	Paladin* paladin = new Paladin();
	Player* player2 = paladin;

	Paladin* paladin2 = dynamic_cast<Paladin*>(player);
	if (!paladin2)
	{
		std::cout << "Player2 is not paladin" << std::endl;
	}

	// 3. const_cast : const 속성을 제거하는 형변환
	// const_cast는 주로 API에서 const 속성을 제거할 때 사용된다
	// 예를 들어, const char* -> char*로 변환할 때 사용된다
	const int a = 10;
	int* p = const_cast<int*>(&a);
	*p = 20;
	std::cout << a << std::endl;	// 10


	int x = 5;
	const int* p2 = &x;
	int* q = const_cast<int*>(p2);
	*q = 10; // 정상적으로 x의 값이 10으로 바뀜
	std::cout << x << std::endl; // 10

	// 4. reinterpret_cast : 포인터를 전혀 관계없는 포인터나 값으로 변환
	// 위험하고 강력한 형변환
	Wolf* wolf = reinterpret_cast<Wolf*>(player);
	__int64 wolfPtrAddress = reinterpret_cast<__int64>(wolf);
}
```

## STL 특징
- 템플릿 기반의 일반화된 알고리즘 제공
- 데이터 추상화와 코드 재사용성 향상

## 이터레이터 활용
- 컨테이너 요소에 대한 순차적 접근 제공
- begin()과 end() 함수로 범위 지정
- 포인터와 유사한 방식으로 요소 접근

# 메모리 관리

## 메모리 효율 파악과 최적화 방법

### 기본 원칙
- 불필요한 동적 할당 최소화
- 적절한 메모리 정렬 유지
- 캐시 친화적인 데이터 구조 설계

### 최적화 기법
- 메모리 풀링 활용
- 객체 재사용 패턴 적용
- 메모리 단편화 최소화

## 기본 컨테이너 사용시 주의사항

### vector
- 리사이즈 시 메모리 재할당 비용 고려
- reserve()를 통한 사전 메모리 할당
- shrink_to_fit()으로 불필요 메모리 해제

### list
- 노드 할당/해제 오버헤드 인지
- 캐시 지역성 저하 주의
- 반복자 무효화 상황 파악

### vector vs list
- 메모리 구조
    - 벡터: 연속적인 메모리 공간에 요소를 저장.

    - 리스트: 비연속적인 메모리 공간에 요소를 저장하며, 각 요소는 이전과 다음 요소를 가리키는 포인터를 가진다.

- 요소 접근
    - 벡터: 임의 접근이 가능하며, 인덱스를 통해 O(1) 시간에 요소에 접근할 수 있다.

    - 리스트: 임의 접근이 불가능하며, 요소에 접근하려면 순차적으로 탐색해야 한다. 이는 O(n) 시간이 소요된다.

- 삽입 및 삭제
    - 벡터: 끝에서의 삽입/삭제는 빠르지만(O(1)), 중간에서의 삽입/삭제는 느림(O(n))

    - 리스트: 어느 위치에서든 삽입/삭제가 빠름(O(1)). 단, 삽입/삭제 위치를 찾는 시간은 제외

- 메모리 사용
    - 벡터: 요소 자체의 크기만큼만 메모리를 사용

    - 리스트: 각 요소마다 추가적인 메모리(이전/다음 요소를 가리키는 포인터)를 사용

- 성능 특성
    - 벡터: 연속적인 메모리 구조로 인해 캐시 효율성이 높다. 읽기 작업에 유리.

    - 리스트: 잦은 삽입/삭제 작업에 유리.

- 사용 시나리오
    - 벡터: 요소의 빈번한 접근이 필요하거나, 주로 끝에서 삽입/삭제가 일어나는 경우에 적합.

    - 리스트: 빈번한 삽입/삭제 작업이 중간에서 일어나는 경우에 적합.

### map
- 레드-블랙 트리 구조의 오버헤드
- 잦은 검색 시 unordered_map 고려
- 키 비교 연산 최적화

## 람다와 람다 캡처 고려사항

### 람다 기본
- 캡처 방식에 따른 메모리 영향 이해
- 값 캡처와 참조 캡처의 수명 주기
- 캡처 목록 최소화

### 주의사항
- 참조 캡처 시 댕글링 레퍼런스 방지
- 캡처된 변수의 메모리 관리
- 람다 객체 크기 고려
