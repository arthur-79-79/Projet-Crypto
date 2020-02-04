import gspread
import requests
from oauth2client.service_account import ServiceAccountCredentials
import json

#ouverture de la google sheet
scope = ['https://spreadsheets.google.com/feeds']
credentials = ServiceAccountCredentials.from_json_keyfile_name('projet4-265217-1deda3d78cf9.json', scope)

gc = gspread.authorize(credentials)

sheet = gc.open_by_url('https://docs.google.com/spreadsheets/d/1MdiBgKwQ5-XcmBkicPIhOnHQhv_AVU4P6DipoAPkQ00/edit?ts=5e1a8a8c#gid=0').sheet1

#on va chercher les données sur crytocompare
response = requests.get("https://min-api.cryptocompare.com/data/price?fsym=BTC&tsyms=BTC,USD,CAD,GBP,EUR,NOK,PLN,"
                        "TRY,RUB,ROM,UAH,CZK,HRK,HUF,CHF,SEK,DKK,BCH,GEL,JPY,KRW,BRL,VEF,ARS,MXN,COP,CLB,PEN,INR,CNY,SGD,HKD,PHP,THB,"
                        "MYR,IDR,TWD,KHR,PGK,VND,AUS,NZD,PKR,SAR,AED,KES,ZAR,MUR,NGN,TZR,?/?fysm=10&tysms=500")
headers={
    "X-RapidAPI-Host": " min-api.cryptocompare.com/",
    "X-RapidAPI-Key": " f3423360d5e8cb8b577daf0b25daca0ea772b5692804867bead3e5e4e9085f26"
  }
print(response.json())
def jprint(obj):
    # create a formatted string of the Python JSON object
    text = json.dumps(obj, sort_keys=True, indent=4)
    print(text)
jprint(response.json())


sheet.update_cell(1,2, "Average monthly Bitcoin trading volume (BTC)")
sheet.update_cell(1,3, "GDP (USD Last Year)")
sheet.update_cell(1,4, "Total numbers of users")
sheet.update_cell(1,5, "Total Population")
sheet.update_cell(1,6, "Penetration rate")

list = ["United States","Canada","United Kingdom","Europe (30 countries)","Norway","Poland","Turkey","Russia","Romania","Ukraine","Czech Republic",
        "Croatia","Hungary","Switzerland","Sweden","Denmark","Bulgarie","Géorgie","Japan","South Korea","Brazil","Venezuela","Argentina","Mexico",
        "Colombia","Chile","Peru","India","China","Singapore","Hong Kong","Philippines","Thailand","Malaysia","Indonesia","Taiwan","Cambodia","New Guinea",
        "Vietnam","Australia","New Zealand","Pakistan","Saudi Arabia","United Arab Emirates","Kenya","South Africa","Mauritius","Nigeria","Tanzania"]

#Remplis plusieurs cellule avec une boucle (pour les pays)
val = sheet.range('A2:A50')
i=0
for cell in val:
    cell.value = list[i]
    i = i + 1
sheet.update_cells(val)
#pour les données de cryptocompare
"""Problème pour mettre les données de cryptocompare sur la googlesheet
val = sheet.range('B2:B50')
i=0
for cell in val:
    cell.value = text[i]
    i = i + 1
sheet.update_cells(val)"""
