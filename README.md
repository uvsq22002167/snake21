# snake21

##################################
# groupe 3 MIASHS TD2
# Anya BOUBCHIR
# Judith FAUCON
# Sana MAAREF
# Nolwenn GAUTIER
# Jalis LACHEHAB
# Rosary NITHIYANANTHAM 
# 
######################################################### 


############################
# Importation des librairies

import tkinter as tk
import tkinter.font as tkFont
import random as rd

############################
# Constantes
H=420
L=600
couleur_pierre1 = "LightGoldenrod3"
couleur_pierre2 = "LightGoldenrod4"

############################
# Variables
dx, dy = 20, 0
serpent=[]
coords_serp=[] # coordonées d'une partie de serpent
coords_serpent=[] # coordonnées de toutes les parties du serpent
perdu=0
grandir=0
score=0
vit=0
fin=0
liste_score_petite=[] # liste  des couple Nom-Score petite vitesse
liste_score_moyenne=[] # liste  des couple Nom-Score moyenne vitesse
liste_score_grande=[] # liste  des couple Nom-Score grande vitesse

var_liste_score = 1 # variable pour najouter qu'une seule fois le Nom et le Score à la liste

############################
# Fonctions


def init_variables():
    """Réinitialise les variables pour rejouer"""
    global perdu,  grandir,  score, fin,  dx,  dy,  serpent,  coords_serpent,  coords_serp,  var_liste_score
    perdu = 0
    grandir = 0
    score=0
    fin=0
    var_liste_score = 1
    dx, dy = 20, 0
    serpent=[]
    coords_serp=[] # coordonées d'une seule partie du corps de serpent
    coords_serpent=[] # coordonnées de tout le corps du serpent
  

def fenetre_menu():
    """Crée la page du menu du jeu qui comprend pseudo, vitesse et illustration du jeu"""
    global b5, b3,  b4, vit,  cadre_menu,  police1,  police2,  nom_entr,  nom
    cadre_menu = tk.Frame(racine, bg="chartreuse3", width=L, height=H+30)   # utilisation de Frame pour créer un cadre 
    cadre_menu.grid()   # placement du cadre dans une grille
    police1=tkFont.Font(cadre_menu, size=15, family='Courier',weight = 'bold' )   # définition de la police de caractère (family (police), bold (caratère gras))
    police2=tkFont.Font(cadre_menu, size=10, family='Courier',weight = 'bold' )
    b3 = tk.Button(cadre_menu, text='Petite',  bg="chartreuse3" , fg="black", width=8, height=1, font=police1, borderwidth=4, command=lambda : vitesse(1),   relief="groove" )    # la fonction lambda envoie la valeur 1 comme argument à la fonction ' vitesse() '
    b3 .place(x = 400, y = 100)     # placement du bouton en fonction des coordonnées x et y
    b4 =tk.Button(cadre_menu,   text='Moyenne ',  bg="chartreuse3" , fg="black", width=8, height=1, font = police1, borderwidth=4, command=lambda : vitesse(2), relief="groove")
    b4.place(x = 400, y = 150)
    b5 =tk.Button(cadre_menu,  text='Grande', bg="chartreuse3", fg="black", width=8, height=1, font = police1, borderwidth=4, command=lambda : vitesse(3), relief="groove")
    b5.place(x = 400, y = 200)       
    lab1 = tk.Label( cadre_menu, text = "choississez", bg="chartreuse3", width=16, height=1, font=police2)
    lab1.place(x = 400, y = 50)
    lab2  = tk.Label( cadre_menu, text = "votre vitesse", bg="chartreuse3", width=16, height=1, font=police2)
    lab2.place(x = 400, y = 65)
    lab3 = tk.Label( cadre_menu, text = "Entrez votre", bg="chartreuse3", width=16,  height=1,font=police2)
    lab3.place(x = 70, y =60)
    lab3 = tk.Label( cadre_menu, text = "pseudo", bg="chartreuse3", width=16,  height=1,font=police2)
    lab3.place(x = 70, y =75)     
    nom_entr = tk.Entry( cadre_menu,  bd = 1,  bg ="chartreuse3" )   # champs de saisie pour le pseudo
    nom_entr.place(x =60, y =90)
   
    canvas2=tk.Canvas(cadre_menu, bg="chartreuse3", width=340, height=220)     # création d'une zone de dessin pour décorer la page menu
    canvas2.place(x =0, y =170)
    canvas2.create_rectangle(20, 120, 40, 220,  fill="LightGoldenrod3", outline="LightGoldenrod3")    # dessine un serpent et une pomme
    canvas2.create_rectangle(40, 120, 100, 140,  fill="LightGoldenrod3", outline="LightGoldenrod3")
    canvas2.create_rectangle(80, 140, 100, 180,  fill="LightGoldenrod3", outline="LightGoldenrod3")
    canvas2.create_rectangle(100, 160, 160, 180,  fill="LightGoldenrod3", outline="LightGoldenrod3")
    canvas2.create_rectangle(160, 180, 180, 40,  fill="LightGoldenrod3", outline="LightGoldenrod3")
    canvas2.create_rectangle(180, 40, 260, 60,  fill="LightGoldenrod3", outline="LightGoldenrod3")
    canvas2.create_rectangle(260, 40, 280, 60,  fill="Yellow", outline="Yellow")
    canvas2.create_oval(310, 45, 320, 55,  fill="red", outline="red")    


def vitesse(v):  
    """Permet de choisir la vitesse du serpent pour la partie (lent, moyen ou rapide)"""
    init_variables()    #  appelle de la fonction "init_variable" pour réinitialiser les différentes variables 
    b6 =tk.Button(cadre_menu,   text='Commencer',  bg="chartreuse3" , fg="black", width=8, height=1, font = police1, borderwidth=4, relief="groove", command=lambda : [pseudo(),  fenetre_jeu()])
    b6.place(x = 400, y = 350)     # création du bouton commencer qui appelle la fonction "pseudo()" pour enregister le nom du joueur et la fonction "fenêtre_jeu()"" qui détruira la fenêtre menu
    global vit,  b5
    if v==1:      # en fonction de l'argument passé à la fonction "vitesse(v)" la variable vit prend différentes valeurs
        vit = 320
        b3.configure(bg="yellow"), b4.configure(bg="chartreuse3"), b5.configure(bg="chartreuse3")     # change la couleur du bouton de la vitesse sélectionnée   
    elif v==2:
       vit= 250
       b4.configure(bg="yellow"), b3.configure(bg="chartreuse3"), b5.configure(bg="chartreuse3") 
    elif v==3:
      vit = 180
      b5.configure(bg="yellow"), b4.configure(bg="chartreuse3"), b3.configure(bg="chartreuse3")


def pseudo():
    """Enregistre le pseudo dans la variable nom"""
    global nom
    nom = nom_entr.get()


def fenetre_jeu():
    """Crée la page du jeu"""
    global canvas,  text_score,  cadre_jeu,  nom
    cadre_menu.destroy()     # détruit la fenêtre du menu (ou plus exactement détruit le Frame du menu)
    cadre_jeu = tk.Frame(racine, bg="chartreuse3", width=L, height=H+30)  # création d'un nouveau cadre (Frame) pour mettre les widgets du jeu
    cadre_jeu.grid()    
    canvas = tk.Canvas(cadre_jeu, bg="chartreuse3", width=L, height=H)
    canvas.grid(column=0, row=1,  columnspan=4)
    canvas1 = tk.Canvas(cadre_jeu, bg="chartreuse3", width=L, height=30)     
    canvas1.grid(column=0, row=0,  columnspan=4)
    b1 = tk.Button(cadre_jeu, text='Nouvelle Partie',command=lambda : choix_fin(1),  bg="white" , fg="black")    # la fonction lambda passe comme argument 1 à la fonction "choix_fin()"
    b1.grid(column=0,row=2)    
    b2 = tk.Button(cadre_jeu, text="Quitter", command=lambda : choix_fin(3), bg="white" , fg="black")
    b2.grid(column=2,row=2)
    b7 = tk.Button(cadre_jeu, text="Menu", command= lambda : choix_fin(2) ,  bg="white" , fg="black")
    b7.grid(column=1,row=2)    
    text_score = tk.Label(cadre_jeu, text="Score: "+str(score),  font=(20),  bg="chartreuse3")   # écrit le score dans le canvas
    text_score.grid(column=3,row=0)
    text_nom = tk.Label(cadre_jeu, text=str(nom),  font=(20),  bg="chartreuse3")   # écrit le pseudo dans le canvas
    text_nom.grid(column=0,row=0)
    # lance les fonction suivantes   
    creer_pierres()
    creer_serpent()
    creer_corps()
    creer_pomme()
    mouvement() 


def choix_fin(fin):
    """Propose plusieurs choix à la fin d'une partie (quitter le jeu, retourner au menu ou relancer une nouvelle partie"""
    global cadre_jeu,  racine,  perdu,  text2
    if fin ==3 :       # si choix 3 (bouton "Quitter"): détruit la racine du jeu
        racine.destroy()
    if fin==2:         # si choix 2 (bouton "Menu"): détruit le cadre de la fenêtre du jeu, réinitialise les variables et lance de nouveau la foncton "fenetre_menu()"
        cadre_jeu.destroy()
        init_variables()
        fenetre_menu()
    if fin==1:         # si choix 1 (bouton "Nouvelle Partie"): détruit le cadre  de la fenêtre de jeu, réinitialise les variable et relance la fonction "fenetre_jeu")
        cadre_jeu.destroy()
        init_variables()
        fenetre_jeu()      
        

def creer_pierres():
    """Crée le décor du jeu (bordure de pierres)"""
    x0,  y0,  x1,  y1 = 0,  0,  30,  30         # dimension des pierres
    for i in range (1,  L//30):     # boucle pour la création des pierres de la bordure du haut
        canvas.create_rectangle( x0,y0,x1 , y1, fill=couleur_pierre1)    # création du carré extérieur des pierres
        canvas.create_rectangle( x0+5,y0+5,x1 -5, y1-5, fill=couleur_pierre2, outline=couleur_pierre1, width=1)      # création de carré intérieur des pierres
        canvas.create_line(x0,y0,x1 , y1, fill=couleur_pierre2)  # création d'une ligne diagonale dans les pierres pour donner l'illusion de relief
        canvas.create_line(x0,y1,x1, y0, fill=couleur_pierre2)
        x0 = x0+30     # incrémente x pour dessiner la pierre suivante
        x1 = x1+30               
    for i in range (1,  H//30):    # boucle pour la création des pierres de la bordure de droite
        canvas.create_rectangle( x0,y0,x1 , y1, fill=couleur_pierre1)
        canvas.create_rectangle( x0+5,y0+5,x1 -5, y1-5, fill=couleur_pierre2, outline=couleur_pierre1, width=1)
        canvas.create_line(x0,y0,x1 , y1, fill=couleur_pierre2)
        canvas.create_line(x0,y1,x1, y0, fill=couleur_pierre2)
        y0 = y0+30
        y1 = y1+30          
    for i in range (1,  L//30):   # boucle pour la création des pierres de la bordure du bas
        canvas.create_rectangle( x0,y0,x1 , y1, fill=couleur_pierre1)
        canvas.create_rectangle( x0+5,y0+5,x1 -5, y1-5, fill=couleur_pierre2, outline=couleur_pierre1, width=1)
        canvas.create_line(x0,y0,x1 , y1, fill=couleur_pierre2)
        canvas.create_line(x0,y1,x1, y0, fill=couleur_pierre2)
        x0 = x0-30
        x1 = x1-30         
    for i in range (1,  H//30):   # boucle pour la création des pierres de la bordure de gauche
        canvas.create_rectangle( x0,y0,x1 , y1, fill=couleur_pierre1)
        canvas.create_rectangle( x0+5,y0+5,x1 -5, y1-5, fill=couleur_pierre2, outline=couleur_pierre1, width=1)
        canvas.create_line(x0,y0,x1 , y1, fill=couleur_pierre2)
        canvas.create_line(x0,y1,x1, y0, fill=couleur_pierre2)
        y0 = y0-30
        y1 = y1-30 
         

def creer_serpent():
    """Crée le serpent du départ (une tête et une partie de corps)"""
    global serpent
    x = L//2 -30               # x et y au centre du canvas (-30 pour que l'interaction avec les pierres se fasse sans dépassement du décor)
    y = H//2
    tete = canvas.create_rectangle( x-20,y,x, y-20, fill="yellow", outline="yellow")      # création de la tête au centre du canvas (carré de 20x20)
    serpent.append(tete)       # ajout de la tête à la liste serpent
    coords_serp=[ x-20,y,x, y-20]       # création du premier élément de son corps
    coords_serpent.append(coords_serp)     

  
def creer_corps():
    """Crée une partie du corps du serpent lorsque le serpent mange une pomme"""
    global serpent
    i = len(serpent)-1   # i corespond au nombre d'éléments de la liste du serpent  (-1 car dans une liste les éléments sont repérés à partir de 0)
    x0, y0, x1, y1 = canvas.coords(serpent[i])      # récupération des coordonnées de la dernière partie du serpent
    corps = canvas.create_rectangle(x0-20, y0, x1, y1-20,  fill="LightGoldenrod3", outline="LightGoldenrod3")      # crée une nouvelle partie de corps au serpent après les coordonnées de la dernière partie
    serpent.append(corps)   # ajoute le corps créé à la liste "serpent"
    x0, y0, x1, y1 = canvas.coords(corps)    # récupère les coordonnées de la nouvelle partie de corps 
    coords_serp=[ x0, y0, x1, y1]     # mise en place des coordonnées sous forme de liste
    coords_serpent.append(coords_serp)   # ajout des coordonnées à la liste des cordonnées des autres parties du serpent
  

def creer_pomme():
    """Fonction création d'une pomme""" 
    global pomme,  xp,  yp      # xp, yp coordonnées du centre de la pomme 
    xp = rd.randrange(50, L-50, 10)     # récupération d'une valeur aléatoire (méthode .ranrange) entre 50 et 600-50 avec un pas de 10
    yp = rd.randrange(50, H-50, 10)
    pomme =canvas.create_oval(xp-5, yp-5,  xp+5,  yp+5, fill = 'red' )   # création de la pomme sur le canvas avec les nouvelles coordonnées
    

def mouvement():
    """Met en mouvement le serpent"""
    global dx, dy,  perdu, grandir,  fin     
    i=len(serpent) -1   # i corespond au nombre d'éléments de la liste du serpent  (-1 car dans une liste les éléments sont repérés à partir de 0)
    # lance les fonctions suivantes pour vérifier que le serpent n'interagit pas avec le décor, lui-même ou la pomme 
    inter_pierre()
    inter_serpent()
    inter_pomme()
    
    if perdu == 0 :     # les actions suivantes ne sont réalisées que si la variable "perdu" est égale à 0 (pas d'interaction avec le décor ou lui-même)
        while i != 0:  
            coords_serpent[i]=coords_serpent[i-1]   # décale les coordonnées de chaque partie du serpent dans la liste  (en commençant par le dernier et l'avant dernier) 
            i=i-1      # decrémentation de l'indice i (décalage de l'avant dernier et de l'avant avant dernier, ect...)   
        canvas.move(serpent[0],dx,dy)    # déplace la tête du serpent ([0]) de dx, dy 
        x0, y0, x1, y1 = canvas.coords(serpent[0])  # récupère les nouvelles coordonées de la tête
        coords_serp=[ x0, y0, x1, y1]     # les place dans la liste "coord_serp"
        coords_serpent[0]=(coords_serp)         # remplace les coordonnées de la tête par les les nouvelles coordonées dans la liste des coordonnées des différentes parties du serpent
        for i in range (1,  len(coords_serpent) ) :    # boucle qui change les coordonées de toutes les partie du serpent (déplace le serpent)
            canvas.coords(serpent[i],coords_serpent[i][0], coords_serpent[i][1],coords_serpent[i][2], coords_serpent[i][3])            
    else:
        canvas.create_text((L//2-20, H//2),  text=" Game over",  fill="blue",  font="Arial 30 bold")   # si perdu: passe à 1 et inscritpion du texte GAME OVER       
    if grandir ==1:   
        creer_corps()
        grandir = 0
    
    canvas.after(vit, mouvement)    # recommence la foncton "mouvement()" selon le réglage de "vit" (exemple: vit=200, mouvement toutes les 200ms)
          
#Fonctions changeant les coordonées de déplacement  
def droite(event):
    """déplace le serpent vers la droite"""
    global dx, dy   # déplacement sur l'axe des x et y à chaque fin de after de la fonction  "mouvement()"
    dx =20
    dy=0   
def gauche(event):
    """déplace le serpent vers la gauche"""
    global dx, dy
    dx = -20
    dy=0      
def haut(event):
    """déplace le serpent vers le haut"""
    global dx, dy
    dx = 0
    dy=-20    
def bas(event):
    """déplace le serpent vers le bas"""
    global dx, dy
    dx = 0
    dy=20
    

def inter_pierre():
    
    global coords_serpent, perdu     # "coords_serpent [0][x]": le [0] repère de la tête du serpent dans la liste, [x] repère des coordonées (si [0]=x0, [1]=y0, [2]=x1, [3]=y1)
    if coords_serpent[0][0] <= 30 or coords_serpent[0][2] >=L-30 or coords_serpent[0][1] <= 30 or coords_serpent[0][3] >=H-30 :
        game_over()


def inter_serpent():
    """Met en interaction la tête du serpent avec son corps: 
    si la tête du serpent touche une autre partie de son corps, la partie est finie"""
    global coords_serpent, perdu
    i= len(coords_serpent)
    for i in range (1, i):     # les coordonnées commençant par [0] sont celles de la tête du serpent et celles commençant par [i] sont celles du reste de son corps
        if coords_serpent[0][0]< coords_serpent[i][2] and coords_serpent[0][2] > coords_serpent[i][0]:        # coordonnées en x
            if coords_serpent[0][1]< coords_serpent[i][3] and coords_serpent[0][3] > coords_serpent[i][1]:     # coordonnées en y
                game_over()                   
    

def inter_pomme():
    """Met en interaction le serpent avec la pomme: si le serpent mange une pomme,
     le corps grandit, la pomme disparait et une autre apparaît aléatoirement sur le canvas"""
    global grandir,  xp ,  xy
    xp, yp,  grandir     # xp et yp coordonnées du centre de la pomme sur le canvas
    if coords_serpent[0][0]< xp+10 and coords_serpent[0][2] > xp -10:     # x0=coords_serpent[0][0]  x1=coords_serpent[0][2] si 
        if coords_serpent[0][1]< yp+10 and coords_serpent[0][3] > yp -10:
            canvas.delete(pomme)
            creer_pomme()
            compter_score()
            grandir = 1  
            

def compter_score():
    """Compte et affiche le score"""
    global text_score,  score
    score = score+1             # ajoute +1 à la variable score
    str_score = "Score: " + str(score)       #  transforme la variable score en chaîne de caractères (pour qu'elle puisse être écrite en tant que texte dans tkinter)
    text_score["text"]=str_score        # change la variable text de txt_score de la fonction "fenetre_jeu()"


def game_over():
    """Affiche la fin de partie: le serpent s'arrête, inscrit le score dans le fichier"""
    global perdu, var_liste_score   
    perdu= 1          # passage de la variable perdu à 1, empèche la fonction "mouvement()"  d'exécuter les mouvements (arrête le serpent)
    while var_liste_score ==1:
        tableau_score()         # appelle la fonction tableau_score() pour inscrire le score de fin de partie
        var_liste_score = var_liste_score -1             # permet de n'écrire le score qu'une seule fois
        

def tableau_score():
    """Implémente les tableaux des scores en fonction de la vitesse"""
    global liste_score_petite, liste_score_moyenne, liste_score_grande
    if vit==320:
        liste_score_petite.append([nom, score])          # ajoute le nom et le score à la liste 
        liste_score_petite.sort(key=lambda x: x[1], reverse=True)       # classement de la liste par ordre décroissant des scores
    if vit==250:
        liste_score_moyenne.append([nom, score]) 
        liste_score_moyenne.sort(key=lambda x: x[1], reverse=True)
    if vit==180:
        liste_score_grande.append([nom, score])
        liste_score_grande.sort(key=lambda x: x[1], reverse=True)
        
    ecrire_fichier_score()
    

def ecrire_fichier_score():   
    """Ecrit à chaque fin de partie le nom et le score dans le fichier texte correspondant"""
    with open("Score_petit.txt",  "w") as score_pet:      # ouverture du fichier texte Score_petit en ecriture "w" en tant que score_pet
        i = len(liste_score_petite)    # i = longueur de la liste
        if i <= 10 :     # il ne doit pas y avoir plus de 10 scores dans le fichier texte (si la longueur de i est inférieure à 10, la valeur de i reste à i)
            i = i
        else :     # (si la valeur de i est plus grande que 10, on l'impose à 10)
            i = 10        
        for i in range (0, i):
            score_pet.write(liste_score_petite[i][0] +":")         # écriture du nom (remarque l'ouverture et l'écriture dans un fichier texte, efface le contenu précédent du fichier)
            score_pet.write(str(liste_score_petite[i][1]) +"\n")        # écriture du score et retour à la ligne (\n) pour l'écriture du nom suivant    
    with open("Score_moyen.txt",  "w") as score_moy:
        i = len(liste_score_moyenne)
        if i <= 10 :
            i = i
        else :
            i = 10     
        for i in range (0, i):
            score_moy.write(liste_score_moyenne[i][0] +":") 
            score_moy.write(str(liste_score_moyenne[i][1]) +"\n") 
    with open("Score_grand.txt",  "w") as score_gra:
        i = len(liste_score_grande)
        if i <= 10 :
            i = i
        else :
            i = 10     
        for i in range (0, i):
            score_gra.write(liste_score_grande[i][0] +":") 
            score_gra.write(str(liste_score_grande[i][1]) +"\n")


def lire_fichier_score():    
    """Permet de lire les fichiers textes score pour remplir les liste_score_petite, moyenne, grande"""
    try:  # essai d'ouvir le fichier Score_petit en lecture (s'il existe, les instructions suivantes seront exécutées, sinon il y aura une IOError)
        # with open permet de d'ouvrir le fichier, l'avantage c'est qu'il se fermera automatiquement à la fin du with (indentation)
        with open("Score_petit.txt",  "r") as score_pet:        # ouverture du fichier Score_petit en lecture "r"
            for ligne in score_pet:      
                liste=ligne.split(":")        # récuperation de chaque chaînes de caractères qui sont séparées par (":") (pour chaque ligne du fichier)
                nom =(liste[0])                # la première chaîne de caractères (liste[0]) est mise dans la variable nom
                score=int(liste[1])           # la deuxième chaîne de caractère (liste[1]) est mise dans la variable score sous forme d'un entier (int)
                liste_score_petite.append([nom,score])          # ajoute nom et score à liste_score_petite
    except IOError:     # si le fichier n'existe pas, il y aura une IOError, cette ligne permet au programme de ne pas s'interrompre et exécute la ligne suivante
        with open("Score_petit.txt",  "w") as score_pet: pass    # ouverture du fichier Score_petit en écriture (ouverture en écriture = création de fichier): pass = pas d'action exécutée en suite
        
    try:
        with open("Score_moyen.txt",  "r") as score_moy:
            for ligne in score_moy:
                liste=ligne.split(":")
                nom =(liste[0])
                score=int(liste[1])
                liste_score_moyenne.append([nom,score])      
    except IOError:
        with open("Score_moyen.txt",  "w") as score_pet: pass        
    try:
        with open("Score_grand.txt",  "r") as score_gra:
            for ligne in score_gra:
                liste=ligne.split(":")
                nom =(liste[0])
                score=int(liste[1])
                liste_score_grande.append([nom,score])
    except IOError:
        with open("Score_grand.txt",  "w") as score_pet: pass      
   

############################   
# création de la racine 
racine = tk.Tk()
racine.title("Snake")
racine.config(bg ="chartreuse3")

# Liaison des événements
racine.bind('<KeyPress-Right>', droite)
racine.bind('<KeyPress-Left>', gauche)
racine.bind('<KeyPress-Up>' , haut)
racine.bind('<KeyPress-Down>', bas)

# Lancement du menu
fenetre_menu()

# Récupération des anciens scores dans les fichiers textes
lire_fichier_score()

########################

# boucle principale
racine.mainloop()
