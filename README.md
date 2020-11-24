# C
C 언어 관련 기억하고 싶은 내용 정리

----------------------------------------------
### 배열 초기화
<details>
   
#### 1. 일반적인 배열
```
int arr[4][5] = {0,}; //전부 0으로 초기화
```
숫자로 크기를 정한 일반적인 배열은 이 형식으로 전체 초기화가 가능하다.
#### 2. 변수로 크기를 지정한 배열 초기화
```
int arr[num][num2] = {0,}; // 에러
```
이런 식으로 변수를 이용해서 선언한 배열은 같은 방법으로 선언했을 때
"Variable-sized object may not be initialized"라고 에러가 난다.
##### 해결 방법 1: for 문 이용
```
int arr[num][num2];
for (int i = 0; i<num; i++){
   for(int j = 0; j<num2; j++){
      arr[i][j] = 0;
   }
}
```
##### 해결 방법 2: memset 이용
```
int arr[num][num2];
memset(arr, 0, num*num2*sizeof(int));
```
처음에 for 문으로 해결했는데 그러면 쓸데없이 코드가 길어진다.
memset으로 처리하니 간결하고 좋다.

아마 사이즈가 정해져 있지 않으면 우항을 결정할 수 없나 보다.
에러를 검색했으면 빨리 해결하는 건데 바보같이 고친 후에 에러 메시지를 확인했다.
다음부터는 에러 메시지부터 찾아서 검색하자.
근데 변수로 크기를 지정한 배열 말고 제대로 된 이름이 있나 모르겠다.
</details>

----------------------------------------------
### 2차원 배열 동적 할당
<details>
   
#### 1. 개요
![2차원동적배열](https://user-images.githubusercontent.com/66747535/100061624-ae934000-2e71-11eb-92f4-8cb4ce2467a6.png)
2차원 배열을 왼쪽 형태로 동적 할당해 주는 경우가 많은데
할당 횟수도 많아지고, 해제하는 과정도 복잡해진다.
하지만 오른쪽 형태로 할당해 주게 되면 과정이 간단해진다.

#### 2. 할당 방법
```
int arr[num][num2];
```
위와 같은 크기의 배열을 동적 할당하고 싶으면
```
int **arr = (int**)malloc(sizeof(int*)*num);
arr[0] = (int*)malloc(sizeof(int)*num*num2);
for( int i=1; i<num; i++) arr[i] = arr[i-1] + num2;
```
이렇게 할당해 주면 된다.

#### 3. 해제 방법
```
free(arr[0]);
free(arr);
```
이렇게 간단하게 해제해 줄 수 있다.

</details>

----------------------------------------------
### 간단한 파일 입출력
* 단순한 숫자 데이터를 읽고 쓰는 간단한 용도

<details>
   
#### 1. 파일 이름 지정
##### 콘솔 창에 입력해서 받기
```
char file[1024];
printf("file name? ");
scanf("%s",file);
```
##### 코드에서 지정해두기
```
char file[] = "file.txt";
```
#### 2. 파일에서 숫자 읽기
##### 1) 파일 열기
```
FILE *fp;
fopen_s(&fp, file, "r"); // read 모드
```
##### 2) 파일에서 숫자 읽기
```
int num;
fscanf_s(fp, "%d", &num);
```
#### 3. 파일에 숫자 쓰기
##### 1) 파일 열기
```
FILE *fp;
fopen_s(&fp, file, "w"); // write 모드
```
##### 2) 파일에 숫자 쓰기
```
int num = 0;
fprintf(fp, "%d", num);
```

#### 4. 참고
```
int mat[rows][cols];

/.../ mat 값 추가

FILE* fp;
fopen_s(&fp, "output.csv", "w");

for (int i = 0; i < rows; i++) {
    for (int j = 0; j < cols; j++) {
        fprintf(fp, "%d", mat[i][j]);
        fprintf(fp, ",");
    }
    fprintf(fp, "\n");
}
```
만약 내보내려는 파일이 csv, 엑셀 형태라면 이런 식으로 내보내면 된다.

</details>
