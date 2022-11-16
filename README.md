# Web_Scraping
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

PATH = "C:\chromedriver.exe" #specify path of the file
browser = webdriver.Chrome(PATH)

def web_scrape(url):

    browser.get(url)

    try:
        stock = WebDriverWait(browser, 50).until(
            EC.presence_of_element_located((By.XPATH, "/html/body/div[2]/main/div/div[1]/div[2]/div[1]/div/div[1]/div/div/span"))
        )

        unitcost = WebDriverWait(browser, 50).until(
            EC.presence_of_element_located((By.XPATH, "/html/body/div[2]/main/div/div[1]/div[2]/div/div/div[4]/span[1]/table/tbody/tr[1]/td[3]"))
        )

        return [stock.text[:-9],unitcost.text[2:]]
    

    finally:
        browser.quit()

web_scrape_out = web_scrape("https://www.digikey.in/en/products/detail/cts-resistor-products/73M1R010F/1543431")
print(web_scrape_out[0])
print(web_scrape_out[1])

#Exporting Data to CSV
filename="Products.csv"
f=open(filename,"w",encoding="utf-8")
headers="Instock,Unit_cost\n"
f.write(headers)
f.write( web_scrape_out[0].replace(',','') + "," + web_scrape_out[1].replace(',','')  + "\n")
f.close()



    
