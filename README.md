T-WHITE CAREER — VERSION KKiaPay
================================

Cette version contient :
- index.html : site T-WHITE CAREER + constructeur de CV
- paiement KKiaPay intégré côté navigateur avec le SDK officiel
- server.js : backend Node.js/Express
- vérification serveur des transactions avec le SDK Admin KKiaPay
- webhook /webhook/kkiapay
- package.json
- .env.example
- config.js (référence publique, sans secret)
- data/orders.json (stockage MVP local des commandes)

IMPORTANT SÉCURITÉ
------------------
Ne mets JAMAIS la Private API Key ou le Secret KKiaPay dans index.html, config.js ou un dépôt public.
Comme les clés visibles dans tes captures ont été exposées dans la conversation, régénère-les avant la production.

ARCHITECTURE
------------
Client -> T-WHITE CAREER -> KKiaPay Mobile Money -> webhook + vérification serveur -> commande PAID

Le montant est fixé côté serveur :
- BASIC = 2 000 FCFA
- PRO = 5 000 FCFA

Le navigateur ne décide pas du montant final.

INSTALLATION LOCALE
-------------------
1. Installe Node.js 18 ou plus récent.
2. Ouvre un terminal dans ce dossier.
3. Lance : npm install
4. Copie .env.example vers .env
5. Dans .env, renseigne tes clés KKiaPay Sandbox :
   KKIAPAY_PUBLIC_KEY=
   KKIAPAY_PRIVATE_KEY=
   KKIAPAY_SECRET_KEY=
   KKIAPAY_SANDBOX=true
6. Choisis un secret long pour :
   KKIAPAY_WEBHOOK_SECRET=
7. Lance : npm start
8. Ouvre : http://localhost:3000

CONFIGURATION KKiaPay SANDBOX
------------------------------
Dans KKiaPay : Développeurs > Webhook > Ajouter un webhook.
Après déploiement du serveur, utilise :
https://TON-DOMAINE/webhook/kkiapay

Coche :
- Transactions succès
- Transactions échecs

Dans « Secret hash », mets exactement la même valeur que KKIAPAY_WEBHOOK_SECRET.

DÉPLOIEMENT SIMPLE
------------------
Le plus simple est de déployer TOUT ce dossier comme une seule application Node.js sur Render, Railway ou un autre hébergeur Node.

Commande de build : npm install
Commande de démarrage : npm start

Variables d'environnement à créer sur l'hébergeur :
PORT (souvent fourni automatiquement)
KKIAPAY_PUBLIC_KEY
KKIAPAY_PRIVATE_KEY
KKIAPAY_SECRET_KEY
KKIAPAY_SANDBOX=true
KKIAPAY_WEBHOOK_SECRET
APP_URL=https://TON-DOMAINE

Après déploiement, teste :
https://TON-DOMAINE/api/health

Puis retourne dans KKiaPay > Développeurs > Webhook et crée le webhook avec :
https://TON-DOMAINE/webhook/kkiapay

PASSAGE EN PRODUCTION
---------------------
1. Active ton compte marchand KKiaPay.
2. Récupère les clés LIVE dans l'espace Développeurs.
3. Remplace les trois clés Sandbox par les clés LIVE dans les variables d'environnement.
4. Mets KKIAPAY_SANDBOX=false.
5. Garde le webhook en HTTPS.
6. Fais un vrai test de paiement de faible montant avant d'ouvrir les ventes.

NOTE SUR LES COMMANDES
-----------------------
data/orders.json est un stockage MVP. Pour une vraie plateforme avec plusieurs clients et beaucoup de commandes, remplace-le par une base de données (PostgreSQL, par exemple). Les fichiers locaux peuvent être perdus selon l'hébergeur.

NOTE SUR T-MONEY / FLOOZ
------------------------
Le site ouvre le moyen Mobile Money KKiaPay et limite le pays à TG. L'opérateur disponible est choisi dans l'interface KKiaPay. Le champ « opérateur préféré » du formulaire n'est pas utilisé pour contourner le widget.

DOCUMENTATION KKiaPay
---------------------
https://kkiapay.me/documentation/
https://docs.kkiapay.me/v1/plugin-et-sdk/sdk-javascript
https://docs.kkiapay.me/v1/plugin-et-sdk/admin-sdks-server-side/node.js-admin-sdk
https://docs.kkiapay.me/v1/tableau-de-bord/webhook