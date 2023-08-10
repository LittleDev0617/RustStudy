Rust 4시간 안에 끝내기  
C / C++ / Python 과 같은 언어들을 숙지하고 있는 경우 이 문서를 빠르게 훑어보며 Rust의 기본적인 문법과 특징을 습득할 수 있게 합니다.  
https://rinthel.github.io/rust-lang-book-ko/ch03-01-variables-and-mutability.html 를 참고하여 작성되었습니다.

<hr />

먼저 Rust 는 속도와 안정성을 동시에 가져갈 수 있는 언어입니다.  
또 high-level 언어처럼 인간 친화적인 문법을 사용하여 좋은 개발 경험을 선사합니다. 
Rust 를 통해 운영체제 개발이나 시스템 프로그래밍과 같은 저수준부터, 웹서버까지 개발할 수 있습니다.

# 설치
Windows - https://www.rust-lang.org/en-US/install.html 에서 rustup installer 설치  
Mac / Linux - curl https://sh.rustup.rs -sSf | sh  

<hr />

# 컴파일 및 빌드
위 설치를 마쳤다면, rustc 나 cargo 와 같은 프로그램이 설치되고 환경변수에 PATH 가 자동 등록되었을 겁니다.  
터미널에서 rustc 나 cargo 를 입력했을 때 정상 실행되는지 참고하세요.  

## 컴파일
gcc 와 같이 `rustc ./main.rs` 를 통해 컴파일할 수 있습니다.
소규모 단위의 프로젝트는 위와 같이해도 무방하지만, 앞으로 러스트 프로그램을 빌드할 때는 `cargo`를 사용합니다.  
cargo 는 rust 빌드를 도와주는 편한 도구입니다.  
`$ cargo new project_name --bin && cd project_name` 을 통해 프로젝트를 생성할 수 있습니다.  

- `cargo build` : 프로젝트를 빌드합니다.
- `cargo run`   : 소스코드가 변경되었다면 빌드 후 실행 / 아니면 그냥 실행
- `cargo check` : 컴파일 가능한지 확인합니다. (빌드 X)

cargo check 는 보통 컴파일 에러를 확인하는데 사용하며, 빌드하지 않기 때문에 속도가 빠릅니다.  

cargo build 를 하면 src/ 와 target/ 폴더가 생성됩니다.  
src/ 에 러스트 소스코드를 작성합니다.  

debug mode 로 빌드하거나 release mode 로 빌드할 수 있는데, 테스트 할 때는 debug 로 빌드한 후(`cargo build`)  
배포할 때 `cargo build --release` 를 통해 배포하세요.  
빌드 결과물은 각각 `target/debug/` 와 `target/release` 에 저장됩니다.  

<hr />

# 변수와 자료형
## 변수와 가변성
### let
변수는 let 을 통해 선언할 수 있습니다.  
let 으로만 선언된 변수는 기본적으로 불변성을 가집니다.  
```rust
fn main() {
    let x = 5;
    x = 6;
    println!("The value of x : {x}");
}
```
위 코드는 `re-assignment of immutable variable` 에러를 발생시킵니다.  

let 은 보통 다음과 같이 Shadowing 을 통해 사용합니다.
```rust
fn main() {
    let x = 5;

    let x = x + 1;

    let x = x * 2;

    println!("The value of x is: {}", x);
}
```
그러면 변수가 아니라 상수가 아니냐는 의문이 당연히 들게 됩니다.  
하지만 Rust 에서는 let 과 const 의 차이가 분명합니다.  
> let 으로 선언된 변수는 함수의 반환값이나 런타임 중 얻게 되는 값을 담을 수 있지만, const 는 그렇지 않다.
확실한건 const 는 컴파일 시 결정되는 값, let 은 컴파일 할 때와 런타임에 정해질 수 있는 값이라 생각하면 됩니다.  

### let mut
불변성을 가지지 않은 변수는 let mut 키워드를 통해 선언할 수 있습니다.
```rust
fn main() {
    let mut x = 5;
    x = 6;
    println!("The value of x : {x}");
}
```
위 코드는 에러를 발생시키지 않습니다.

### const
상수는 다른 언어처럼 const 키워드를 사용하여 선언합니다.
```rust
fn main() {
    const x: u32 = 5;
    println!("The value of x : {x}");
}
```

## 데이터 타입
Rust 에서 사용되는 모든 값들은 각각 타입을 갖습니다. 그렇기 때문에 어떤 타입인지 명시하여야합니다.  
Rust 는 타입이 고정된 언어입니다. 따라서 컴파일 시 반드시 변수들은 타입이 정해져 있어야합니다.  

### 스칼라 타입
#### 정수형
정수형 타입은 부호를 나타내는 i/u 접두사와  8, 16, 32, 64, size 접미사를 합하여 나타냅니다.  
size 는 cpu 가 32bit 면 32, 64bit 이면 64로 작동합니다.  
```rust
let x : i8 = 5; // 부호 있는, 8비트 정수형
let y : u32 = 45; // 부호 없는, 32비트 정수형
```

여러 진법으로 숫자를 나타낼 수 있습니다.  
```rust
// Decimal  / 10진수    / 1234
// Hex      / 16진수    / 0x1234
// Octal    / 8진수     / 0o12
// Binary   / 2진수     / 0b11110010
// Byte     / u8        / b'A'
```

_ 를 통해 숫자를 나타낼 수 있습니다. _ 는 그저 시각적인 구분입니다.  
```rust
let apple_price : u32 = 100_000;    // 100000 을 _ 를 통해 시각적으로 나타냅니다.
let one_byte : u8 = 0b0100_1001;
```

#### 부동 소수점
f32, f64 가 있습니다. 기본 타입은 f64 입니다.  

```rust
fn main() {
    let x = 2.0; // f64

    let y: f32 = 3.0; // f32
}
```

#### 연산

```rust
fn main() {
    // addition
    let sum = 5 + 10;

    // subtraction
    let difference = 95.5 - 4.3;

    // multiplication
    let product = 4 * 30;

    // division
    let quotient = 56.7 / 32.2;

    // remainder
    let remainder = 43 % 5;
}
```

#### Boolean 타입
true / false 두 값을 가지는 타입입니다.
```rust
fn main() {
    let t = true;

    let f: bool = false; // with explicit type annotation
}
```


#### 문자 타입
```rust
fn main() {
   let c = 'z';
   let z = 'ℤ';
   let heart_eyed_cat = '😻';
}
```
" 를 쓰는 문자열과 달리 ' 를 사용합니다. Rust 의 char 타입은 Unicode Scalar 를 표현하는 값입니다.

### 복합 타입

#### tuple 타입

다양한 타입의 원소들을 하나의 집합으로 사용합니다.  

```rust
fn main() {
    let tup = (500, 6.4, 1);

    let (x, y, z) = tup;    //  destructing 을 통해 x y z 에 값을 나눠 넣습니다.

    println!("The value of y is: {}", y);
}
```

마침표(.) 뒤에 인덱스를 넣어 원하는 값에 접근할 수 있습니다.  
```rust
fn main() {
    let x: (i32, f64, u8) = (500, 6.4, 1);

    let five_hundred = x.0;

    let six_point_four = x.1;

    let one = x.2;
}
```

#### 배열
배열은 같은 타입의 원소들만 허용합니다.  
Rust 의 배열은 고정된 길이를 갖습니다.  가변 길이의 배열은 표준 라이브러리의 벡터를 사용할 수 있습니다.  
차이점이라 한다면 배열은 stack 메모리 영역에, 벡터는 heap 에 할당됩니다.  
```rust
fn main() {
    let a = [1, 2, 3, 4, 5];

    let first = a[0];
    let second = a[1];
}
```

#### Out Of Bound - 유효하지 않은 배열 요소에 대한 접근
```rust
fn main() {
    let a = [1, 2, 3, 4, 5];
    let index = 10;

    let element = a[index];

    println!("The value of element is: {}", element);
}
```
컴파일 시에는 아무런 에러도 발생되지 않지만, 런타임 중에 에러가 발생하며 `panic` 됩니다.  
많은 저수준 언어에서는  OOB 검사를 하지 않고 수행하지만, Rust는 즉시 종료하여 오류로부터 사용자를 보호합니다.  
9장에서 오류 처리에 대해 자세히 설명합니다.  

<hr />

# 함수
함수는 앞/뒤 어디던 정의되어있으면 호출할 수 있습니다.
```rust
fn main() {
    another_function(5);
}

fn another_function(x: i32) {
    println!("The value of x is: {}", x);
}
```

Rust 에서는 매개변수의 타입을 `반드시` 명시해야 합니다.  


# 구문과 표현식
Rust 에서는 '구문' 과 '표현식'으로 코드를 나눌 수 있습니다.  
- 구문   : ; 으로 끝나는 명령문. 반환값이 없다.  
- 표현식 : ; 으로 끝나지 않고 {} 으로 감싸진 구문. 반환값은 {} 안의 내용이 된다.

예를 들어 다음 과 같은 코드가 성립합니다.  
```rust
let  x = {
    let y = 5;
    y * 2
};
```
중괄호로 감싼 부분이 `표현식` 이며 `;` 으로 끝나지 않은 y * 2 즉 10 이 반환값이 됩니다.  

## 반환 값을 갖는 함수
Rust 에서 `->` 뒤에 반환값의 타입을 명시해야 합니다.  
`return` 키워드를 통해 일찍 반환할 수 있지만, 대부분의 함수는 함수 본문의 마지막 표현식의 값을 반환합니다.  
```rust
fn five() -> i32 {
    5
}

fn main() {
    let x = five();

    println!("The value of x is: {}", x);
}
```

<hr />

# 제어문
다른 언어처럼 if 문을 사용할 수 있습니다.
```rust
fn main() {
    let number = 6;

    if number % 4 == 0 {
        println!("number is divisible by 4");
    } else if number % 3 == 0 {
        println!("number is divisible by 3");
    } else if number % 2 == 0 {
        println!("number is divisible by 2");
    } else {
        println!("number is not divisible by 4, 3, or 2");
    }
}
```

표현식을 통해 let 구문에서 사용할 수 있습니다.
```rust
fn main() {
    let condition = true;
    let number = if condition {
        5
    } else {
        6
    };

    println!("The value of number is: {}", number);
}
```

<hr />

# 반복문
## loop

```rust
fn main() {
    // 무한 반복
    loop {
        // if, break 를 통한 탈출
        println!("again!");
    }
}
```

## while
```rust
fn main() {
    let a = [10, 20, 30, 40, 50];
    let mut index = 0;

    while index < 5 {
        println!("the value is: {}", a[index]);

        index = index + 1;
    }
}
```

## for
while 을 사용하여 배열에 인덱스로 접근하는 것은 배열의 크기를 넘은 값을 참조할 위험이 있습니다.  
for 를 통해 안전하게 코드를 짤 수 있습니다.  
```rust
fn main() {
    let a = [10, 20, 30, 40, 50];

    for element in a.iter() {
        println!("the value is: {}", element);
    }
}
```

Range 는 [a,b) 의 숫자를 차례로 생성합니다.  
```rust
fn main() {
    for number in (1..4) {
        println!("{}!", number);
    }
    println!("LIFTOFF!!!");
}
```