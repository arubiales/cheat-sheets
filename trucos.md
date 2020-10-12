# Instalar paquetes con conda y si no pip de requirement.txt

```while read requirement; do conda install --yes $requirement || pip install $requirement; done < requirements.txt```

Con esta sentencia instalamos los paquetes de requirement.txt con conda, y si no lo encontramos, usamos pip