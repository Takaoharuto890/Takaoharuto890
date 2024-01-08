import pygame
 
pygame.init()
 
# Font that is used to render the text
font20 = pygame.font.Font('freesansbold.ttf', 20)
 
# RGB values of standard colors
BLACK = (40, 46, 51)
WHITE = (128, 128, 128)
GREEN = (83, 100, 118)


WIDTH, HEIGHT = 900, 600
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Pong")
 
clock = pygame.time.Clock()    
FPS = 30
 
 
class Striker:

    def __init__(self, posx, posy, width, height, speed, color):
        self.posx = posx
        self.posy = posy
        self.width = width
        self.height = height
        self.speed = speed
        self.color = color

        self.geekRect = pygame.Rect(posx, posy, width, height)

        self.geek = pygame.draw.rect(screen, self.color, self.geekRect)

    def display(self):
        self.geek = pygame.draw.rect(screen, self.color, self.geekRect)
 
    def update(self, yFac):
        self.posy = self.posy + self.speed*yFac
 

        if self.posy <= 0:
            self.posy = 0

        elif self.posy + self.height >= HEIGHT:
            self.posy = HEIGHT-self.height
 

        self.geekRect = (self.posx, self.posy, self.width, self.height)
 
    def displayScore(self, text, score, x, y, color):
        text = font20.render(text+str(score), True, color)
        textRect = text.get_rect()
        textRect.center = (x, y)
 
        screen.blit(text, textRect)
 
    def getRect(self):
        return self.geekRect

 
 
class Ball:
    def __init__(self, posx, posy, radius, speed, color):
        self.posx = posx
        self.posy = posy
        self.radius = radius
        self.speed = speed
        self.color = color
        self.xFac = 1
        self.yFac = -1
        self.ball = pygame.draw.circle(
            screen, self.color, (self.posx, self.posy), self.radius)
        self.firstTime = 1
 
    def display(self):
        self.ball = pygame.draw.circle(
            screen, self.color, (self.posx, self.posy), self.radius)
 
    def update(self):
        self.posx += self.speed*self.xFac
        self.posy += self.speed*self.yFac
 

        if self.posy <= 0 or self.posy >= HEIGHT:
            self.yFac *= -1
 
        if self.posx <= 0 and self.firstTime:
            self.firstTime = 0
            return 1
        elif self.posx >= WIDTH and self.firstTime:
            self.firstTime = 0
            return -1
        else:
            return 0
 
    def reset(self):
        self.posx = WIDTH//2
        self.posy = HEIGHT//2
        self.xFac *= -1
        self.firstTime = 1

    def hit(self):
        self.xFac *= -1
 
    def getRect(self):
        return self.ball
 

def main():
    running = True
 

    m= Striker(20, 0, 10, 100, 10, GREEN)
    n= Striker(WIDTH-30, 0, 10, 100, 10, GREEN)
    ball = Ball(WIDTH//2, HEIGHT//2, 7, 7, WHITE)
 
    listOfGeeks = [m, n]
 

    mScore, nScore = 0, 0
    mYFac, nYFac = 0, 0
 
    while running:
        screen.fill(BLACK)
 
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                running = False
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_UP:
                    nYFac = -1
                if event.key == pygame.K_DOWN:
                    nYFac = 1
                if event.key == pygame.K_w:
                    mYFac = -1
                if event.key == pygame.K_s:
                    mYFac = 1
            if event.type == pygame.KEYUP:
                if event.key == pygame.K_UP or event.key == pygame.K_DOWN:
                    nYFac = 0
                if event.key == pygame.K_w or event.key == pygame.K_s:
                    mYFac = 0
 

        for geek in listOfGeeks:
            if pygame.Rect.colliderect(ball.getRect(), geek.getRect()):
                ball.hit()
 

        m.update(mYFac)
        n.update(nYFac)
        point = ball.update()
 

        if point == -1:
            mScore += 1
        elif point == 1:
            nScore += 1

        if point:   
            ball.reset()
 
        m.display()
        n.display()
        ball.display()
 
  
        m.displayScore("player: ", 
                           mScore, 100, 20, WHITE)
        n.displayScore("player2: ", 
                           nScore, WIDTH-100, 20, WHITE)
 
        pygame.display.update()
        clock.tick(FPS)     
 
 
if __name__ == "__main__":
    main()
    pygame.quit()

Takaoharuto890/Takaoharuto890 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
