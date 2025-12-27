mport pygame
import sys

# 初始化 Pygame
pygame.init()

# 游戏窗口设置
WIDTH, HEIGHT = 600, 400
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption('乒乓球游戏')

# 颜色定义
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)

# 球拍设置
PADDLE_WIDTH, PADDLE_HEIGHT = 15, 60
paddle_speed = 10

# 球设置
ball_width = 15
ball_speed_x, ball_speed_y = 7, 7

# 左右球拍位置
left_paddle = pygame.Rect(30, HEIGHT // 2 - PADDLE_HEIGHT // 2, PADDLE_WIDTH, PADDLE_HEIGHT)
right_paddle = pygame.Rect(WIDTH - 45, HEIGHT // 2 - PADDLE_HEIGHT // 2, PADDLE_WIDTH, PADDLE_HEIGHT)

# 球的位置
ball = pygame.Rect(WIDTH // 2 - ball_width // 2, HEIGHT // 2 - ball_width // 2, ball_width, ball_width)

# 分数
left_score = 0
right_score = 0

# 游戏字体
font = pygame.font.SysFont("Arial", 30)

# 游戏主循环
while True:
    screen.fill(BLACK)

    # 事件处理
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()

    # 获取键盘状态
    keys = pygame.key.get_pressed()

    # 控制左侧球拍
    if keys[pygame.K_w] and left_paddle.top > 0:
        left_paddle.y -= paddle_speed
    if keys[pygame.K_s] and left_paddle.bottom < HEIGHT:
        left_paddle.y += paddle_speed

    # 控制右侧球拍
    if keys[pygame.K_UP] and right_paddle.top > 0:
        right_paddle.y -= paddle_speed
    if keys[pygame.K_DOWN] and right_paddle.bottom < HEIGHT:
        right_paddle.y += paddle_speed

    # 球的移动
    ball.x += ball_speed_x
    ball.y += ball_speed_y

    # 碰到上下边界反弹
    if ball.top <= 0 or ball.bottom >= HEIGHT:
        ball_speed_y = -ball_speed_y

    # 碰到左右边界，给分
    if ball.left <= 0:
        right_score += 1
        ball.x = WIDTH // 2 - ball_width // 2
        ball.y = HEIGHT // 2 - ball_width // 2
        ball_speed_x = -ball_speed_x

    if ball.right >= WIDTH:
        left_score += 1
        ball.x = WIDTH // 2 - ball_width // 2
        ball.y = HEIGHT // 2 - ball_width // 2
        ball_speed_x = -ball_speed_x

    # 碰到球拍反弹
    if ball.colliderect(left_paddle) or ball.colliderect(right_paddle):
        ball_speed_x = -ball_speed_x

    # 绘制球拍和球
    pygame.draw.rect(screen, WHITE, left_paddle)
    pygame.draw.rect(screen, WHITE, right_paddle)
    pygame.draw.ellipse(screen, WHITE, ball)

    # 显示分数
    left_text = font.render(str(left_score), True, WHITE)
    right_text = font.render(str(right_score), True, WHITE)
    screen.blit(left_text, (WIDTH // 4 - left_text.get_width() // 2, 20))
    screen.blit(right_text, (WIDTH * 3 // 4 - right_text.get_width() // 2, 20))

    # 更新屏幕
    pygame.display.flip()

    # 设置帧率
    pygame.time.Clock().tick(60)
