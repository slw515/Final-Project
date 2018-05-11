# Steven & Yunho's Final-Project
add_library('sound')
add_library('minim')
import os, time, timeit, random
rand10=[]
rand5=[]
shuf = True
shuff = True
while shuf:
    k=random.randint(1,20)
    if rand10.count(k)==0:
        rand10.append(k)
        if len(rand10)==10:
            shuf = False
    else:
        pass
while shuff:
    k=random.randint(1,20)
    if rand10.count(k)==0 and rand5.count(k)==0:
        rand5.append(k)
        if len(rand5)==5:
            shuff = False
    else:
        pass

path = os.getcwd()
print path
minim = Minim(this);
class Game:
    def __init__(self):
        self.w=1280
        self.h=720
        self.g=720-137
        self.platforms=[]
        self.bgImgs=[]
        self.x=0
        self.enemies=[]
        self.Boss=[]
        self.paused = False
        self.state = 'menu'
        self.stars=[]
        self.score = 0
        self.cnt = 0
        self.time = 0
        self.BossGomba=[]
        self.score = 0
        self.stage = 1
        self.name = ''
                
        
    def createGame(self):
        self.keyHandler={SHIFT:False}
        self.bgImgs.append(loadImage(path+'/resources/layer_01.png'))
        self.bgImgs.append(loadImage(path+'/resources/background4.jpg'))
        for b in range(1):
            self.Boss.append(Boss(3300,100,191,self.g,"resources/boss.jpg",1))
        for i in rand10:
            self.enemies.append(Gomba(400+100*i,50,35,self.g,"resources/gomba.jpg",1))
        for i in rand5:
            self.BossGomba.append(BossGomba(400+100*i,100,35,self.g,"resources/gomba2.jpg",1))
        for i in range(5):
            self.stars.append(Star(1000+200*i,100,20,self.g,"resources/star.png",6))
        
        #load sources from a stage file
        #f = open(path+'/resources/stage1.csv','r')
        #for item in f:
            #item = item.strip().split(",")
            #if item[0] is something:
                #self.something = Something(int(item[1]),int(item[2])...)
            #...
        #f.close()
        self.mario = Mario(45,0,90,self.g,"resources/mario3.png",28)
        self.bgMusic=minim.loadFile(path+"/resources/bgmusic2.mp3")
        self.bgMusic.setVolume(0.5)
        self.bgMusic.loop()
        # self.bgMusic=SoundFile(this, path+"/resources/bgmusic1.mp3")
        # self.bgMusic.amp(0.5)
        # self.bgMusic.play()
        self.pauseSound=SoundFile(this, path+"/resources/pause.mp3")
        self.gameover=SoundFile(this, path+"/resources/gameover.mp3")
        self.clearsound=SoundFile(this, path+"/resources/clear.mp3")
        
        for k in rand10:
            self.platforms.append(Platform(300+100*k,450-150*(k%3),200,52,'resources/platform1.jpg'))        
        self.time=0

    def display(self):
        self.cnt  = (self.cnt + 1)%60
        if self.cnt == 0:
            self.time+=1
        cnt = 0
        for img in self.bgImgs[::-1]:
            if cnt == 0:
                x = (self.x//10)%self.w
            elif cnt == 1:
                x = (self.x//5)%self.w
            elif cnt == 2:
                x = (self.x//3)%self.w
            elif cnt == 3:
                x = (self.x//2)%self.w
            else:
                x = (self.x)%self.w
                
            image(img,0,0,self.w-x,self.h,int(x),0,self.w,self.h)
            image(img,self.w-x-1,0,x,self.h,0,0,int(x),self.h)
            cnt+=1
        fill(255)
        text(str(self.score), 10, 25)
        text(str(self.time), 10, 50)
        #stroke(255)
        #line(0,self.g,self.w,self.g)
        
        for s in self.stars:
            s.display()
        
        for e in self.enemies:
            e.display()
            
        for p in self.platforms:
            p.display()
        
        for b in self.Boss:
            b.display()
        
        for bg in self.BossGomba:
            bg.display()
            
        self.mario.display()
            
            

class Creature:
    def __init__(self,x,y,r,g,imgName,F):
        self.x=x
        self.y=y
        self.r=r
        self.w=self.r*2
        self.h=self.r*2
        self.vx=0
        self.vy=0
        self.F=F # max number of frames
        self.f=0 # current frame
        self.g=g
        self.dir=1
        self.img = loadImage(imgName)
        self.jump = 0
        
        
    def gravity(self):
        if self.y+self.r < self.g:
            self.vy+=0.1
            
            if self.y+self.r+self.vy > self.g:
                self.vy = self.g-self.y-self.r
        else:
            self.vy=0
            self.jump=0
            #self.y = self.g-self.r
            
        # adjusting the character ground
        for p in game.platforms:
            if self.x-self.r/2+60 >= p.x and self.x+self.r/2-40 <= p.x+p.w and self.y+self.r <= p.y:
                self.g = p.y
                break
            else:
                self.g=game.g
    
    def update(self):
        self.gravity()
        self.x+=self.vx
        self.y+=self.vy    
        
    def display(self):
        self.update()
        
        if self.vx != 0 and self.vx != 2 and self.vx != -2:
            self.f = (self.f+0.1)%self.F
        else:
            self.f = 27
            
        stroke(0,255,0)
        noFill()
        # ellipse(self.x-game.x,self.y,self.r*2,self.r*2)
        stroke(255,0,0)
        # line(self.x-self.r-game.x,self.g,self.x+self.r-game.x,self.g)
        
        if self.dir > 0:
            image(self.img,self.x-self.r-game.x,self.y-self.r,self.w,self.h,int(self.f)*self.w,0,int(self.f+1)*self.w,self.h)
        else:
            image(self.img,self.x-self.r-game.x,self.y-self.r,self.w,self.h,int(self.f+1)*self.w,0,int(self.f)*self.w,self.h)
         
class Mario(Creature):
    def __init__(self,x,y,r,g,imgName,F):
        self.keyHandler={LEFT:False,RIGHT:False,UP:False,SHIFT:False}
        Creature.__init__(self,x,y,r,g,imgName,F)
        
        
        self.jumpSound = SoundFile (this, path+"/resources/jump.mp3") 
        self.killSound = SoundFile (this, path+"/resources/kill.mp3")
        self.starSound = SoundFile (this, path+"/resources/coin.mp3")
        self.swordSound = SoundFile (this, path+"/resources/swordsound.mp3")
        self.deathSound = SoundFile (this, path+"/resources/death.mp3")
    
    def update(self):
    
        self.gravity() 
        if self.keyHandler[SHIFT] and self.keyHandler[RIGHT]:
            self.vx = 3
            self.dir = -1
        elif self.keyHandler[SHIFT] and self.keyHandler[LEFT]:
            self.vx = -3
            self.dir = 1
                   
        elif self.keyHandler[LEFT]:
            self.vx = -2
            self.dir = 1
        elif self.keyHandler[RIGHT]:
            self.vx = 2
            self.dir = -1
        elif self.keyHandler[SHIFT]:
            self.vx=0.0000000000000000001
            # else:
            #     self.vx=0
            #     self.x = self.x+200
            #     self.f=self.f+1
        
        else:
            self.vx = 0
            
        if self.keyHandler[UP] and self.jump < 2 and self.vy >= 0:
            self.vy = -6
            self.jumpSound.play()
            self.jump+=1
        
        self.x+=self.vx
        self.y+=self.vy 
        
        if self.x >= game.w//2:
            game.x += self.vx
        #collision detection
        for e in game.enemies:
            if self.distance(e) < self.r+e.r-30:
                if self.dir != 1:
                    
                    if self.x < e.x-10 and self.y < e.y+100 and self.vx !=0 and self.vx !=2 and self.vx !=-2:
                        game.enemies.remove(e)
                        del e
                        self.killSound.play()
                        game.score += 10
                    else:
                        game.gameover.play()
                        self.deathSound.play()
                        game.paused = not game.paused
                        game.bgMusic.close()
                        time.sleep(4)
                    
                        game.state='inputName'
                else:
                    if self.x > e.x+10 and self.y < e.y+100 and self.vx !=0 and self.vx !=2 and self.vx !=-2:
                        game.enemies.remove(e)
                        del e
                        self.killSound.play()
                        game.score += 10
                    else:
                        game.gameover.play()
                        self.deathSound.play()
                        game.paused = not game.paused
                        game.bgMusic.close()
                        time.sleep(4)
                    
                        game.state='inputName'
        for e in game.BossGomba:
            if self.distance(e) < self.r-33+e.r:
                if self.dir != 1:
                    
                    if self.x < e.x-10 and self.y < e.y+100 and self.vx !=0 and self.vx !=2 and self.vx !=-2:
                        game.BossGomba.remove(e)
                        del e
                        self.killSound.play()
                        game.score += 10
                    else:
                        game.gameover.play()
                        self.deathSound.play()
                        game.paused = not game.paused
                        game.bgMusic.close()
                        time.sleep(4)
                    
                        game.state='inputName'
                else:
                    if self.x > e.x+10 and self.y < e.y+100 and self.vx !=0 and self.vx !=2 and self.vx !=-2:
                        game.BossGomba.remove(e)
                        del e
                        self.killSound.play()
                        game.score += 10
                    else:
                        game.gameover.play()
                        self.deathSound.play()
                        game.paused = not game.paused
                        game.bgMusic.close()
                        time.sleep(4)
                    
                        game.state='inputName'
        
        for b in game.Boss:
            if self.distance(b) < self.r+b.r-30:
                if self.dir != 1:
                    
                    if self.x < b.x-10 and self.y < b.y and self.vx !=0 and self.vx !=2 and self.vx !=-2 and game.score > 150:
                        game.Boss.remove(b)
                        del b
                        self.killSound.play()
                        game.score += 10
                        game.clearsound.play()
                        game.bgMusic.close()
                        time.sleep(4)
                        game.state='inputName'
                    else:
                        game.gameover.play()
                        self.deathSound.play()
                        game.paused = not game.paused
                        game.bgMusic.close()
                        time.sleep(4)
                    
                        game.state='inputName'
                else:
                    if self.x > b.x+10 and self.y < b.y and self.vx !=0 and self.vx !=2 and self.vx !=-2 and game.score > 150:
                        game.Boss.remove(b)
                        del b
                        self.killSound.play()
                        game.score += 10
                        game.clearsound.play()
                        game.bgMusic.close()
                        time.sleep(4)
                        game.state='inputName'
                    else:
                        game.gameover.play()
                        self.deathSound.play()
                        game.paused = not game.paused
                        game.bgMusic.close()
                        time.sleep(4)
                        game.state='inputName'
                          
                                            
        for s in game.stars:
            if self.distance(s) < self.r+s.r-30:
                game.stars.remove(s)
                del s
                self.starSound.play()
                game.score += 10
                
    def distance(self,other):
        return((self.x-other.x)**2+(self.y-other.y)**2)**0.5

        

class Platform:
    def __init__(self,x,y,w,h,img):
        self.x=x
        self.y=y
        self.w=w
        self.h=h
        self.img=loadImage(path+'/'+img)
    
    def display(self):
        image(self.img,self.x-game.x,self.y)

class Gomba(Creature):
    def __init__(self,x,y,r,g,imgName,F):
        Creature.__init__(self,x,y,r,g,imgName,F)
        self.vx = 1
        self.x1=self.x-50
        self.x2=self.x+50
        
    def update(self):
        self.gravity()
        
        if self.x < self.x1:
            self.vx = 1
            self.dir = 1
        elif self.x > self.x2:
            self.vx=-1
            self.dir=-1
            
        self.x+=self.vx
        self.y+=self.vy
class Star (Creature):
    def __init__(self,x,y,r,g,imgName,F):
        Creature.__init__(self,x,y,r,g,imgName,F)
        self.vx=0.0000000000000000000001
class BossGomba (Creature):
    def __init__(self,x,y,r,g,imgName,F):
        Creature.__init__(self,x,y,r,g,imgName,F)
        self.vx = -3
        self.vy = -6
        self.x1=self.x-random.randint(1,3)*100
        self.x2=self.x+300
    def update(self):
        self.gravity()
        
        if self.x < self.x1:
            self.vx = 3
            self.dir = -1
        elif self.x > self.x2:
            self.vx=-3
            self.dir=1
            
        self.x+=self.vx
        self.y+=self.vy
class Boss (Creature):
    def __init__(self,x,y,r,g,imgName,F):
        Creature.__init__(self,x,y,r,g,imgName,F)
        self.vx = 1
        self.x1=self.x-random.randint(1,2)*100
        self.x2=self.x+random.randint(1,2)*100
        self.vy=1
    def update(self):
        self.gravity()
        
        if self.x < self.x1:
            self.vx = 1
            self.dir = -1
        elif self.x > self.x2:
            self.vx=-1
            self.dir=1
        if self.vy==0:
            self.vy = -5
            
        self.x+=self.vx
        self.y+=self.vy
        
        
        
        
            
        
        
        
game = Game()

def setup():
    size(game.w,game.h)
    background(0)
    game.createGame()
    
def draw():
    if game.state=='menu':
        background(loadImage(path+'/resources/background5.jpg'))
        if game.state=='menu' and game.w//2-100 <= mouseX <= game.w//2+160 \
        and game.h//2-30 <= mouseY <= game.h//2+10:
            fill(0,255,0)
        else:
            fill(255)
        textSize(32)
        text("Play Game",game.w//2-70,game.h//2)
        # noFill()
        # stroke(255)
        # rect(game.w//2,game.h//2-30,160,40)
    elif game.state == 'play':
        if not game.paused:
            background(0)
            game.display()
        else:
            fill(255,0,0)
            textSize(32)
            text("Pause",game.w//2,game.h//2)
    elif game.state=='inputName':
        background(0)
        textSize(32)
        text("Please enter your name"+"\n"+"To check the records"+"\n"+"of the game, open "+"\n"+"highscores.csv in the folder",game.w//2-200,game.h//2-200)
        text(game.name,game.w//2-200,game.h//2)
    
def keyPressed():
    if game.state=='play':
        print (keyCode)
        game.mario.keyHandler[keyCode]=True
        
        if keyCode == 80:
            game.paused = not game.paused
            game.pauseSound.play()
    elif game.state=='inputName':
        print keyCode, key, type(key)
        if keyCode == 8:
            game.name = game.name[:len(game.name)-1]
        elif keyCode == 10:
            f = open("highscores.csv","a")
            f.write(game.name+','+str(game.score)+','+str(game.time)+'\n')
            f.close()
            game.__init__()
            game.createGame()
        elif type(key) != int :
            game.name += key
            
def keyReleased():
    game.mario.keyHandler[keyCode]=False
    
def mouseClicked():
    #game.w//2,game.h//2-30,160,40
    if game.state=='menu' and game.w//2 -70 <= mouseX <= game.w//2+140 \
        and game.h//2-30 <= mouseY <= game.h//2+10:
       game.state='play'
