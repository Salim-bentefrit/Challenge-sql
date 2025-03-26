FROM victoriametrics/vmagent:v1.93.0

# Copie de la config dans le conteneur
COPY vmagent.yml /etc/vmagent/vmagent.yml

# Commande de lancement avec le fichier de config
CMD ["/vmagent", "-promscrape.config=/etc/vmagent/vmagent.yml", "-remoteWrite.url=http://victoria-metrics:8428/api/v1/write"]



