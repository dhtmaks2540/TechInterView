# Java Virtual Machine

## JVM이란?

JVM이란 JAVA Virtual Machine, 자바 가상 머신의 약자를 따서 줄여 부르는 용어이다 (가상머신이란 프로그램의 실행하기 위해 물리적 머신과 유사한 머신을 소프트웨어로 구현한 것이다.) JVM 역할은 자바 애플리케이션을 클래스 로더를 통해 읽어 들여 자바 API와 함께 실행하는 것이다. 그리고 JVM은 JAVA와  OS사이에서 중개자 역할을 수행하여 JAVA가 OS에 구애받지 않고 재사용을 가능하게 해준다. 그리고 가장 중요한 메모리관리, Garbage collection을 수행한다. 그리고 JVM은 스**택기반의 가상머신**이다. ARM 아키텍쳐 같은 하드웨어는 레지스터 기반으로 동작하는데 비해 JVM은 스택기반으로 동작한다.

<br/>

## 왜 자바 가상머신을 알아야 하는가?

한정된 메모리를 효율적으로 사용하여 최고의 성능을 내기 위해서가 그 답이 될지 모르겠다. 메모리 효율성을 위해 메모리 구조를 알아야 하기 때문이다. 동일한 기능의 프로그램이더라도 메모리 관리에 따라 성능이 좌우된다. 메모리 관리가 되지 않은 경우 속도저하 현상이나 튕김 현상 등이 일어날 수 있다.  그래서, 알아두면 좋다를 넘어서 알아야 하는 것이다.

<br/>

## 자바프로그램 실행과정

1. 프로그램이 실행되면 JVM은 OS로부터 이 프로그램이 필요로 하는 메모리를 할당받는다.
   JVM은 이 메모리를 용도에 따라 여러 영역으로 나누어 관리한다.
2. 자바 컴파일러(javac)가 자바 소스코드(.java)를 읽어들여 자바 바이트코드(.class)로 변환시킨다.
3. Class Loader를 통해 class파일들을 JVM으로 로딩한다.
4. 로딩된 class파일들은 Execution engine을 통해 해석된다.
5. 해석된 바이트코드는 Runtime Data Areas 에 배치되어 실질적인 수행이 이루어지게 된다.
이러한 실행과정 속에서 JVM은 필요에 따라 Thread Synchronization과 GC같은 관리작업을 수행한다.

<br/>

#### JVM

![](https://t1.daumcdn.net/cfile/tistory/25616D45576B854C3F)

<br/>

## JVM의 구성

### Class Loader(클래스 로더)

JVM내로 클래스(.class파일)를 로드하고, 링크를 통해 배치하는 작업을 수행하는 모듈이다. Runtime 시에 동적으로 클래스를 로드한다. jar파일 내 저장된 클래스들을 JVM위에 탑재하고 사용하지 않는 클래스들은 메모리에서 삭제한다. (컴파일러 역할) 자바는 동적코드, 컴파일 타임이 아니라 런타임에 참조한다. 즉, 클래스를 처음으로 참조할 때, 해당 클래스를 로드하고 링크한다는 것이다.  그 역할을 클래스 로더가 수행한다.

<br/>

### Execution Engine(실행 엔진)

클래스를 실행시키는 역할이다. 클래스 로더가 JVM내의 런타임 데이터 영역에 바이트 코드를 배치시키고, 이것은 실행엔진에 의해 실행된다. 자바 바이트코드는 기계가 바로 수행할 수 있는 언어보다는 비교적 인간이 보기 편한 형태로 기술된 것이다. 그래서 실행 엔진은 이와 같은 바이트코드를 실제로 JVM내부에서 기계가 실행할 수 있는 형태로 변경한다. 이 때 두 가지 방식을 사용하게 된다.

* Interpreter(인터프리터) : 실행 엔진은 자바 바이트 코드를 명령어 단위로 읽어서 실행한다. 하지만 이 방식은 인터프리터 언어의 단점을 그대로 갖고 있다. 한 줄 씩 수행하기 때문에 느리다는 것이다.

* JIT(Just-In-Time) : 인터프리터 방식의 단점을 보완하기 위해 도입된 JIT 컴파일러이다. 인터프리터 방식으로 실행하다가 적절한 시점에 바이트코드 전체를 컴파일하여 네이티브 코드로 변경하고, 이후에는 해당 더 이상 인터프리팅 하지 않고 네이티브 코드로 직접 실행하는 방식이다. 네이티브 코드는 캐시에 보관하기 때문에 한 번 컴파일된 코드는 빠르게 수행하게 된다. 물론 JIT컴파일러가 컴파일하는 과정은 바이트코드를 인터프리팅하는 것보다 훨씬 오래걸리므로 한 번만 실행되는 코드라면 컴파일하지 않고 인터프리팅하는 것이 유리하다. 따라서 JIT 컴파일러를 사용하는 JVM들은 내부적으로 해당 메서드가 얼마나 자주 수행되는지 체크하고, 일정 정도를 넘을 때에만 컴파일을 수행한다. 

<br/>

### Garbage collector

GC를 수행하는 모듈(쓰레드)이 있다.

<br/>

## Runtime Data Area

프로그램을 수행하기 위해 OS에서 할당받은 메모리 공간

![](https://t1.daumcdn.net/cfile/tistory/275A103F576B85550D)

<br/>

### PC Register

Thread 가 시작될 때 생성되며 생성될 때마다 생성되는 공간으로 스레드마다 하나씩 존재한다. Thread가 어떤 부분을 어떤 명령으로 실행해야할 지에 대한 기록을 하는 부분으로 현재 수행 중인 JVM 명령의 주소를 갖는다.

<br/>

### JVM 스택 영역

프로그램 실행과정에서 임시로 할당되었다가 메소드를 빠져나가면 바로 소멸되는 특성의 데이터를 저장하기 위한 영역이다. 각종 형태의 변수나 임시 데이터, 스레드나 메소드의 정보를 저장한다. 메소드 호출 시마다 각각의 스택 프레임(그 메서드만을 위한 공간)이 생성된다. 메서드 수행이 끝나면 프레임 별로 삭제를 한다. 메소드 안에서 사용되는 값들(local variable)을 저장한다. 또 호출된 메소드의 매개변수, 지역변수, 리턴 값 및 연산 시 일어나는 값들을 임시로 저장한다.

<br/>

### Native method stack

자바 프로그램이 컴파일되어 생성되는 바이트 코드가 아닌 실제 실행할 수 있는 기계어로 작성된 프로그램을 실행시키는 영역이다. JAVA가 아닌 다른 언어로 작성된 코드를 위한 공간이다. JAVA Native Interface를 통해 바이트 코드로 전환하여 저장하게 된다. 일반 프로그램처럼 커널이 스택을 잡아 독자적으로 프로그램을 실행시키는 영역이다. 이 부분을 통해 C code를 실행시켜 Kernel에 접근할 수 있다.

<br/>

### Method Area(= Class area = Static area)

클래스 정보를 처음 메모리 공간에 올릴 때 **초기화되는 대상을 저장하기 위한 메모리 공간**. 올라가게 되는 메소드의 바이트 코드는 프로그램의 흐름을 구성하는 바이트 코드이다. 자바 프로그램은 main 메소드의 호출에서부터 계속된 메소드의 호출로 흐름을 이어가기 때문이다. 대부분 인스턴스의 생성도 메소드 내에서 명령하고 호출한다. 사실상 컴파일 된 바이트코드의 대부분이 메소드 바이트코드이기 때문에 거의 모든 바이트코드가 올라간다고 봐도 상관없다. 이 공간에는 **Runtime Constant Pool**이라는 별도의 관리 영역도 함께 존재한다. 이는 상수 자료형을 저장하여 참조하고 중복을 막는 역할을 수행한다.

<br/>

**올라가는 정보의 종류**

1. Field Information :
멤버변수의 이름, 데이터 타입, 접근 제어자에 대한 정보
2. Method Information :
메소드의 이름, 리턴타입, 매개변수, 접근제어자에 대한 정보
3. Type Information : 
class인지 interface인지의 여부 저장 /Type의 속성, 전체 이름, super class의 전체 이름(interface 이거나 object인 경우 제외)

<br/>

### Heap(힙 영역)

객체를 저장하는 가상 메모리 공간이다. new연산자로 생성된 객체와 배열을 저장한다. 물론 class area영역에 올라온 클래스들만 객체로 생성할 수 있다. 힙은 세 부분으로 나눌 수 있다.

![](https://t1.daumcdn.net/cfile/tistory/266E283B576B8E060B)

<br/>

#### Permanent Generation

생성된 객체들의 정보의 주소값이 저장된 공간이다. Class loader에 의해 load되는 Class, Method 등에 대한 Meta 정보가 저장되는 영역이고 JVM에 의해 사용된다. Reflection을 사용하여 동적으로 클래스가 로딩되는 경우에 사용된다. 내부적으로 Reflection 기능을 자주 사용하는 Spring Framework를 이용할 경우 이영역에 대한 고려가 필요하다.

<br/>

#### New/Young 영역

* Eden : 객체들이 최초로 생성되는 공간
* Survivor 0 / 1 : Eden에서 참조되는 객체들이 저장되는 공간

<br/>

#### Old 영역

New area에서 일정 시간 참조되고 있는, 살아남은 객체들이 저장되는 공간 Eden영역에 객체가 가득차게 되면 첫번째 GC(minor GC)가 발생한다. Eden영역에 있는 값들을 Survivor 1 영역에 복사하고 이 영역을 제외한 나머지 영역의 객체를 삭제한다.

인스턴스는 소멸 방법과 소멸 시점이 지역 변수와는 다르기에 힙이라는 별도의 영역에 할당된다. 자바 가상 머신은 매우 합리적으로 인스턴스를 소멸시킨다. 더이상 인스턴스의 존재 이유가 없을 때 소멸시킨다. 

<br/>

# Garbage Collection

#### 가비지 컬렉션, GC(Garbage Collection)

![](https://t1.daumcdn.net/cfile/tistory/26765B4258C27F891A)

<br/>

## Minor GC

새로 생성된 대부분의 객체(Instance)는 Eden 영역에 위치한다. Eden영역에서 GC가 한 번 발생한 후 살아남은 객체는 Survivor 영역 중 하나로 이동된다. 이 과정을 반복하다가 계속해서 살아남아 있는 객체는 일정시간 참조되고 있다는 뜻이므로 Old영역으로 이동시킨다.

<br/>

## Major GC

Old영역에 있는 모든 객체들을 검사하여 참조되지 않은 객체들을 한꺼번에 삭제한다. 시간이 오래 걸리고 실행 중 프로세스가 정지된다. 이것을 `stop-the-world`라고 하는데 Major GC가 발생하면 GC를 실행하는 스레드를 제외한 나머지 스레드는 모두 작업을 멈춘다. GC 작업을 완료한 이후에야 중단했던 작업을 다시 시작한다.

<br/>

## 가비지 컬렉션은 어떤 원리로 소멸시킬 대상을 선정하는가?

알고리즘에 따라 동작 방식이 매우 다양하지만 공통적인 원리가 있다. Gargabe Collector는 힙 내의 객체 중에서 가비지(Garbage)를 찾아내고 찾아낸 가비지를 처리해서 힙의 메모리를 회수한다. 참조되고 있지 않은 객체(Instance)를 가비지라고 하며 객체가 가비지인지 아닌지 판단하기 위해서 reachability라는 개념을 사용한다. 어떤 힙 영역에 할당된 객체가 유효한 참조가 있으면 reachability, 없다면 unreachability로 판단한다. 하나의 객체는 다른 객체를 참조하고, 다른 객체는 또 다른 객체를 참조할 수 있기 때문에 참조 사슬이 형성이 되는데, 이 참조 사슬 중 최초에 참조한 것을 Root Set이라고 칭한다. 힙 영역에 있는 객체들은 총 4가지 경우에 대한 참조를 하게 된다.

![](https://t1.daumcdn.net/cfile/tistory/27134A4D576BD1F526)

1= 힙 내의 다른 객체에 의한 참조

2= Java스택, 즉 Java 메서드 실행 시에 사용하는 지역변수와 파라미터들에 의한 참조

3= 네이티브 스택(JNI, Java Native Interface)에 의해 생성된 객체에 대한 참조

4= 메서드 영역의 정적 변수에 의한 참조

2,3,4 는 Root set이다.

즉 참조 사슬 중 최초에 참조한 것이다.

인스턴스가 가비지 컬렉션의 대상이 되었다고 해서 바로 소멸이 되는 것은 아니다. 빈번한 가비지 컬렉션의 실행은 시스템에 부담이 될 수 있기에 성능에 영향을 미치지 않도록 가비지 컬렉션 실행 타이밍은 별도의 알고리즘을 기반으로 계산이 되며, 이 계산결과를 기반으로 가비지 컬렉션이 수행된다.

<br/>

* Serial GC : 적은 메모리와 CPU 코어 개수가 적을 때 적합한 방식
* Parallel GC :
기본적인 GC 알고리즘은 Serial GC와 동일하지만 Parallel GC는 GC를 처리하는 스레드가 여러 개라서 보다 빠른 GC를 수행할 수 있다. 메모리가 충분하고 코어의 개수가 많을 때 유리하다.
* Parallel Old GC(Parallel Compacting GC)
JDK 5 update 6부터 제공한 GC방식이다. 별도로 살아있는 객체를 식별한다는 부분에서 보다 복잡한 단계로 수행된다.
* Concurrent Mark & Sweep GC(이하 CMS)
* G1(Garbage First) GC


<br/>

**참조**
* [원본링크1](https://asfirstalways.tistory.com/158)
* [원본링크2](https://asfirstalways.tistory.com/159)
* [참조](https://d2.naver.com/helloworld/1329)