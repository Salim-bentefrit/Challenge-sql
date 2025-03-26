FROM victoriametrics/vmagent:v1.93.0

# Copie de la config dans le conteneur
COPY vmagent.yml /etc/vmagent/vmagent.yml

# Commande de lancement avec le fichier de config
CMD ["/vmagent", "-promscrape.config=/etc/vmagent/vmagent.yml", "-remoteWrite.url=http://victoria-metrics:8428/api/v1/write"]




apiVersion: v1
kind: ConfigMap
metadata:
  name: vmagent-config
  namespace: test
data:
  vmagent.yml: |
    global:
      scrape_interval: 30s
    scrape_configs:
      - job_name: 'blackbox_exporter_status'
        metrics_path: /probe
        scrape_interval: 30s
        scrape_timeout: 10s
        params:
          module: ["http_2xx"]
        static_configs:
          - targets: ["https://www.google.com"]
