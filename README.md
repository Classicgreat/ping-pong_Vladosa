# ping-pong_Vladosa
САС
ЭТО САМАЯ САСНАЯ ИГРА ДЛЯ ДВОИХ ОТ МЕНЯ!!!! В ней ты и твой друг должны бить ракеткой по мячу и не давать ему попасть на твою зону , а у твоего соперника-друга такая же миссия. Не дай ему попасть мячом на твою зону и победи в сражении используя ракетку из пинг-понга.
Кнопки управления W и S движение вверх и вниз у игрока 1 , UP и DOWN движение вверх и вниз у игрока 2.

from pygame import *

font.init()
font1 = font.SysFont("Arial", 80)
win1 = font1.render('ПОБЕДА ИГРОКА1!', True, (255, 255, 255))
win2 = font1.render('ПОБЕДА ИГРОКА2!', True, (180, 0, 0))

mixer.init()
mixer.music.load('sasa.mp3')
mixer.music.play()

speed_x = 3
speed_y = 3

img_back = "zadniy fon.png"
img_racket1 = "racket1.png"
img_racket2 = "racket2.png"
img_ball = "ball.png"

clock = time.Clock()
FPS = 60

class GameSprite(sprite.Sprite):
    def __init__(self, player_image, player_x, player_y, size_x, size_y, player_speed):
        sprite.Sprite.__init__(self)
        self.image = transform.scale(image.load(player_image), (90,65))
        self.speed = player_speed
        self.rect = self.image.get_rect()
        self.rect.x = player_x
        self.rect.y = player_y
    def reset(self):
        window.blit(self.image, (self.rect.x, self.rect.y))

class Player(GameSprite):
    def update1(self):
        keys = key.get_pressed()
        if keys[K_UP] and self.rect.y < 5:
            self.rect.y -= self.speed
        if keys[K_DOWN] and self.rect.y < win_height - 80:
            self.rect.y += self.speed
    def update2(self):
        keys = key.get_pressed()
        if keys[K_w] and self.rect.y < 5:
            self.rect.y -= self.speed
        if keys[K_s] and self.rect.y < win_height - 80:
            self.rect.y += self.speed

win_width = 1000
win_height = 600
display.set_caption("пинг-pong_Vladosa")
window = display.set_mode((win_width, win_height))
background = transform.scale(image.load(img_back), (win_width, win_height))

racket1 = Player(img_hero, win_width - 50, 100, 100, 50 , 5)
racket2 = Player(img_hero, win_width - 900, 100, 100, 50 , 5)
ball = GameSprite(img_ball , win_height - 100, 100, 100, 50 , 5)

#переменная "игра закончилась": как только там True, в основном цикле перестают работать спрайты
finish = False
game = True

while game:
    for e in event.get():
        if e.type == QUIT:
            game = False
        if not finish:
        window.blit(background,(0,0))

    if finish != True:
        background = transform.scale(image.load(img_back), (win_width, win_height))
        racket1.update1()
        racket2.update2()
        
        ball.rect.x += speed_x
        ball.rect.y += speed_y

        if sprite.spritecollide(racket1, ball) or sprite.spritecollide(racket2, ball):
            speed_x *= -1
            speed_y *= 1
        if ball.rect.y > win_height-50 or ball.rect.y < 0:
            speed_y *= -1
        if ball.rect.x < 0:
            finish = True
            window.blit(win2, (200, 200))
        if ball.rect.x > win_width:
            finish = True
            window.blit(win1, (200, 200))

        racket1.reset()
        racket2.reset()
        ball.reset()

    clock.tick(FPS)
    display.update()
