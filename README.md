import tkinter as tk
from tkinter import messagebox
import random


class tictactoe:
    def __init__(self,root):
        self.root = root
        self.score_valp = 0
        self.score_valc = 0
        self.label = tk.Label(root,text="TIC TAC TOE", font=("arial",30,"bold"))
        self.label.pack(side="top",padx=10,pady=10)
        self.mainframe = tk.Frame(root, bg="gray")
        self.mainframe.pack(padx=30, pady=30, )
        self.mainframe.pack_propagate(False) 
        self.usedbtn = {}
        self.com = "O"
        self.player = "X"
        self.turn = self.player
        self.found = False
        self.buttons = {}
        for row in range(3):
            for col in range(3):
                self.btn = tk.Button(self.mainframe, text="", font=("Arial", 30), width=5, height=2,command=lambda r=row, c=col: self.onclick(r, c))
                self.btn.grid(row=row, column=col, padx=5, pady=5)
                self.btn.grid_propagate(False) 
                self.buttons[(row, col)] = self.btn
            # print(self.buttons)
        self.playagain = tk.Button(root, width=8,height=0 , text="Play Again", font=("arial",17), bg="green", command=lambda:self.tryagain())
        self.playagain.place(x=165,y=520)
        self.startbtn = tk.Button(root, width=8,height=0 , text="Start", font=("arial",17), bg="yellow", command=lambda:self.newgame())
        self.startbtn.place(x=315,y=520)
        self.score_com = tk.Label(root, text="0",font=("arial",24), height=2,width=2, bg="red")
        self.score_com.place(x=525,y=250)
        self.score_player = tk.Label(root, text="0",font=("arial",24), height=2,width=2, bg="blue")
        self.score_player.place(x=10, y=250)
    def tryagain(self):
        for btn in self.buttons.values():
            btn["text"]=""

        self.usedbtn = {}
            #   print(btn)
        self.enablebtn()
        self.found = False
        
    def newgame(self):
        self.tryagain()
        self.score_player["text"] = ""
        self.score_com["text"] = ""
        self.score_valp=0
        self.score_valc=0
        pass
  

    def onclick(self,r,c):
        self.row = r
        self.col = c
        self.btn = self.buttons[(self.row,self.col)]

        if self.btn["text"]  == "" :
            self.btn.config(text= self.player)
            self.usedbtn[(self.row,self.col)] = self.player
            self.turn = self.com
            
            self.check()
            self.gameloop()
        else:
             return
     
    def gameloop(self):
        if len(self.usedbtn)<=9 and self.found == False:
            self.empty = {}
            for i in range(3):
                for j in range(3):
                    if self.buttons[(i,j)]["text"]=="":
                        self.empty[(i)] = j
            self.rowcol = random.choice(list(self.empty.items()))
            self.btn = self.buttons[(self.rowcol)]
            self.btn.config(text=self.com)
            self.usedbtn[(self.rowcol)] = self.com
            self.turn = self.player
            self.check()
              
            
    def check(self):
        self.pairs = {1: [(0, 0), (0, 1), (0, 2)],
                         2:[(1, 0), (1, 1), (1, 2)],
                         3:[(2, 0), (2, 1), (2, 2)],
                         4:[(0, 0), (1, 0), (2, 0)],
                         5:[(0, 1), (1, 1), (2, 1)],
                         6:[(0, 2), (1, 2), (2, 2)],
                         7:[(0, 0), (1, 1), (2, 2)], 
                         8:[(0, 2), (1, 1), (2, 0)]}
        
        self.found = False 
        if len(self.usedbtn)<=9:
            for value in self.pairs.values(): 
                if self.buttons[value[0]]["text"] == self.player and self.buttons[value[1]]["text"] ==self.player  and self.buttons[value[2]]["text"]== self.player:
                            messagebox.showinfo("Game over","Player wins") 
                            self.disablebtn()
                            self.found = True
                            self.score_valp += 1
                            self.score_player["text"] = str(self.score_valp) 
                            return 
                elif self.buttons[value[0]]["text"]== self.com and self.buttons[value[1]]["text"]==self.com  and self.buttons[value[2]]["text"]  == self.com :
                            messagebox.showinfo("Game over","computer wins")
                            self.disablebtn()
                            self.found = True 
                            self.score_valc+=1
                            self.score_com["text"]= str(self.score_valc)
                            return 
            
        if len(self.usedbtn)==9 and self.found == False: 
                messagebox.showinfo("GAME OVER", "DRAW")
                self.disablebtn() 
                
    def disablebtn(self):
         for btn in self.buttons.values():
              btn["state"] = "disabled"
    def enablebtn(self):
         for btn in self.buttons.values():
              btn["state"] = "normal"
            #   print(self.btn)
        



root = tk.Tk()
tictactoe(root)
root.geometry("600x600")
root.mainloop()
        
