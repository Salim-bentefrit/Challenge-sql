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



#!/bin/sh
set -e  # Arrêter le script en cas d'erreur

echo "🔄 Mise à jour des dépôts Alpine..."
apk update && apk upgrade

echo "📋 Vérification de la version d'Alpine..."
ALPINE_VERSION=$(cat /etc/alpine-release | cut -d '.' -f1,2)
echo "Version détectée : Alpine $ALPINE_VERSION"

Définition de la version stable d'Alpine attendue
ALPINE_STABLE="3.21"

Si la version d'Alpine est inférieure à la version stable, mettre à jour les dépôts
if [ "$(printf '%s\n' "$ALPINE_VERSION" "$ALPINE_STABLE" | sort -V | head -n1)" != "$ALPINE_STABLE" ]; then
    echo "⚠️ Mise à jour des dépôts vers la dernière version stable..."
    sed -i "s|v$ALPINE_VERSION|latest-stable|g" /etc/apk/repositories
    apk update
fi

echo "📦 Vérification de binutils..."
BINUTILS_VERSION=$(apk search binutils | grep binutils | tail -n1)

if [ -z "$BINUTILS_VERSION" ]; then
    echo "❌ binutils introuvable dans les dépôts standards."
    echo "Ajout du dépôt Edge pour binutils..."
    echo "https://dl-cdn.alpinelinux.org/alpine/edge/main" >> /etc/apk/repositories
    apk update
fi

echo "⬇️ Installation des dépendances essentielles..."
apk add --no-cache binutils git go

echo "✅ binutils installé avec la version :"
apk info binutils | grep version

echo "✅ Go installé avec la version :"
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
