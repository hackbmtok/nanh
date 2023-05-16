# nanh
ko
import pygame
import random

# Khởi tạo màn hình game
pygame.init()
WIDTH, HEIGHT = 400, 600
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Flappy Bird")

# Màu sắc
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)

# Thông số chim
bird_width = 50
bird_height = 35
bird_x = WIDTH // 3
bird_y = HEIGHT // 2
bird_y_change = 0
gravity = 0.5
jump_height = 8

# Thông số ống
pipe_width = 70
pipe_height = random.randint(150, 400)
pipe_x = WIDTH
pipe_y = random.randint(pipe_height + 100, HEIGHT - 100)
pipe_speed = 4

score = 0
font = pygame.font.Font("freesansbold.ttf", 32)

def display_score():
    text = font.render(f"Score: {score}", True, BLACK)
    screen.blit(text, (10, 10))

def draw_bird():
    pygame.draw.rect(screen, BLACK, (bird_x, bird_y, bird_width, bird_height))

def draw_pipe():
    pygame.draw.rect(screen, BLACK, (pipe_x, 0, pipe_width, pipe_height))
    pygame.draw.rect(screen, BLACK, (pipe_x, pipe_y, pipe_width, HEIGHT - pipe_height))

def check_collision():
    if bird_y < 0 or bird_y + bird_height > HEIGHT or \
            (bird_x + bird_width > pipe_x and bird_x < pipe_x + pipe_width and (bird_y < pipe_height or bird_y + bird_height > pipe_y)):
        return True
    return False

running = True
clock = pygame.time.Clock()

while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_SPACE:
                bird_y_change = -jump_height

    bird_y += bird_y_change
    bird_y_change += gravity

    screen.fill(WHITE)

    draw_pipe()
    draw_bird()

    pipe_x -= pipe_speed
    if pipe_x + pipe_width < 0:
        pipe_x = WIDTH
        pipe_height = random.randint(150, 400)
        pipe_y = random.randint(pipe_height + 100, HEIGHT - 100)
        score += 1

    if check_collision():
        running = False

    display_score()

    pygame.display.update()
    clock.tick(60)

pygame.quit()
