# 4번 답안

```
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

int main(void) {
    int n;

    printf("피라미드의 높이 n을 입력하세요: ");
    scanf("%d", &n);

    // 1부터 n까지 행(row)을 반복
    for (int i = 1; i <= n; i++) {
        
        // 1. 중앙 정렬을 위한 공백 출력 (n - i 개 만큼 띄어쓰기)
        for (int j = 0; j < n - i; j++) {
            printf(" ");
        }

        // 2. 행 번호에 따라 출력할 문자 결정 (홀수 행: '*', 짝수 행: '#')
        char ch = (i % 2 != 0) ? '*' : '#';

        // 3. 해당 행의 문자 출력 (2 * i - 1 개 만큼 출력)
        for (int k = 0; k < 2 * i - 1; k++) {
            printf("%c", ch);
        }

        // 4. 한 행이 끝나면 줄바꿈
        printf("\n");
    }

    return 0;
}
```

<img width="1115" height="628" alt="image" src="https://github.com/user-attachments/assets/c94ec42e-1ec8-4974-a8e5-ef540d003264" />
