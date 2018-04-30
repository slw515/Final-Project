# Steven & Yunho's Final-Project
add_library('minim')
add_library('sound')
import os
path = os.getcwd()

minim = Minim(this);

class Game:
    def __init__(self):
        self.w = 1920
        self.h = 600
        self.score = 0
        self.stage = 1
    
    def createGame(self):
        self.platforms=[]
        self.bgImgs=[]
        self.x=0
        
        self.bgMusic=minim.loadFile(path+"/resources/bgmusic1.mp3")
        # self.bgMusic.rate(0.01)
        # self.bgMusic.amp(0.7)
        # self.bgMusic.loop(1)
        self.bgMusic.loop()
        self.img = loadImage(path+"/resources/background1.jpg")
        
    
    
    def display(self):
        image(self.img,0,0,1920,600)
        
class Creature:
    def __init__(self,x,y,imgName,F):
        self.x=x
        self.y=y
        self.vx=0
        self.F=F
        self.F=F
        self.f=0
        self.img = loadImage(imgName)
    
    def update(self):
        self.x+=self.vx
    
    def display(self):
        self.update()
        
        if self.vx != 0:
            self.f = (self.f+0.1)%self.F
    # def update(self):
    #     if attack:
    #         self.
    #         self.x+=self.vx

game = Game()

def setup():
    size(game.w,game.h)
    background(0)
    game.createGame()

def draw():
    background(0)
    game.display()
