import pygame
import time
import random

# Initialize Pygame
pygame.init()

# Screen dimensions
width = 600
height = 400
screen = pygame.display.set_mode((width, height))
pygame.display.set_caption('Snake Game')

# Colors
black = pygame.Color(0, 0, 0)
white = pygame.Color(255, 255, 255)
red = pygame.Color(255, 0, 0)
green = pygame.Color(0, 255, 0)
orange = pygame.Color(255, 165, 0)

# FPS controller
fps = pygame.time.Clock()

# Snake settings
snake_size = 10
snake_speed = 11

# Font
font = pygame.font.SysFont('times new roman', 20)

def score_display(score):
    value = font.render("Score: " + str(score), True, white)
    screen.blit(value, [0, 0])

def game_over():
    msg = font.render("Game Over >:]......Press Q to Quit or R to Restart", True, red)
    screen.blit(msg, [width // 6, height // 3])
    pygame.display.flip()
    time.sleep(2)

def main():
    game_running = True
    game_close = False

    x = width // 2
    y = height // 2
    x_change = 0
    y_change = 0

    snake_body = []
    length = 1

    food_x = round(random.randrange(0, width - snake_size) / 10.0) * 10.0
    food_y = round(random.randrange(0, height - snake_size) / 10.0) * 10.0

    while game_running:
        while game_close:
            screen.fill(black)
            game_over()
            score_display(length - 1)
            pygame.display.update()

            for event in pygame.event.get():
                if event.type == pygame.KEYDOWN:
                    if event.key == pygame.K_q:
                        game_running = False
                        game_close = False
                    if event.key == pygame.K_r:
                        main()

        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                game_running = False
            elif event.type == pygame.KEYDOWN:
                if event.key == pygame.K_LEFT:
                    x_change = -snake_size
                    y_change = 0
                elif event.key == pygame.K_RIGHT:
                    x_change = snake_size
                    y_change = 0
                elif event.key == pygame.K_UP:
                    y_change = -snake_size
                    x_change = 0
                elif event.key == pygame.K_DOWN:
                    y_change = snake_size
                    x_change = 0

        x += x_change
        y += y_change

        if x >= width or x < 0 or y >= height or y < 0:
            game_close = True

        screen.fill(black)
        pygame.draw.rect(screen, green, [food_x, food_y, snake_size, snake_size])

        snake_head = []
        snake_head.append(x)
        snake_head.append(y)
        snake_body.append(snake_head)

        if len(snake_body) > length:
            del snake_body[0]

        for segment in snake_body[:-1]:
            if segment == snake_head:
                game_close = True

        for segment in snake_body:
            pygame.draw.rect(screen, orange, [segment[0], segment[1], snake_size, snake_size])

        score_display(length - 1)
        pygame.display.update()

        if x == food_x and y == food_y:
            food_x = round(random.randrange(0, width - snake_size) / 10.0) * 10.0
            food_y = round(random.randrange(0, height - snake_size) / 10.0) * 10.0
            length += 1

        fps.tick(snake_speed)

    pygame.quit()
    quit()

main()
