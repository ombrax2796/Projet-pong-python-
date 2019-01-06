# Projet-pong-python-

    from tkinter import *
    from tkinter import colorchooser
    import time
    from random import *
    from threading import Thread



    colorj1 = 'red'
    colorj2 = 'red'
    colorball= 'red'

    #code des parametres (couleur, points,vitesse)
    def parametre(): 
        global colorj1,colorj2,colorball,V,WIN
        #fenetre de jeu et des composants du jeu
        fenetreP=Toplevel()
        fenetreP.title('pong')
        can=Canvas(fenetreP,width=150,height=150,bg='black') 

        #nb de points pour gagner
        Nbpg=Label(fenetreP,text='Nombre de points gagnants?')
        points=Scale(fenetreP,orient='horizontal',from_=3,to=10,resolution=1,tickinterval=1)
        WIN=(points.get())


        #vitesse balle
        vitessebaaf=Label(fenetreP,text='Vitesse balle?')
        vitesseba=Scale(fenetreP,orient='horizontal',from_=0,to=20,resolution=1,tickinterval=5)
        V=int(vitesseba.get())


        #couleur raquette1
        RGB1=Label(fenetreP,text='couleur joueur1?')
        def coulJ1():
            global colorj1
            color = colorchooser.askcolor()
            color
            ((106.4140625, 83.32421875, 238.9296875), '#6a53ee')
            colorj1=color[1]
            couleurj1=can.create_rectangle(10,15,20,45, fill=colorj1)
        BouttoncoulJ1= Button(fenetreP,text='changer couleur J1',command=coulJ1,width=20)
        couleurj1=can.create_rectangle(10,15,20,45, fill=colorj1)



        #couleur raquette 2
        RGB2=Label(fenetreP,text='couleur joueur2?')
        def coulJ2():
            global colorj2
            color = colorchooser.askcolor()
            color
            ((106.4140625, 83.32421875, 238.9296875), '#6a53ee')
            colorj2=color[1]
            couleurj2=can.create_rectangle(130,15,140,45,fill=colorj2)
        BouttoncoulJ2= Button(fenetreP,text='changer couleur J2',command=coulJ2,width=20)
        couleurj2=can.create_rectangle(130,15,140,45,fill=colorj2)


        #couleur balle
        RGBB=Label(fenetreP,text='couleur balle?')
        def coulball():
            global colorball
            color = colorchooser.askcolor()
            color
            ((106.4140625, 83.32421875, 238.9296875), '#6a53ee')
            colorball=color[1]
            ball=can.create_oval(75,80,85,90,fill=colorball)
        Bouttoncoulball= Button(fenetreP,text='changer couleur ball',command=coulball,width=20)
        ball=can.create_oval(75,80,85,90,fill=colorball)



        #placements composants
        can.grid(row=6,column=7,columnspan=2)
        Nbpg.grid(row=0,column=0)
        points.grid(row=0,column=1)
        vitessebaaf.grid(row=1,column=0)
        vitesseba.grid(row=1,column=1)
        RGB1.grid(row=2,column=0)
        BouttoncoulJ1.grid(row=2,column=1)
        RGB2.grid(row=3,column=0)
        BouttoncoulJ2.grid(row=3,column=1)
        RGBB.grid(row=4,column=0)
        Bouttoncoulball.grid(row=4,column=1)

        #code du jeu 
        def play():
            fenetreP.destroy()
            #variables
            global colorj1,colorj2,colorball,V,WIN,PJ1,PJ2,T,KIWIN# les variables global
            #variables temps de jeu
            t0 = time.clock()#capture du temps au début du jeu
            T=0
            #variables pour vitesse et direction balles aléatoire
            A=[-2,2]
            B=[-2,2]
            a=choice(A)+V
            b=choice(B)
            #variables points joueurs
            PJ1=0
            PJ2=0
            #variables pour place bonus
            x1=400
            x2=440
            y1=200
            y2=240


            #fenetre de jeu et des composants du jeu
            fenetrej=Toplevel()
            can=Canvas(fenetrej,width=1200,height=700,bg='black') 
            btn_retour=Button(fenetrej,text='Retour',command=fenetrej.destroy,width=20)
            w=Message(fenetrej,fg='blue', text=PJ1)
            z=Message(fenetrej,fg='blue', text=PJ2)
            fenetrej.update()

            """
            #teste non comcluants :(
            def les_bonus():
                global positionX_aléa,positionY_aléa,bonus2j
                #position bonus/malus aléatoire
                positionX_aléa=randrange(0, 460)
                positionY_aléa=randrange(0, 260) 
                #choix malus ou bonus
                #BouM=randint(1,3)
                BouM=1
                activ=1
                if BouM==1:
                    bonus2j=can.create_oval(x1+positionX_aléa,y1+positionY_aléa,x2+positionX_aléa,y2+positionY_aléa,fill='yellow')#bonus sur les 2 joueur
                    if can.coords(ball)[0]==can.coords(bonus2j)[0]:
                        a=a**10
                elif BouM==2:
                    malus=can.create_oval(x1+positionX_aléa,y1+positionY_aléa,x2+positionX_aléa,y2+positionY_aléa,fill='red')#malus 
                elif BouM==3:
                    bonus=can.create_oval(x1+positionX_aléa,y1+positionY_aléa,x2+positionX_aléa,y2+positionY_aléa,fill='green')#bonus


            fenetrej.after(2000,les_bonus)
            """

            #placements des composants
            can.grid(row=0,column=0,columnspan=3)
            w.grid(row=1,column=0)
            z.grid(row=1,column=2)
            btn_retour.grid(row=1,column=1)
            player_one=can.create_rectangle(50,200,130,400,fill=colorj1)
            player_two=can.create_rectangle(1070,200,1150,400,fill=colorj2)
            ball=can.create_oval(550,350,590,390,fill=colorball)

            #mouvement raquette player1
            def haut(event):
                if can.coords(player_one)[1]<0:
                    can.move(player_one,0,0)
                else:
                    can.move(player_one,0,-10)
            def bas(event):
                if can.coords(player_one)[3]>700:
                    can.move(player_one,0,0)
                else:
                    can.move(player_one,0,10)

            #mouvement raquette player2
            def haut_2(event):
                if can.coords(player_two)[1]<0:
                    can.move(player_two,0,0)
                else:
                    can.move(player_two,0,-10)
            def bas_2(event):
                if can.coords(player_two)[3]>700:
                    can.move(player_two,0,0)
                else:
                    can.move(player_two,0,10)

            #touche mouvements raquette
            can.bind_all('<z>', haut)
            can.bind_all('<s>', bas)
            can.bind_all('<Up>', haut_2)
            can.bind_all('<Down>', bas_2)



            #Mouvents et rebond de la balle
            while True:
                global bonus2j
                fenetrej.update_idletasks()
                fenetrej.update()
                time.sleep(0.01)
                can.move(ball,a,b)
                # si touche le bord droit
                if can.coords(ball)[2]>1200:
                    can.coords(ball, 550, 350, 590, 390)#repositionnement balle
                    #on redonne une direction aléatoire 
                    a=choice(A)+V
                    b=choice(B)
                    PJ1 += 1#on ajoute un point a J1
                    w.configure(text=PJ1)#on ecrit ce point
                    #fin du game si J1 gagne
                    if PJ1==WIN:
                        KIWIN=1
                        t1 = time.clock()#capture du temps en fin de jeu
                        T=time.strftime('%M:%S', time.gmtime(t1-t0))#calcul du temps de jeu
                        findugame()
                        fenetrej.destroy()
                #si touche le haut
                if can.coords(ball)[3]>700:
                    b=-1*b
                #si touche le bord gauche
                if can.coords(ball)[0]<0:
                    can.coords(ball, 550, 350, 590, 390)#repositionnement balle 
                    #on redonne une direction aléatoire
                    a=choice(A)+V
                    b=choice(B)
                    PJ2 += 1#on ajoute un point a J2
                    z.configure(text=PJ2)#on ecrit ce point
                    #fin du game si J2 gagne
                    if PJ2==WIN:
                        KIWIN=2
                        t1 = time.clock()#capture du temps en fin de jeu
                        T=time.strftime('%M:%S', time.gmtime(t1-t0))#calcul du temps de jeu
                        findugame()
                        fenetrej.destroy()
                if can.coords(ball)[1]<0:
                    b=-1*b


                #Les collisions
                #collissions des raquette
                if len(can.find_overlapping(can.coords(player_one)[0],can.coords(player_one)[1],can.coords(player_one)[2],can.coords(player_one)[3]))>1:
                    a=-1*a
                    a += 0.5
                if len(can.find_overlapping(can.coords(player_two)[0],can.coords(player_two)[1],can.coords(player_two)[2],can.coords(player_two)[3]))>1:
                    a=-1*a
                    a-=0.5

                #collisions des bonus/malus

                    #if len(can.find_overlapping(can.coords(malus)[0],can.coords(malus)[1],can.coords(malus)[2],can.coords(malus)[3]))>1:
                    #if len(can.find_overlapping(can.coords(bonus)[0],can.coords(bonus)[1],can.coords(bonus)[2],can.coords(player_one)[3]))>1:

            #bouce infnie
            fenetrej.mainloop()


        #bouton OK
        ok=Button(fenetreP,text='Ok',command=play,width=20)
        ok.grid(row=5,column=0)

        #fenetre de fin de game
        def findugame():
            global PJ1,PJ2,T,KIWIN#variables global nécessaire
            fenetreF=Toplevel()#création fenetre
            #Joueur a gagner?
            if KIWIN == 1:
                joueurwinnner=Label(fenetreF,text='JOUEUR 1')
            elif KIWIN == 2:
                joueurwinnner=Label(fenetreF,text='JOUEUR 2')
            gagner=Label(fenetreF,text='à gagner :)')
            #points joueur 1
            w=Label(fenetreF,text='points joueur 1:')
            scorej1=Label(fenetreF,text=PJ1)
            #points joueur 2
            z=Label(fenetreF,text='points joueur 2:')
            scorej2=Label(fenetreF,text=PJ2)
            # temps partie
            y=Label(fenetreF,text='la partie a durée')
            temps=Label(fenetreF,text=T)
            # les boutons
            rejoue=Button(fenetreF,text='REJOUER',command=parametre,width=20)
            retour=Button(fenetreF,text='retour menu',command=fenetreF.destroy,width=20)

            #placements des composants
            joueurwinnner.pack()
            gagner.pack()
            w.pack()
            scorej1.pack()
            z.pack()
            scorej2.pack()
            y.pack()
            temps.pack()
            rejoue.pack()
            retour.pack()


    #fenetre de menu principales
    fenetreM=Tk()#création fenetre maitre
    fenetreM.title('pong')#titre fenetre
    #les boutons
    btn_start=Button(fenetreM,text='start',width=20,command=parametre)
    btn_quitte=Button(fenetreM,text='quitter',width=20,command=fenetreM.destroy)

    #placements des composants
    btn_start.pack()
    btn_quitte.pack()

    #bouce infnie
    fenetreM.mainloop()
