import pygame
import random

# Initialize Pygame
pygame.init()

# Set up the game window
window_width = 800
window_height = 600
window = pygame.display.set_mode((window_width, window_height))
pygame.display.set_caption("Bike Game")

# Define colors
white = (255, 255, 255)
black = (0, 0, 0)
red = (255, 0, 0)

# Set up the bike
bike_width = 50
bike_height = 100
bike_x = window_width // 2 - bike_width // 2
bike_y = window_height - bike_height
bike_speed = 5

# Set up obstacles
obstacle_width = 100
obstacle_height = 20
obstacle_x = random.randint(0, window_width - obstacle_width)
obstacle_y = -obstacle_height
obstacle_speed = 3

# Initialize score
score = 0
font = pygame.font.Font(None, 36)

clock = pygame.time.Clock()
game_over = False

# Game loop
while not game_over:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            game_over = True

    keys = pygame.key.get_pressed()
    if keys[pygame.K_LEFT]:
        bike_x -= bike_speed
    if keys[pygame.K_RIGHT]:
        bike_x += bike_speed

    # Update obstacle position
    obstacle_y += obstacle_speed
    if obstacle_y > window_height:
        obstacle_x = random.randint(0, window_width - obstacle_width)
        obstacle_y = -obstacle_height
        score += 1

    # Check for collision
    if (
        bike_x < obstacle_x + obstacle_width
        and bike_x + bike_width > obstacle_x
        and bike_y < obstacle_y + obstacle_height
        and bike_y + bike_height > obstacle_y
    ):
        game_over = True

    # Clear the screen
    window.fill(white)

    # Draw the bike
    pygame.draw.rect(window, black, (bike_x, bike_y, bike_width, bike_height))

    # Draw the obstacle
    pygame.draw.rect(
        window, red, (obstacle_x, obstacle_y, obstacle_width, obstacle_height)
    )

    # Draw the score
    score_text = font.render(f"Score: {score}", True, black)
    window.blit(score_text, (10, 10))

    # Update the display
    pygame.display.update()

    clock.tick(60)  # Set the frame rate

# Game over
game_over_text = font.render("Game Over", True, black)
window.blit(game_over_text, (window_width // 2 - 100, window_height // 2))
pygame.display.update()

# Wait for a moment before quitting
pygame.time.wait(2000)

# Quit Pygame
pygame.quit()
