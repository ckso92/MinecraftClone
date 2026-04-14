# main.py
import pygame
import sys

# Constants
SCREEN_WIDTH = 800
SCREEN_HEIGHT = 600
BLOCK_SIZE = 40
GRID_WIDTH = SCREEN_WIDTH // BLOCK_SIZE
GRID_HEIGHT = SCREEN_HEIGHT // BLOCK_SIZE

# Colors
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
GREEN = (0, 255, 0)
GRAY = (200, 200, 200)

# Initialize pygame
pygame.init()
screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))
pygame.display.set_caption("Minecraft-like Game")
clock = pygame.time.Clock()

# Player setup
player_pos = [3, 3]

# Grid setup
grid = [[0 for _ in range(GRID_HEIGHT)] for _ in range(GRID_WIDTH)]


def draw_grid():
    for x in range(0, SCREEN_WIDTH, BLOCK_SIZE):
        for y in range(0, SCREEN_HEIGHT, BLOCK_SIZE):
            rect = pygame.Rect(x, y, BLOCK_SIZE, BLOCK_SIZE)
            pygame.draw.rect(screen, GRAY, rect, 1)


def draw_blocks():
    for x in range(GRID_WIDTH):
        for y in range(GRID_HEIGHT):
            if grid[x][y] == 1:
                rect = pygame.Rect(x * BLOCK_SIZE, y * BLOCK_SIZE, BLOCK_SIZE, BLOCK_SIZE)
                pygame.draw.rect(screen, BLACK, rect)


def draw_player():
    rect = pygame.Rect(player_pos[0] * BLOCK_SIZE, player_pos[1] * BLOCK_SIZE, BLOCK_SIZE, BLOCK_SIZE)
    pygame.draw.rect(screen, GREEN, rect)


def main():
    global player_pos
    running = True

    while running:
        screen.fill(WHITE)
        draw_grid()
        draw_blocks()
        draw_player()
        pygame.display.flip()

        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                running = False

        keys = pygame.key.get_pressed()

        # Player movement
        if keys[pygame.K_w] and player_pos[1] > 0:
            player_pos[1] -= 1
        if keys[pygame.K_s] and player_pos[1] < GRID_HEIGHT - 1:
            player_pos[1] += 1
        if keys[pygame.K_a] and player_pos[0] > 0:
            player_pos[0] -= 1
        if keys[pygame.K_d] and player_pos[0] < GRID_WIDTH - 1:
            player_pos[0] += 1

        # Place block
        if keys[pygame.K_SPACE]:
            grid[player_pos[0]][player_pos[1]] = 1

        clock.tick(60)  

    pygame.quit()
    sys.exit()


if __name__ == "__main__":
    main()
