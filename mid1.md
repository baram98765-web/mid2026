# 1번 답안
## 나의 프롬포트: 무지개 블록깨기 게임을 씨언어로 작성하시오. (그래픽 raylib 최신 버전 라이브러리 활용), 비주얼 스튜디오 최신 버전에서 nuget에서 설치해서 사용하고 싶어. 한글은 절대 사용하지 말것. 반드시 작동이 되어야 하고 5회 실패시 다시 시작하도록 해줘. 코드는 간결하게

```
#include "raylib.h"

#define SCREEN_WIDTH 800
#define SCREEN_HEIGHT 600
#define LINES_OF_BRICKS 5
#define BRICKS_PER_LINE 10
#define MAX_LIVES 5

typedef struct Paddle {
    Vector2 position;
    Vector2 size;
    float speed;
} Paddle;

typedef struct Ball {
    Vector2 position;
    Vector2 speed;
    float radius;
    bool active;
} Ball;

typedef struct Brick {
    Vector2 position;
    Vector2 size;
    bool active;
    Color color;
} Brick;

int main(void) {
    InitWindow(SCREEN_WIDTH, SCREEN_HEIGHT, "Rainbow Brick Breaker");
    SetTargetFPS(60);

    Paddle paddle = {
        .position = { SCREEN_WIDTH / 2.0f - 50.0f, SCREEN_HEIGHT - 40.0f },
        .size = { 100.0f, 15.0f },
        .speed = 8.0f
    };

    Ball ball = {
        .position = { SCREEN_WIDTH / 2.0f, SCREEN_HEIGHT / 2.0f },
        .speed = { 4.0f, -4.0f },
        .radius = 8.0f,
        .active = true
    };

    Brick bricks[LINES_OF_BRICKS][BRICKS_PER_LINE];
    Color rainbowColors[LINES_OF_BRICKS] = { RED, ORANGE, YELLOW, GREEN, BLUE };

    float brickWidth = (float)SCREEN_WIDTH / BRICKS_PER_LINE;
    float brickHeight = 20.0f;

    int lives = MAX_LIVES;
    bool gameOver = false;
    bool gameWon = false;

    // Initialize bricks
    for (int i = 0; i < LINES_OF_BRICKS; i++) {
        for (int j = 0; j < BRICKS_PER_LINE; j++) {
            bricks[i][j].position = (Vector2){ j * brickWidth + 1.0f, i * brickHeight + 40.0f };
            bricks[i][j].size = (Vector2){ brickWidth - 2.0f, brickHeight - 2.0
```
