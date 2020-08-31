# Amazon-Price-Tracker
Tracks Amazon Prices


#%%
import requests
from bs4 import BeautifulSoup
import smtplib
import time
#%% 
URL = 'https://www.amazon.nl/Apple-MacBook-13-inch-SSD-opslag-Keyboard/dp/B0886YDRSK/ref=sr_1_3?__mk_nl_NL=%C3%85M%C3%85%C5%BD%C3%95%C3%91&dchild=1&keywords=macbook%2Bpro&qid=1598777977&sr=8-3&th=1'

headers = {'User-Agent': 'Mozilla/5.0 (Macintosh: Intel Mac OS X 10_15_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/84.0.4147.135 Safari/537.36'}

def check_price(): 
    page = requests.get(URL, headers=headers)

    soup = BeautifulSoup(page.content, 'lxml')

    title = soup.find(id = 'productTitle').get_text()

    price = soup.find(id ="priceblock_ourprice").get_text()

    converted_price = float(price[2:-3])

    if(converted_price < 1.500):
        send_mail()
    if(converted_price > 1.700):
        send_mail()
#Establish our connection with Google's Gmail connection
def send_mail():
    server = smtplib.SMTP('smtp.gmail.com', 587)
    server.ehlo() #Extends a connection between a connecting email and the connected email
    server.starttls() #encrypts our connection
    server.ehlo()
    server.login('your@email.com','your2wayauthentication')

    subject = 'Price fell down'
    body = 'Check the Amazon link, your product price has dropped -  https://www.amazon.nl/Apple-MacBook-13-inch-SSD-opslag-Keyboard/dp/B0886YDRSK/ref=sr_1_3?__mk_nl_NL=%C3%85M%C3%85%C5%BD%C3%95%C3%91&dchild=1&keywords=macbook%2Bpro&qid=1598777977&sr=8-3&th=1'

    msg = f'Subject: {subject}\n\n{body}'

    server.sendmail('martynas.lape@gmail.com', 'martynas.lape@gmail.com', msg)

    print('E-mail has been sent')

    server.quit()

while(True):
    check_price()
    time.sleep(36000)
