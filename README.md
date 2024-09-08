# Configuration Apache SSL - Mozilla Guideline

Voici un exemple de fichier README.md expliquant le fichier de configuration Apache/SSL que tu as partagé :
Configuration Apache SSL - Mozilla Guideline

Ce fichier de configuration a été généré le 14 juillet 2024 en utilisant les recommandations de la version 5.7 des directives de sécurité de Mozilla pour la configuration intermédiaire d'Apache 2.4.61 avec OpenSSL 1.1.1k. Il a pour but de sécuriser les connexions HTTP/HTTPS en configurant SSL/TLS avec des pratiques de sécurité recommandées.
Prérequis

Cette configuration nécessite les modules suivants dans Apache :

    mod_ssl : pour activer SSL sur Apache.
    mod_socache_shmcb : pour la mise en cache du certificat SSL (Stapling).
    mod_rewrite : pour rediriger les requêtes HTTP vers HTTPS.
    mod_headers : pour définir des en-têtes HTTP, comme HSTS (HTTP Strict Transport Security).

# Détails de la configuration

## 1. Redirection HTTP vers HTTPS

Le premier bloc VirtualHost *:80 gère les connexions HTTP (port 80) et redirige tout le trafic vers HTTPS avec une redirection 301 permanente, sauf pour les requêtes vers /.well-known/acme-challenge/ (nécessaire pour le processus de validation Let's Encrypt).

## 2. Configuration HTTPS

Le second bloc VirtualHost *:443 gère les connexions HTTPS (port 443) et active SSL via SSLEngine on.

    Certificats SSL : Les chemins vers le certificat signé et la clé privée doivent être renseignés dans les directives SSLCertificateFile et SSLCertificateKeyFile.
    DH Parameters : Pour une sécurité supplémentaire, des Diffie-Hellman parameters peuvent être utilisés (comme recommandé par Mozilla).

## 3. Support HTTP/2

La directive Protocols h2 http/1.1 active HTTP/2 si le serveur le supporte, tout en gardant HTTP/1.1 comme alternative.

## 4. Sécurité HSTS (HTTP Strict Transport Security)

Le module mod_headers est utilisé pour forcer les navigateurs à utiliser HTTPS via l'en-tête Strict-Transport-Security avec une durée de 2 ans (63072000 secondes).

## 5. Paramètres SSL/TLS

    Protocole : Seuls TLS v1.2 et v1.3 sont autorisés, tandis que SSLv3, TLSv1 et TLSv1.1 sont désactivés pour des raisons de sécurité.
    Suite de chiffrement : La configuration utilise une liste de suites de chiffrement sécurisées recommandées par Mozilla.
    Stapling : L'option SSLUseStapling On active le certificat OCSP stapling pour améliorer la performance des vérifications de validité de certificat.
