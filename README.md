# Alert Bot Celestia Light Node By Demigohu

### Guide for use Alert Bot


Jika ada error pada logs light node akan dikirimkan pesan ke telegram anda setiap 1 jam sekali


## jika tidak ada error 

![image](https://github.com/demigohu/alertbotcelestia/assets/46390405/7c9cc9b9-6796-4913-9a59-525348a52e04)


Step For Installation


```bash 
pip install requests 
pip install telegram-send
pip install python-telegram-bot==11.1.0
apt install cron
```

## bikin file alert.py

```bash
nano alert.py
```

## copy program ini dan pastekan ke alert.py

``` bash
import subprocess
import requests
import time

TOKEN = '5955033657:AAEncyC9ucfqTeVDTgzPA-lSse4cYBA_9_Q'
CHAT_ID = 'TOKEN_ID TELGRAM ANDA'

def send_telegram_message(msg):
    requests.post(f'https://api.telegram.org/bot{TOKEN}/sendMessage?chat_id={CHAT_ID}&text={msg}')

while True:
    try:
        logs = subprocess.check_output(['journalctl', '-u', 'celestia-lightd.service', '--since', '1 minute ago', '--no pager']).decode('utf-8')
        if "ERROR" in logs:
            send_telegram_message("Error ditemukan di log Celestia:\n" + logs)
        else:
            send_telegram_message("Tidak ada error di log Celestia dalam 1 menit terakhir")
        time.sleep(10)
    except Exception as e:
        send_telegram_message(f"Error saat menjalankan program: {str(e)}")
        
```

## setelah itu konfigurasi crontab

```bash
crontab -e
``` 
## tambahkan baris berikut di akhir file 

```bash
0 * * * * /path/to/your/command
```

## Running

```
python3 alert.py
```

## That's it and Voila 
