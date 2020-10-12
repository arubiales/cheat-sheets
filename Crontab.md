Hace falta escribir esto en el crontab para utilizar conda y el entorno que queramos

```
SHELL=/bin/bash
BASH_ENV=~/.bashrc_conda

#00 21 * * * conda activate /home/ubuntu/presstruth/presstruth-venv; python /home/ubuntu/presstruth/backend/scraping_launcher.py > /home/ubuntu/scraping_log.txt 2>&1
```

**Ademas** es necesario de home, copiar el apaartado de ```/.bashrc``` de conda y crear uno nuevo que se llame ```.bashrc_conda``` que será el que usará crontab. Een el copiaremos como hemos dicho el apartado de conda de ```/.bashrc```

