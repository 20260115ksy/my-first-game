## 목표
파란 원 화면에 띄우기

**AI 대화기록

Q1. 파란 원을 움직이게 만드는 방법을 알려줘.
- A. 기본 코드에서 위치(x=400, y= 300) 속도 변수(speed = 5)를 만들고 키보드 입력(keys = pygame.key.get_pressed())으로 움직이게 하면 가능.
-시행착오: 중간에 코드 오류가 남. 다시 한 번 코드를 만들어달라고 요청함.
- 적용 결과 파란 원을 키보드 입력대로 움직이는데 성공함.

Q2. 파란색 원이 창을 넘어가지 않게 코딩하는 방법도 있어?
- A. X,Y 좌표의 이동 후 위치를 강제로 제한. 키 입력 처리 바로 아래 코드 입력.
- 적용 결과 파란 원을 화면 밖으로 나가지 않게 하는데 성공함.

Q3. 벽돌 게임처럼 공이 벽돌을 맞추면 사라지게 해줄 수 있어?
- A. collidepoint() 로 닿았는지 체크하고 *=-1로 공이 벽돌에 맞을 시 반대 방향으로 튕길 수 있도록 설계.
- 벽돌게임 만들기를 성공함.

**배운 점
- 코딩이나 코드에 대해서 하나도 모르는 초보인데, 이 코드는 왜 있는지 이 코드가 반드시 필요한 이유가 뭔지 무슨 역할을 하는지를 AI가 설명해줘서 정말 편했다.
- 또 만들어진 게임을 그냥 플레이하는 것이 아닌 이렇게 AI를 활용해서 내가 원하는대로 만들어볼 수 있다는 점이 정말 흥미로웠다.

import pygame
import sys

pygame.init()
screen = pygame.display.set_mode((800, 600))
pygame.display.set_caption("Brick Breaker")

WHITE = (255, 255, 255)
BLUE = (0, 0, 255)
RED = (255, 0, 0)

clock = pygame.time.Clock()

#공
ball_x = 400
ball_y = 300
ball_dx = 4
ball_dy = -4
radius = 10

#패들
paddle_x = 350
paddle_y = 550
paddle_width = 100
paddle_height = 10
paddle_speed = 6

#벽돌
bricks = []
for i in range(8):
    bricks.append(pygame.Rect(i*100, 50, 80, 30))

running = True

while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    #패들 이동
    keys = pygame.key.get_pressed()
    if keys[pygame.K_LEFT]:
        paddle_x -= paddle_speed
    if keys[pygame.K_RIGHT]:
        paddle_x += paddle_speed

    #공 이동
    ball_x += ball_dx
    ball_y += ball_dy

    #벽 충돌
    if ball_x <= 0 or ball_x >= 800:
        ball_dx *= -1
    if ball_y <= 0:
        ball_dy *= -1

    #패들 충돌
    paddle = pygame.Rect(paddle_x, paddle_y, paddle_width, paddle_height)
    if paddle.collidepoint(ball_x, ball_y):
        ball_dy *= -1

    #벽돌 충돌
    for brick in bricks[:]:
        if brick.collidepoint(ball_x, ball_y):
            bricks.remove(brick)
            ball_dy *= -1
            break

    screen.fill(WHITE)

    #그리기
    pygame.draw.circle(screen, BLUE, (ball_x, ball_y), radius)
    pygame.draw.rect(screen, BLUE, paddle)

    for brick in bricks:
        pygame.draw.rect(screen, RED, brick)

    pygame.display.flip()
    clock.tick(60)

pygame.quit()
sys.exit()
