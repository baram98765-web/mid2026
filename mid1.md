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

    for (int i = 0; i < LINES_OF_BRICKS; i++) {
        for (int j = 0; j < BRICKS_PER_LINE; j++) {
            bricks[i][j].position = (Vector2){ j * brickWidth + 1.0f, i * brickHeight + 40.0f };
            bricks[i][j].size = (Vector2){ brickWidth - 2.0f, brickHeight - 2.0f };
            bricks[i][j].active = true;
            bricks[i][j].color = rainbowColors[i % LINES_OF_BRICKS];
        }
    }

    while (!WindowShouldClose()) {
        if (!gameOver && !gameWon) {
            if (IsKeyDown(KEY_LEFT) && paddle.position.x > 0) {
                paddle.position.x -= paddle.speed;
            }
            if (IsKeyDown(KEY_RIGHT) && paddle.position.x < SCREEN_WIDTH - paddle.size.x) {
                paddle.position.x += paddle.speed;
            }

            ball.position.x += ball.speed.x;
            ball.position.y += ball.speed.y;

            if (ball.position.x - ball.radius <= 0 || ball.position.x + ball.radius >= SCREEN_WIDTH) {
                ball.speed.x *= -1.0f;
            }
            if (ball.position.y - ball.radius <= 0) {
                ball.speed.y *= -1.0f;
            }

            if (ball.position.y + ball.radius >= SCREEN_HEIGHT) {
                lives--;
                if (lives <= 0) {
                    gameOver = true;
                } else {
                    ball.position = (Vector2){ SCREEN_WIDTH / 2.0f, SCREEN_HEIGHT / 2.0f };
                    ball.speed = (Vector2){ 4.0f, -4.0f };
                    paddle.position.x = SCREEN_WIDTH / 2.0f - paddle.size.x / 2.0f;
                }
            }

            Rectangle paddleRect = { paddle.position.x, paddle.position.y, paddle.size.x, paddle.size.y };
            if (CheckCollisionCircleRec(ball.position, ball.radius, paddleRect)) {
                if (ball.speed.y > 0) {
                    ball.speed.y *= -1.0f;
                    ball.speed.x = (ball.position.x - (paddle.position.x + paddle.size.x / 2.0f)) / (paddle.size.x / 2.0f) * 5.0f;
                }
            }

            bool activeBricksLeft = false;
            for (int i = 0; i < LINES_OF_BRICKS; i++) {
                for (int j = 0; j < BRICKS_PER_LINE; j++) {
                    if (bricks[i][j].active) {
                        activeBricksLeft = true;
                        Rectangle brickRect = { bricks[i][j].position.x, bricks[i][j].position.y, bricks[i][j].size.x, bricks[i][j].size.y };
                        if (CheckCollisionCircleRec(ball.position, ball.radius, brickRect)) {
                            bricks[i][j].active = false;
                            ball.speed.y *= -1.0f;
                            break;
                        }
                    }
                }
            }

            if (!activeBricksLeft) gameWon = true;

        } else {
            if (IsKeyPressed(KEY_R)) {
                lives = MAX_LIVES;
                gameOver = false;
                gameWon = false;

                ball.position = (Vector2){ SCREEN_WIDTH / 2.0f, SCREEN_HEIGHT / 2.0f };
                ball.speed = (Vector2){ 4.0f, -4.0f };
                paddle.position.x = SCREEN_WIDTH / 2.0f - paddle.size.x / 2.0f;

                for (int i = 0; i < LINES_OF_BRICKS; i++) {
                    for (int j = 0; j < BRICKS_PER_LINE; j++) {
                        bricks[i][j].active = true;
                    }
                }
            }
        }

        BeginDrawing();
        ClearBackground(BLACK);

        if (!gameOver && !gameWon) {
            for (int i = 0; i < LINES_OF_BRICKS; i++) {
                for (int j = 0; j < BRICKS_PER_LINE; j++) {
                    if (bricks[i][j].active) {
                        DrawRectangleV(bricks[i][j].position, bricks[i][j].size, bricks[i][j].color);
                    }
                }
            }

            DrawRectangleV(paddle.position, paddle.size, WHITE);
            DrawCircleV(ball.position, ball.radius, WHITE);

            DrawText(TextFormat("LIVES: %d", lives), 10, 10, 20, GREEN);
        } else if (gameOver) {
            DrawText("GAME OVER", SCREEN_WIDTH / 2 - MeasureText("GAME OVER", 40) / 2, SCREEN_HEIGHT / 2 - 40, 40, RED);
            DrawText("Press [R] to Restart", SCREEN_WIDTH / 2 - MeasureText("Press [R] to Restart", 20) / 2, SCREEN_HEIGHT / 2 + 10, 20, LIGHTGRAY);
        } else if (gameWon) {
            DrawText("YOU WIN!", SCREEN_WIDTH / 2 - MeasureText("YOU WIN!", 40) / 2, SCREEN_HEIGHT / 2 - 40, 40, GREEN);
            DrawText("Press [R] to Restart", SCREEN_WIDTH / 2 - MeasureText("Press [R] to Restart", 20) / 2, SCREEN_HEIGHT / 2 + 10, 20, LIGHTGRAY);
        }

        EndDrawing();
    }

    CloseWindow();
    return 0;
}
```
<img width="802" height="632" alt="image" src="https://github.com/user-attachments/assets/9475225b-2ec5-4d05-92d4-d6694458bb7b" />

<img width="1115" height="628" alt="image" src="https://github.com/user-attachments/assets/8886625e-e3b8-4422-832e-2bb15f6aec07" />
