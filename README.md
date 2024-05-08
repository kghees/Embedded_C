# Embedded_C  

### App
- Application : **운영체제 안에서** 동작하는 프로그램을 App 이라고 한다.

### Middleware(미들웨어)  
- Application과 운영체제 중간 다리 역할
  
    - 운영체제의 신호를 App이 가져갈 수 있는 API
  
    - App Level에서 운영체제에게 신호를 전달하는 API
  
![image](https://github.com/kghees/Embedded_C/assets/92205960/e5cec51b-306c-42c0-85d9-d715ae2d8637)

### Low Level 개발  
- Firmware 개발
  - H/W를 제어하는, 작은 운영체제를 직접 개발
  - 주 언어 : C언어, Assembly
- Device Driver 개발
  - 커널 내부에서 동작되는 프로그램으로, H/W를 제어하는 프로그램 개발
  - 주 언어 : C언어

- **주소는 Byte 단위**이다. (1Byte = 8bit)
- OS는 메모리 주소를 1Byte단위로 관리한다.

### 포인터란?
- "포인터 변수"를 줄여서 포인터 라고한다.
- **메모리 주소**를 보관하는 변수이다.
- 데이터의 복사를 피하고, 데이터를 공유하여 작업하고자 할 때 쓴다.
```c
*pa = &a; -> 포인터를 처음 선언할 때만 사용한다.
pa = &a;
*pa가 값이고, pa가 주소다.
가리키고 있을 때, *을 사용해서, 가리킨 영역의 값을 제어할 수 있다.
int x = 100;
int *p = &x;
int *g = &x;
*p = 5000; //실제 x에 5000이 들어감
printf("%d", *g); //5000이 출력됨
```
```c
배열의 이름이 바로 "배열의 시작 메모리 주소"이다.
int arr[5] = {1,5,3,2,6};
int *p;
p= arr;
printf("%x\n",p); // 주소 값 출력
printf("%x\n",&arr[0]); // 주소 값 출력
printf("%x\n",arr); // 주소 값 출력

for (int i = 0; i < 5; i++) {
	printf("%d\n", *(p + i)); //1,5,3,2,6 순서대로 출력 
}
```
```c
void bbq(int* p) { //포인터로 받는다.
	printf("%d\n", p[1]);
}

int main() {
	int arr[5] = { 1,3,2,1,2 };

	bbq(arr); //주소를 보낸다.
	return 0;
}
C언어 내부적으로 1차원 배열을 포인터로 자동 변환한다.
```
```c
int main(){
	const char *p = "ABC";
	for(int i=0; i<3; i++)
		printf("%c ",p[i]); //문자 하나씩 출력 A B C
	return 0;
}
```
### 진수 변환  
- 진수 변환은 임베디드 장치로부터 나온 Data를 분석하는 과정이다.
#### strrol()  
```c
long strrol(char const* str, char** endptr, int base)
문자열에서 숫자를 추출하는 함수
str : 문자열
endptr : 문자열 끝 포인터
base : 변환 될 문자열의 진수(2,8,16,10)
return : 성공 ->변환된 정수, 실패 -> 0
int main(){
	char inputH[10]= "0xFF";
	char inputB[10]= "1010";
	char inputO[10]= "52";
	char inputD[10]= "123";

	int result16 = strtol(inputH,NULL,16);
	int result2 = strtol(inputB,NULL,2);
	int result8 = strtol(inputO,NULL,8);
	int result10 = strtol(inputD,NULL,10);

	printf("%d\n", result16); //255
	printf("%d\n", result2); //10
	printf("%d\n", result8); //42
	printf("%d\n", result10); //123

	return 0;
}
```
#### sprintf()  
```c
int sprintf(char *str, const char* format, ...)
문자열에 지정한 형식으로 담는다.
문자열 끝에 '\0'이 자동으로 추가된다.
str : 변환된 문자열을 담을 버퍼
format : string 문자 서식
return : 성공 ->byte 수, 실패 -> -1
int main() {

	int a = 100;
	char buf[10] = { 0 };

	sprintf(buf, "%d", a);
	printf("%s", buf);

	return 0;
}
```
### 비트연산  
- & 연산(and)
  - 값을 추출할 때 사용, 값에 "1"을 적은 곳만 추출한다.
- | 연산(or)
  - 2진수 덧셈시에 사용, 둘 중 한 곳에 "1"이 있으면, 결과는 "1"이다.
- ^ 연산(XOR)
  - 같으면 0, 다르면 1, 암호화에 사용된다.
- Shift 연산자
  - 비트 단위로 이동한다.
  - Left Shift : "<<" 값을 왼쪽으로 밀어서 숫자를 추가해준다.
  - Right Shift : ">>" 값을 오른쪽으로 밀어서 숫자를 없앤다.
- ~ 연산(not)
  - 비트가 반대가 된다. "기준 비투 수"에 따라 결과 값이 달라질 수 있다.

#### char과 unsigned char의 차이  
**char**  
- MSB를 부호로 사용한다.
- 나머지 7bit를 수를 저장하는 용도로 사용한다.
- 저장할 수 있는 수 : -128 ~ 127

**unsigned char**  
- 부호는 없다.
- 8bit 모두 수를 저장하는 용도로 사용한다.
- 0 ~ 255까지 저장 가능

```
2번 bit에서 1개 bit 추출하여 변수 b에 저장하기
b = (a >> 2) & 0x01;
5번 bit에서 1개 bit 추출하여 변수 b에 저장하기
b = (a >> 5) & 0x01;
6번 bit에서 2개 bit 추출하여 변수 c에 저장하기
c = (a >> 6) & 0b11; 
```
#### 비트 set/clear, 반전  

**비트 clear**  
- 특정 비트를 0으로 만드는 것을 clear 한다고 표현한다.
  
**비트 set**
  
- 특정 비트를 1로 만드는 것을 set한다라고 한다.

**비트 반전**
  
- XOR을 이용하여 반전
```
6번 bit에서 1개 bit set하여 변수 b에 저장하기
b = (1 << 6) | a
3번 bit에서 2개 bit set하여 변수 c에 저장하기
c = (0b11 << 3) | a

1번 bit에서 1개 bit clear하여 변수 b에 저장하기
b = ~(1 << 1) & a
3번 bit에서 2개 bit clear하여 변수 c에 저장하기
c = ~(0x3 << 3) & a

1번 bit에서 1개 bit 반전하여 변수 b에 저장
b = a^(1<<1)
변수 b에 다시 1번 bit를 1개 bit 반전하여 c에 저장
c = b^(1<<1)
5번 bit에서 3개 bit 반전하여 변수 d에 저장
d = a^(0b111 << 5)
변수 d에서 다시 한번 5번 bit에서 3개 bit 반전하여 변수 e에 저장
e = d^(0x7 << 5)
```

### 파싱(parsing)이란?  
- 파싱(parsing)은 임베디드 업계에서 Visualization(시작화 = 차트화)을 하기위해 자주 사용된다.
- **파싱을 어디에 사용할까?**
  - H/W를 만들어내는 무수한 장비들은 로그데이터가 존재한다.(작동이 잘되고 있는지,에러가 발생했는지)
  - Law Data를 파싱을 통해 가공하고, 가공된 데이터를 차트화 하는 업무를 수행

### String Library  
```
strlen(arr) : 문자열(arr)의 길이를 반환한다.(널문자를 만날 때까지 진행한다.)

strcpy(arr,brr) : 문자열을 복사한다. (arr에 brr 값 복사)

strncpy(arr,brr,2) : 문자열을 n개 복사한다. (arr에 brr 값 2개 복사)

strcat(arr,brr) : 문자열을 이어 붙인다. (arr에 brr 이어 붙이기)

strncat(arr,brr,2) : 문자열을 n개 이어 붙인다. (arr에 brr 2개 이어 붙이기)

strcmp(arr,brr) : 두 문자열을 비교한다. (arr,brr 비교) (같으면 0, 앞서면 -1, 뒤라면 1)

strncmp(arr,brr,3) : 두 문자열을 n개 비교한다.

sscanf(arr, "[%f]%s%d", &time, msg, &num) : 문자열에서 형식화된 데이터를 읽어온다.

sprintf(arr, "[%.6f] %s %d", time, msg, num) : 형식화된 데이터를 문자열로 저장한다.

atoi(arr) : 문자열을 정수로 변환한다.

strtok(arr, "!") : 문자열을 지정된 구분자로 토근으로 분리한다. (Split)

(구분자를 찾으면 NULL로 전환)

strchr(arr, 'A') : 문자열에서 문자 찾기

strstr(arr, "AB") : 문자열에서 문자열 찾기 (대소문자 구분)
```
