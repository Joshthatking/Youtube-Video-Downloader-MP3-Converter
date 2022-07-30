# Youtube-Video-Downloader-MP3-Converter
Use a Youtube link to download a video or audio file of your choice
###

import os
from tkinter import *
import tkinter as tk
from tkinter import filedialog
import moviepy as myp
from moviepy import *
from moviepy.editor import VideoFileClip
from moviepy.editor import AudioFileClip
from moviepy.editor import *
from pytube import YouTube
import shutil



# Function (1)

def pathx():
    # select path
    path = filedialog.askdirectory()
    path_screen.config(text=path)
    
# Download Audio & Video
def downloadx():
    # get path
    linking = link_screen.get()
    # select
    user_path = path_screen.cget('text')
    screen.title('Downloading...')
    
# Download 
    mp4 = YouTube(linking).streams.get_highest_resolution().download()
    vid = VideoFileClip(mp4)
    audio = vid.audio
    #names audio title of Video
    audio.write_audiofile(mp4 + '.mp3')
    mp3 = os.path.abspath(mp4 + '.mp3')
    audio.close()
    vid.close()
    shutil.move(mp4, user_path)
    shutil.move(mp3,user_path)
    screen.title('Download Complete!')
    
    
# UI Screen Development
screen = Tk()
title_tk = screen.title('YouTube Download')
canvas = Canvas(screen, width = 500, height = 500)
canvas.pack()


# Link & Labels
link_screen = Entry(screen,width = 50)
link_label = Label(screen,text = 'Enter Link: ')

canvas.create_window(250, 200, window=link_label)
canvas.create_window(250, 220, window=link_screen)

# Save Path
path_screen = Label(screen, text='Select Download Path')
selecting = Button(screen, text='Select Folder', command=pathx)

# Window
canvas.create_window(250, 300, window=path_screen)
canvas.create_window(250, 330, window=selecting)
    
# Download Button
downloading = Button(screen, text='Download', command=downloadx)
canvas.create_window(250, 390, window=downloading)


screen.mainloop()

