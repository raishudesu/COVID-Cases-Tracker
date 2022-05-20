# COVID-Cases-Tracker
import tkinter as tk
import requests

HEIGHT = 500
WIDTH = 600
font = 'Arial', 18
LIGHT_ORANGE= '#fff0e6'
LIGHT_ORANGE_1 = '#ffd1b3'

#add the country name after /countries/

api = 'https://corona.lmao.ninja/v2/countries/'


def format_response(cases):
    try:
        country = cases['country']
        cases1 = cases['cases']
        deaths = cases['deaths']
        recpat = cases['recovered']
        actv = cases['active']

        final_str = 'Country: %s \nCases: %s \nDeaths: %s \nRecovered: %s \nActive Cases: %s' % (country, cases1, deaths, recpat, actv)
    except:
        final_str = 'Invalid Country name or Country Code'

    return final_str


def get_cases(country):
    url = api + country
    response = requests.get(url)
    cases = response.json()

    label1['text'] = format_response(cases)

    #print('Country: ', cases['country'])
    #print('Number of Cases: ', cases['cases'])
    #print('Number of Deaths: ', cases['deaths'])
    #print('Number of Recovered Patients', cases['recovered'])
    #print('Number of active cases: ', cases['active'])


app = tk.Tk()
app.title("CCTracker")

canvas = tk.Canvas(app, height=HEIGHT, width=WIDTH)
canvas.pack()

bg_image = tk.PhotoImage(file='fusion-medical-animation-rnr8D3FNUNY-unsplash.png')
bg_label = tk.Label(app, image=bg_image)
bg_label.place(relwidth=1, relheight=1)

label = tk.Label(app, text='COVID Cases Tracker', bg='#ffb3cc', font=('arial', 12))
label.place(relx=0.5, relwidth=0.75, relheight=0.1, anchor='n')

frame = tk.Frame(app, bg='#ffe6ee', bd=5)
frame.place(relx=0.5, rely=0.15, relwidth=0.75, relheight=0.1, anchor='n')

entry = tk.Entry(frame, font=12)
entry.place(relwidth=0.65, relheight=1)

button = tk.Button(frame, text="Track", font=('arial', 12), command=lambda: get_cases(entry.get()))
button.place(relx=0.7, relwidth=0.3, relheight=1)

lower_frame = tk.Frame(app, bg='#ffe6ee', bd=10)
lower_frame.place(relx=0.5, rely=0.25, relwidth=0.75, relheight=0.6, anchor='n')

label1 = tk.Label(lower_frame, bg='#ffb3cc', font=('arial', 18))
label1.place(relwidth=1, relheight=1)

app.mainloop()
