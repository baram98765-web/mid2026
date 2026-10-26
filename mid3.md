# 3번 답안

```
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

int main(void) {
    int com[3];     // 컴퓨터의 3자리 숫자
    int user[3];    // 사용자가 입력한 3자리 숫자
    int strike = 0;
    int ball = 0;
    int tries = 0;

    // 1. 난수 생성 초기화 (컴퓨터가 3개의 랜덤 숫자 뽑기)
    srand((unsigned int)time(NULL));

    // 1~9 사이의 서로 다른 숫자를 뽑는 로직
    com[0] = rand() % 9 + 1; // 1 ~ 9
    do {
        com[1] = rand() % 9 + 1;
    } while (com[1] == com[0]); // 첫 번째 숫자와 겹치면 다시 뽑기

    do {
        com[2] = rand() % 9 + 1;
    } while (com[2] == com[0] || com[2] == com[1]); // 앞의 숫자들과 겹치면 다시 뽑기

    printf("=== 숫자 야구 게임을 시작합니다! (1~9 중 3개) ===\n");

    // 2. 게임 메인 루프 (3스트라이크가 될 때까지 반복)
    while (1) {
        strike = 0;
        ball = 0;
        tries++;

        printf("\n시도 %d회 - 숫자 3개를 띄워서 입력하세요 (예: 1 2 3): ", tries);
        scanf("%d %d %d", &user[0], &user[1], &user[2]);

        // 3. 스트라이크와 볼 판정 (이중 for문 핵심 로직)
        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {
                if (com[i] == user[j]) {
                    if (i == j) {
                        strike++; // 자릿수와 숫자가 모두 같으면 스트라이크
                    } else {
                        ball++;   // 숫자는 같지만 자릿수가 다르면 볼
                    }
                }
            }
        }

        // 4. 결과 출력
        if (strike == 0 && ball == 0) {
            printf("Out! (아웃)\n");
        } else {
            printf("결과: %d 스트라이크, %d 볼\n", strike, ball);
        }

        // 5. 정답 체크
        if (strike == 3) {
            printf("\n정답입니다! %d번 만에 맞췄습니다!\n", tries);
            break;
        }
    }

    return 0;
}
```

<img width="1115" height="628" alt="image" src="https://github.com/user-attachments/assets/ce747b93-4794-4525-94ed-a084d292ce1e" />

