import tkinter as tk
from tkinter import *
from tkinter import filedialog, messagebox
import os

def createWidgets():
    global textArea
    textArea = Text(root, wrap=WORD)
    textArea.grid(sticky=N + E + S + W)

    menuBar = Menu(root)
    root.config(menu=menuBar)

    fileMenu = Menu(menuBar, tearoff=0)
    fileMenu.add_command(label="New", command=newFile)
    fileMenu.add_command(label="Open", command=openFile)
    fileMenu.add_command(label="Save", command=saveFile)
    fileMenu.add_separator()
    fileMenu.add_command(label="Exit", command=exitApp)
    menuBar.add_cascade(label="File", menu=fileMenu)

    editMenu = Menu(menuBar, tearoff=0)
    editMenu.add_command(label="Cut", command=cutText)
    editMenu.add_command(label="Copy", command=copyText)
    editMenu.add_command(label="Paste", command=pasteText)
    menuBar.add_cascade(label="Edit", menu=editMenu)

    helpMenu = Menu(menuBar, tearoff=0)
    helpMenu.add_command(label="About Notepad", command=showAbout)
    menuBar.add_cascade(label="Help", menu=helpMenu)

    root.grid_rowconfigure(0, weight=1)
    root.grid_columnconfigure(0, weight=1)


def newFile():
    global file
    file = None
    textArea.delete(1.0, END)
    root.title("Untitled - Notepad")

def openFile():
    global file
    file = filedialog.askopenfilename(defaultextension=".txt", filetypes=[("Text Documents", "*.txt"), ("All Files", "*.*")])
    if file:
        with open(file, "r") as f:
            textArea.delete(1.0, END)
            textArea.insert(1.0, f.read())
        root.title(os.path.basename(file) + " - Notepad")

def saveFile():
    global file
    if file is None:
        file = filedialog.asksaveasfilename(defaultextension=".txt", filetypes=[("Text Documents", "*.txt"), ("All Files", "*.*")])
    if file:
        with open(file, 'w') as f:
            f.write(textArea.get(1.0, END))
        root.title(os.path.basename(file) + " - Notepad")

def exitApp():
    root.destroy()

def cutText():
    textArea.event_generate("<<Cut>>")

def copyText():
    textArea.event_generate("<<Copy>>")

def pasteText():
    textArea.event_generate("<<Paste>>")

def showAbout():
    messagebox.showinfo("About Notepad", "This is a simple text Notepad.")

root = tk.Tk()
root.title("Untitled - Notepad")
file = None

createWidgets()
root.mainloop()
