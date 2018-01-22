from Tkinter import *
import ttk
import sqlite3
#from pygame import mixer
import webbrowser
#import speech_recognition as sr
con=sqlite3.Connection('hrdb')
cur=con.cursor()
cur.execute('create table if not exists recent5(name varchar(20),site varchar(10))')
root=Tk()
root.title('Power Browser')
root.iconbitmap('mic.ico')
label1=ttk.Label(root,text='Query').grid(row=0,column=0)
entry1=ttk.Entry(root,width=40)
entry1.grid(row=0,column=1)
btn2=StringVar()

def callback():

    if(btn2.get()=='google'):
       webbrowser.open('http://google.com/search?q='+entry1.get())
    elif btn2.get()=='amazon':
        webbrowser.open('http://amazon.com/s/?url=search-alias%3Dstripbooks&field-keywords='+entry1.get())
    elif btn2.get()=='youtube':
        webbrowser.open('http://www.youtube.com/result?search_query='+entry1.get())
    else:
        pass
    n=[btn2.get(),entry1.get()]
    cur.execute("insert into recent5 values(?,?)",n)
    con.commit()
    entry1.delete(0,END)
def get(event):

    if btn2.get()=='google':
       webbrowser.open('http://google.com/search?q='+entry1.get())
    elif btn2.get()=='amazon':
       webbrowser.open('http://amazon.com/s/?url=search-alias%3Dstripbooks&field-keywords='+entry1.get())
    elif btn2.get()=='youtube':
        webbrowser.open('http://www.youtube.com/result?search_query='+entry1.get())
    else:
        pass
    n=[btn2.get(),entry1.get()]
    cur.execute("insert into recent5 values(?,?)",n)
    con.commit()
    entry1.delete(0,END)
def click():
   # mixer.init()
   #  mixer.music.load('note.mp3')
   # mixer.music.play()
    r=sr.Recognizer()
    with sr.Microphone() as source:
        try:
            audio=r.listen(source,timeout=5)
            message=str(r.recognize_google(audio,key="GOOGLE_SPEECH_RECOGNITION_API_KEY"))
            entry1.focus()
            entry1.delete(0,END)
            entry1.insert(0,message)
            if(btn2.get()=='google'):
                webbrowser.open('http://google.com/search?q='+message)
            elif btn2.get()=='amazon':
                webbrowser.open('http://amazon.com/s/?url=search-alias%3Dstripbooks&field-keywords='+message)
            elif btn2.get()=='youtube':
                webbrowser.open('http://www.youtube.com/result?search_query='+message)
            else:
                pass
        except sr.UnknownValueError:
            print('Sorry!! Not Able To Recognize')
        else:
            pass
        n=[btn2.get(),entry1.get()]
        cur.execute("insert into recent5 values(?,?)",n)
        con.commit()
        entry1.delete(0,END)
def view():
    cur.execute('select * from recent5')
    #cur.execute('delete from recent5')
    l=cur.fetchall()
    label5=ttk.Label(root,text=l).grid(row=3,column=1)
    #print (l)
entry1.bind("<Return>",get)

MyButton1 =ttk.Button(root,text='Search',width=10,command=callback)
MyButton1.grid(row=0,column=6)

MyButton2=ttk.Radiobutton(root,text='Google',value='google',variable=btn2)
MyButton2.grid(row=1,column=1,sticky=W)
MyButton3=ttk.Radiobutton(root,text='Amazon',value='amazon',variable=btn2)
MyButton3.grid(row=1,column=1)
MyButton4=ttk.Radiobutton(root,text='YouTube',value='youtube',variable=btn2)
MyButton4.grid(row=1,column=3,sticky=E)
MyButton5=Button(root,text='MicroPhone',command=click)
MyButton5.grid(row=0,column=5)

MyButton6=Button(root,text='History',command=view)
MyButton6.grid(row=2,column=0)
entry1.focus()
root.wm_attributes('-topmost',1)
root.mainloop()
    

