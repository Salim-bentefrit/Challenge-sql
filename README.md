#!/bin/sh
set -e  # Arrêter le script en cas d'erreur

echo "🔄 Mise à jour des dépôts Alpine..."
apk update

echo "⬇️ Installation de Go et Git..."
apk add --no-cache go git
apk add binutils

echo "✅ Go installé :"
go version

echo "📁 Création du projet Go..."
mkdir -p /app
cd /app

echo "🔹 Initialisation du module Go..."
go mod init myproject

echo "📦 Installation de la version spécifique de golang.org/x/oauth2@v0.27.0..."
go get golang.org/x/oauth2@v0.27.0

echo "✅ Dépendance OAuth2 installée avec succès."

echo "📋 Vérification des dépendances du projet..."
go list -m all

echo "🚀 Installation terminée !"


