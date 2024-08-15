# hello-world
This repository is for practicing the GitHub Flow.
zxc



# Team members Dylan,Carlos,Esther
# 08-15-24
#chat app that works from a browser

#importing libraries
#grabs the date and time
import logging
from datetime import datetime
#used when defining message
from typing import List,Tuple
#generates random user id 
from uuid import uuid4
# generates ui elements for the app like textbox and buttons
from nicegui import ui

logging.basicConfig(level=logging.INFO)


#define messages
messages: List[Tuple[str,str,str,str]] = []

@ui.refreshable
def chat_messages(own_id:str) -> None:
    if messages:
        for user_id, avatar, text, stamp in messages:
            ui.chat_message(text=text, stamp=stamp, avatar=avatar,sent = own_id == user_id)
    else:
        ui.label("No message yet").classes('mx-auto my-36')
    ui.run_javascript("window.scrollTo(0, document.body.scrollHeight)")


#building window for chat app
@ui.page('/')
async def main():
    def send() -> None:
        stamp = datetime.utcnow().strftime("%X")
        messages.append((user_id,avatar,text.value,stamp))
        text.value = ""
        chat_messages.refresh()
    user_id = str(uuid4())
    avatar = f"https://robohash.org/{user_id}?bgset=bg2"
    
    ui.add_css(r'a:link,a:visited{color: inherit !important: text-decoration: none; font-weight: 500}')
    with ui.footer().classes("bg-white"), ui.column().classes("w-full max-w-3xl mx-auto ny-6"):
        with ui.row().classes("w-full no-wrap items-center"):
            with ui.avatar().on("click",lambda: ui.navigate.to(main)):
                ui.image(avatar)
            text = ui.input(placeholder = "message").on("keydown.enter",send) \
                .props("rounded outline input-class = mx-3").classes("flex-row")

    await ui.context.client.connected()
    with ui.column().classes("w-full max-w-2xl mx-auto items-stretch"):
        chat_messages(user_id) 


if __name__ in {'__main__'} :
    ui.run
logging.info
