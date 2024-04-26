# Embedded_C  

### App
- Application : **운영체제 안에서** 동작하는 프로그램을 App 이라고 한다.

### Middleware(미들웨어)  
- Application과 운영체제 중간 다리 역할
    운영체제의 신호를 App이 가져갈 수 있는 API
  
    App Level에서 운영체제에게 신호를 전달하는 API
  
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
