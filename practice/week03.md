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


for event in pygame.event.get():
    if event.type == pygame.QUIT:
        running = False

    if event.type == pygame.KEYDOWN:
        # 방향키 → 시작
        if event.key in [pygame.K_LEFT, pygame.K_RIGHT]:
            ball_moving = True

        # Shift → 공 추가
        if event.key in [pygame.K_LSHIFT, pygame.K_RSHIFT]:
            balls.append([400, 300, 4, -4])

keys = pygame.key.get_pressed()


# 패들 이동
if keys[pygame.K_LEFT]:
    paddle_x -= paddle_speed
if keys[pygame.K_RIGHT]:
    paddle_x += paddle_speed


# 시작 전 → 공을 패들 위에 고정

if not ball_moving:
    for ball in balls:
        ball[0] = paddle_x + paddle_width // 2
        ball[1] = paddle_y - 10


# 공 이동 + 벽 충돌

for ball in balls:
    if ball_moving:
        ball[0] += ball[2]
        ball[1] += ball[3]

    if ball[0] <= 0 or ball[0] >= 800:
        ball[2] *= -1
    if ball[1] <= 0:
        ball[3] *= -1

# -------------------------
# 패들 충돌
# -------------------------
paddle = pygame.Rect(paddle_x, paddle_y, paddle_width, paddle_height)
for ball in balls:
    if paddle.collidepoint(ball[0], ball[1]):
        ball[3] *= -1

# -------------------------
# 벽돌 충돌
# -------------------------
for ball in balls:
    for brick in bricks[:]:
        if brick.collidepoint(ball[0], ball[1]):
            bricks.remove(brick)
            ball[3] *= -1
            break

# ⭐ 벽돌이 다 사라지면 다시 생성
if not bricks:
    bricks = create_bricks()

# -------------------------
# 화면 그리기
# -------------------------
screen.fill(WHITE)

# 공
for ball in balls:
    pygame.draw.circle(screen, BLUE, (ball[0], ball[1]), radius)

# 패들
pygame.draw.rect(screen, BLUE, paddle)

# 벽돌
for brick in bricks:
    pygame.draw.rect(screen, RED, brick)

# 안내 텍스트 (왼쪽 중간)
text = font.render("SHIFT : Add ball to screen", True, BLACK)
screen.blit(text, (10, 280))

pygame.display.flip()
clock.tick(60)
