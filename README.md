# yj-history
파이게임


import pygame
import sys
import time
import random

WHITE=(255,255,255)
pad_with=1280
pad_height=700

def 겜끝표시(text,font):
    textSurface=font.render(text,True,(0,128,0))#Surface는 이미지 파일 읽음
    return textSurface,textSurface.get_rect()#보이지 않는 사각형 가져와 참조

def 메시지위치크기(text):
    global gamepad
    largeText=pygame.font.Font('파이게임글꼴.ttf',100)#글자
    TextSurf,TextRect=겜끝표시(text,largeText)
    TextRect.center=((pad_with/2),(pad_height/2))
    gamepad.blit(TextSurf,TextRect)
    time.sleep(5)
    게임구동()

def 띄울문구():
    global gamepad
    메시지위치크기('게임끝')
    

def 유령(졸,x2,y2):#유령병사
    global gamepad,soldier
    gamepad.blit(soldier,(x2,y2))


def 전(병,x,y):#전령
    global gamepad,messenger
    gamepad.blit(messenger,(x,y))

def 배경(x1,y1):#배경위치
    global gamepad,pygamebg
    gamepad.blit(pygamebg,(x1,y1))


def 해골병(x,y):#위치
    global gamepad,skeleton1
    gamepad.blit(skeleton1,(x,y))

def 게임구동():
    global gamepad,clock,skeleton1,pygamebg#병사의 위치와 이동
    global solldier,messenger #적

    x=pad_with*0.5
    y=pad_height*0.6 #해골위치
    x_이동=0
    y_이동=0

    pygamebg_x1=0
    pygamebg_y1=0 #배경위치

    
    messenger_x=random.randrange(0,pad_with)#위치설
    messenger_y=pad_height

    soldier_x=random.randrange(0,pad_with)
    soldier_y=pad_height

    성파괴=False#해골병사 이동조건과 화면 끄는방법
    while not 성파괴:
        for event in pygame.event.get():
            if event.type==pygame.QUIT:
                성파괴=True


            if event.type==pygame.KEYDOWN:#키누르는동안 조건
                if event.key==pygame.K_RIGHT:
                    x_이동 += 5
                elif event.key==pygame.K_LEFT:
                    x_이동 -= 5
                elif event.key==pygame.K_UP:
                    y_이동-=50
                        
                        
                        
                    
            if event.type==pygame.KEYUP :#키땔때조건
                if event.key==pygame.K_RIGHT or event.key==pygame.K_LEFT or event.key==pygame.K_UP:
                    x_이동=0
                    y_이동=0
                    if y_이동>=-5:
                        
                        y=pad_height*0.6
        
        y+=y_이동        
        x+=x_이동

        gamepad.fill(WHITE)

        messenger_x-=5# 각 적들의 이동속도와 위치조정
        if messenger_x<=0:
            messenger_x=random.randrange(0,pad_with)
            messenger_y=pad_height*0.6
        soldier_x-=7
        if soldier_x<=0: 
            soldier_x=random.randrange(0,pad_with)
            soldier_y=pad_height*0.6
        if x<soldier_x and y==pad_height*0.6:#적한테 맞을때 조건
            if int(x)&int(soldier_x):
                띄울문구()
            
        배경(pygamebg_x1,pygamebg_y1) # 각 함수들 발동
        전(messenger,messenger_x,messenger_y)
        유령(soldier,soldier_x,soldier_y)
        해골병(x,y)
        
        pygame.display.update()
        clock.tick(30)
    pygame.quit()
    quit()

def 모든배경():#해골과 배경넣는 함수 적까지 포함 여기는정의해주는 함수
    global gamepad,clock,skeleton1,pygamebg
    global messenger,soldier

    pygame.init()
    gamepad=pygame.display.set_mode((pad_with,pad_height))#위치
    pygame.display.set_caption('메탈슬러그한장면')
    skeleton1 = pygame.image.load("skeleton.png")#이동가능한해골사진
    pygamebg=pygame.image.load('pygamebg.png')
    messenger=pygame.image.load('messenger.png')
    soldier=pygame.image.load('soldier.png')
    clock=pygame.time.Clock()
    게임구동()

모든배경()
