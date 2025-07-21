import flet as ft
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys
import time


def main(page:ft.Page):
    page.vertical_alignment ='center'
    page.horizontal_alignment ='center'
    
    def iniciar(e):
        navegador = webdriver.Chrome()
        navegador.get('https://www.google.com/?safe=active&ssui=on')
        time.sleep(1)
        
        input_pesquisar= navegador.find_element(By.XPATH,"//textarea[@title='Pesquisar']")
        input_pesquisar.send_keys(f"{input_cidade.value} temperatura")
        time.sleep(0.5)
        input_pesquisar.send_keys(Keys.ENTER)
        time.sleep(2)
        
        
        temperatura_google= navegador.find_element(By.ID,"wob_tm")
        temperatura_c.value = f' Temperatura Atual {input_cidade.value} {temperatura_google.text}°C'
        
        page.update()
                
        navegador.close()
    
    input_cidade = ft.CupertinoTextField(
        placeholder_text= 'Cidade',
        width= 200,
        bgcolor= 'transparent',
        prefix=ft.Icon(name=ft.icons.CLOUD,color='white')
        
    )
    
    btn_iniciar = ft.CupertinoButton(
        text= 'Iniciar',
        bgcolor= 'green',
        color= 'white',
        width= 200,
        on_click= iniciar
    )
    
    temperatura_text = ft.Text(
        value= f'{input_cidade.value}',
        color='white',
        size= 24
    )
    
    temperatura_c = ft.Text(
        color='white',
        size= 20
    )
    
    page.add(
        input_cidade,
        btn_iniciar,
        ft.Row(
            alignment='center',
            controls=[
                temperatura_text,
                temperatura_c
            ]
        ),
        
    )
    
ft.app(target=main)

