import pygame
from random import randint
pygame.init()

WHITE = (255,0,90)
BLACK = (0,90,0)
SIZE = (500,500)
FPS = 60

sc = pygame.display.set_mode(SIZE)
clock = pygame.time.Clock()
font = pygame.font.SysFont("arial", 18)

x = SIZE[0] // 2
y = SIZE[1] // 2
R = 25
herospeed = 10
herohp = 10
move = "NONE"

xe = randint(0,450)
ye = -50
enemyspeed = 5
enemycolor = (0,0,0)
enemydamage = 1
blockhp = False



gamemode = 1
isGameRunning = True
while isGameRunning:
    if gamemode == 1:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                isGameRunning = False
            if event.type == pygame.MOUSEBUTTONDOWN:
                if event.pos[0] > 170 and event.pos[0] < 160 + 170 \
                and event.pos[1] > 100 and event.pos[1] < 100 + 50:
                    gamemode = 2
                if event.pos[0] > 170 and event.pos[0] < 160 + 170 \
                and event.pos[1] > 200 and event.pos[1] < 200 + 50:
                    if event.button == 1:
                        if herospeed > 5:
                            herospeed -= 1
                    if event.button == 3:
                        if herospeed < 15:
                            herospeed += 1
                if event.pos[0] > 170 and event.pos[0] < 160 + 170 \
                and event.pos[1] > 300 and event.pos[1] < 300 + 50:
                    isGameRunning = False
                    


        text = font.render("Сложность: " + str(herospeed),1,BLACK)
        
        sc.fill(WHITE)
        pygame.draw.rect(sc, (0,255,0), (170,100,160,50))
        pygame.draw.rect(sc, (255,255,0), (170,200,160,50))
        pygame.draw.rect(sc, (0,0,0), (170,300,160,50))

        sc.blit(text, (170,200))
        
    if gamemode == 2:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                isGameRunning = False
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_UP:
                    move = "UP"
                if event.key == pygame.K_DOWN:
                    move = "DOWN"
                if event.key == pygame.K_LEFT:
                    move = "LEFT"
                if event.key == pygame.K_RIGHT:
                    move = "RIGHT"
            if event.type == pygame.KEYUP:
                move = "NONE"

        if move == "UP":
            y -= herospeed
        if move == "DOWN":
            y += herospeed
        if move == "RIGHT":
            x += herospeed
        if move == "LEFT":
            x -= herospeed
          
        
        ye += enemyspeed
        if ye >= 500:
            ye = -50
            xe = randint(0,450)
            blockhp = False

        if xe + 50 >= x - R and xe <= x + R and ye + 50 >= y - R and ye <= y + R and not blockhp:
            herohp -= enemydamage #Уменьшение здоровья при столкновении
            blockhp = True
        
        text = font.render("Здоровье: " + str(herohp),1,WHITE)
        sc.fill(BLACK)
        sc.blit(text, (10,10))
        pygame.draw.circle(sc, WHITE, (x,y), R)
        pygame.draw.rect(sc, enemycolor, (xe, ye, 50, 50)) #отрисовка противника на экране

    pygame.display.update()

    clock.tick(FPS)

pygame.quit()




            
        
