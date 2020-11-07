from tkinter import *
from tkinter import messagebox 
import requests

url  = "https://api.openweathermap.org/data/2.5/weather?appid=4c22d6872257394dab6f68e9242a1470&q={}"

def getweather(city):
    json_data = requests.get(url.format(city)).json()
    temp_far = json_data['main']['temp']-273.15             #tempratur
    temp_cel=  (json_data['main']['temp']-273.15)*9/5+32
    main = json_data['weather'][0]['main']       #enviourment condition
    cont = json_data['sys']['country']  
    icon = json_data['weather'][0]['icon'] 
    name = json_data['name']
    print(json_data)
    final=(temp_far,temp_cel,main,name,cont)
    return final

def search():
    city = city_text.get()
    weather = getweather(city)
    if weather:
        location_lbl['text']='{}   {}'.format(weather[3],weather[4])
        temprature_l['text']='{:.2f} C,   {:.2f} F'.format(weather[0],weather[1])
        weather_l['text']='{}'.format(weather[2])
    else:
        messagebox.error("{} city cannnot found".format(city))        
        

app = Tk()
app.title("Weather application")
app.geometry('700x350')

city_text = StringVar()
city_entry = Entry(app,textvariable=city_text)
city_entry.pack()

search_btn=Button(app,text='search weather',width=20, command=search)
search_btn.pack()

location_lbl = Label(app,text="",font=('bold',20))
location_lbl.pack()

temprature_l = Label(app,text='')
temprature_l.pack()

weather_l = Label(app,text='')
weather_l.pack()




app.mainloop()
