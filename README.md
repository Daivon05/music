# music

import tkinter
from tkinter import filedialog
from tkinter import *
import pygame
import os
import tkinter

root = Tk()
root.title("Music Player")
root.geometry("500x300")

pygame.mixer.init()


menubar = Menu(root)
root.config(menu=menubar)

songs = []
current_song = ""
paused = False

def load_music():
    global current_song
    root.directory = filedialog.askdirectory()

    for song in os.listdir(root.directory):
        name, ext = os.path.splitext(song)
        if ext == '.m4p':
            songs.append(song)

    for song in songs:
        songlist.insert("end", song)

    songlist.selection_set(0)
    current_song = songs[songlist.curselection()[0]]

def play_pause_music():
    global current_song, paused

    if not paused:
        pygame.mixer.music.load(os.path.join(root.directory, current_song))
        pygame.mixer.music.play()
    else:
        pygame.mixer.music.unpause()
        paused = False

def next_music():
    global current_song, paused
    
    try:
        songlist.selection_clear(0, END)
        songlist.selection_set(songs.index(current_song) +1)
        current_song = songs[songlist.curselection()[0]]
        play_pause_music()
    except:
        pass
def prev_music():
    global current_song, paused

    try:
        songlist.selection_clear(0, END)
        songlist.selection_set(songs.index(current_song) - 1)
        current_song = songs[songlist.curselection()[0]]
        play_pause_music()
    except:
        pass

    
organize_menu = Menu(menubar)
organize_menu.add_command(label="Select Folder", command=load_music)
menubar.add_cascade(label="Organize", menu=organize_menu)

songlist = Listbox(root, bg="black", fg="white", width=200, height=15)
songlist.pack()


play_pause_btn_image=tkinter.PhotoImage(file="images/play_pause.png" )
next_btn_image=tkinter.PhotoImage(file="images/next.png")
prev_btn_image = tkinter.PhotoImage(file="images/previous.png")

control_frame = Frame(root)
control_frame.pack()

play_pause_btn = Button(control_frame, image=play_pause_btn_image, borderwidth=0, command=play_pause_music)
next_btn = Button(control_frame, image=next_btn_image, borderwidth=0, command=next_music)
prev_btn = Button(control_frame, image=prev_btn_image, borderwidth=0, command=prev_music)

play_pause_btn.grid(row=0, column=1, padx=7, pady=10)
next_btn.grid(row=0, column=2, padx=7, pady=10)
prev_btn.grid(row=0, column=0, padx=7, pady=10)


root.mainloop
