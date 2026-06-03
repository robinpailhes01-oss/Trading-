# 🥇 Gold Master Strategy — XAUUSD (Pine Script v6)

Stratégies de trading sur l'**OR (XAUUSD)** pour TradingView, construites à partir d'une
recherche sur les méthodes qui fonctionnent réellement sur l'or :
**tendance + entrée sur pullback + Supertrend en trailing + gestion du risque pro**.

> ⚠️ **Avertissement** : aucune stratégie n'est « garantie rentable ». Le trading comporte
> un risque de perte. Aucun backtest ne garantit les performances futures. Teste et adapte à
> ton courtier avant tout usage réel.

---

## 🚀 PAR OÙ COMMENCER (lis ça d'abord)

1. **Symbole** : utilise le **XAUUSD de Vantage** (il a beaucoup d'historique). ⚠️ Évite OANDA
   en intraday : trop peu de bougies chargées → backtests à 1-2 trades, inutilisables.
2. **Une seule stratégie à la fois** sur le graphique. Supprime les doublons (le badge « +2/+3 »
   en haut à gauche = plusieurs copies → stats faussées).
3. **Charge assez d'historique** : dézoome / fais défiler vers la gauche, teste sur **2-3 ans**.
   Il faut **30+ trades** pour qu'un backtest ait un sens.
4. Colle le contenu du fichier `.pine` dans le **Pine Editor** → **Ajouter au graphique**.

---

## 📁 Quel fichier utiliser selon ton objectif

| Objectif | Fichier | Time frame | Profil |
|----------|---------|-----------|--------|
| 🏆 **DÉFI : 2-5 trades/sem gagnants, perte max 10 %** | `Gold_Strategy_XAUUSD_Challenge10.pine` | **1H** | Repli RSI en tendance + **gouverneur de drawdown 10 %** |
| 🔥 **Agressif + beaucoup de trades** | `Gold_Strategy_XAUUSD_30m_Balanced.pine` | 30 min | Risque 3 %, ~200 trades |
| 🎯 **Le plus fiable** (peu de trades) | `Gold_Strategy_XAUUSD_4H_Sniper.pine` | 4H | Confluence max, PF ~1,7 |
| 🎯 **Fort taux de réussite** | `Gold_Strategy_XAUUSD_HighWinRate.pine` | 1H | Repli RSI, cible serrée |
| 🧱 **Base robuste polyvalente** | `Gold_Strategy_XAUUSD.pine` | 4H | Risque 1 %, réglable |
| 👁️ **Avec supports/résistances** | `Gold_Strategy_XAUUSD_PriceAction_SR.pine` | 30m / 1H | Niveaux S/R affichés |
| ⛔ **À éviter** | `Gold_Strategy_XAUUSD_15m_Aggressive.pine` | 15 min | 15 min = trop bruité (PF < 1) |

---

## 🏆 LE DÉFI 10 % — `Gold_Strategy_XAUUSD_Challenge10.pine`

Conçue à partir de 3 axes de recherche (entrée, money management, fréquence) pour répondre
**exactement** à tes critères : **2-5 trades gagnants par semaine, petits gains réguliers,
perte maximale plafonnée à 10 %**.

| Critère | Comment c'est garanti |
|---------|------------------------|
| **2-5 trades/semaine** | Signal **1H** + filtre tendance **4H** + session **08:00-16:00** |
| **Trades gagnants** | Repli RSI **dans le sens de la tendance** (seul edge vérifié sur l'or) + cible serrée 1,3R + break-even |
| **Perte max 10 %** | **Gouverneur de capital** : réduit la taille dès −5 % de drawdown, et **ARRÊTE les entrées à −10 %** (kill switch) |
| **Risque assumé** | 1 % par trade (agressif mais borné par le gouverneur) |

### Le gouverneur de drawdown (groupe « 5b »)
- Suit le **sommet de capital** et calcule le **drawdown courant** (affiché en haut à droite).
- À **−5 %** de drawdown → taille de position **divisée par 2**.
- À **−10 %** → **plus aucune nouvelle entrée** (le défi est « perdu », on protège le capital).
- Fond rouge/orange sur le graphique quand le gouverneur agit.

### Réglages pour ajuster
- **Plus de trades** : élargis la session, baisse l'ADX à 15, ou désactive le pullback EMA21.
- **Encore plus de réussite** : baisse la cible à 1,0-1,2R.
- **Resserrer la fréquence à 2-3/sem** : session sur l'overlap **12:00-16:00**.

> ⚠️ Le gouverneur plafonne le risque, il ne garantit pas le gain. Un fort taux de réussite
> n'est pas une garantie de profit (les sources le rappellent toutes). Backteste sur 2-3 ans.

---

## 🔥 TA STRATÉGIE : 30 min Équilibré (agressif)

C'est le **meilleur compromis « beaucoup de trades + rentable »** d'après les backtests :
**~218 trades**, **PF ~1,3**, **53 % de réussite**, drawdown faible. Réglée pour l'agressivité :

| Réglage | Valeur | Rôle |
|---------|--------|------|
| **Risque par trade** | **3 %** | Agressif → rendement amplifié |
| **Levier max** | 10 | Permet d'atteindre les 3 % de risque |
| **Filtre HTF 4H** | activé | Aligne sur la grande tendance |
| **Break-even + sortie partielle** | activés | Sécurisent les gains, lissent la courbe |

### Comment l'utiliser
1. Graphique **Vantage XAUUSD** en **30 min**.
2. Colle `Gold_Strategy_XAUUSD_30m_Balanced.pine` → Ajouter au graphique.
3. Backtest sur 2025-2026 (charge bien l'historique).

### Régler ton niveau d'agressivité (groupe « 6) Gestion du risque »)
| Risque/trade | Rendement* | Drawdown* |
|--------------|------------|-----------|
| 2 % | modéré | ~8 % |
| **3 % (défaut)** | élevé | ~12 % |
| 4-5 % | très élevé | ~16-20 % ⚠️ |

*\(approximations ; le PF et le % de réussite ne changent pas, c'est juste la taille des positions\)*

> ⚠️ **Ne dépasse pas 5 %.** Au-delà, une mauvaise série de pertes peut détruire le compte,
> même avec un bon edge. L'agressivité se règle par le **risque par trade**, pas en descendant
> sur des time frames plus bruités.

### Pour avoir ENCORE plus de trades
- Désactive le **filtre HTF 4H** (groupe « 4 ») → plus de signaux.
- Baisse **ADX minimum** à 15 (groupe « 3 »).
- Désactive le **filtre pente EMA200** (groupe « 3 »).
- ⚠️ Plus de trades = signaux de moins bonne qualité en moyenne. Surveille le Profit Factor.

---

## 📊 Logique commune à toutes les versions

1. **Filtre de régime** : on ne trade QUE dans une vraie tendance (EMA50/200 + pente + ADX + Supertrend).
2. **Entrée sur pullback** : on attend un repli vers l'EMA rapide PUIS la reprise → on entre bas, meilleur R:R.
3. **Sorties** : trailing Supertrend (laisse courir les gains) + break-even + sortie partielle.
4. **Money management** : taille de position calculée sur le risque (% du capital / distance de stop),
   plafond de levier, capital 1 000 €.

---

## 📈 Résultats backtestés (référence, Vantage XAUUSD)

| Version | TF | Période | Trades | Réussite | Profit Factor | Drawdown |
|---------|----|---------|--------|----------|---------------|----------|
| Sniper | 4H | 7,5 ans | 64 | 56 % | **1,70** | 4,8 % |
| Base | 4H | 7,5 ans | 162 | 49 % | 1,35 | 9,9 % |
| **30m Équilibré** | 30m | 1,5 an | **218** | 54 % | 1,30 | 3,1 % |
| 15m | 15m | — | — | 39 % | 0,65 ❌ | — |

*Le 30 min à 3 % de risque vise un rendement nettement supérieur pour un drawdown maîtrisé.*

---

## ⚠️ Le mot de la fin (honnête)

- Un **Profit Factor de 1,3 à 1,7 sur l'or, c'est un vrai bon résultat.** Personne ne te
  donnera un « +500 %/an garanti » — ça n'existe pas, et celui qui le promet ment.
- L'agressivité **se gère par le % de risque**, en gardant une stratégie qui marche (30m/4H),
  **pas** en cherchant plus d'action sur le 15 min qui ne fonctionne pas.
- La vraie performance dépendra de ta **discipline** et du **respect du risque**, plus que d'un
  indicateur de plus.

---

## 📂 Liste des fichiers
- `Gold_Strategy_XAUUSD_Challenge10.pine` — **DÉFI 10 % : 2-5 trades/sem, perte max 10 % (GOLD-DEFI-10)** 🏆
- `Gold_Strategy_XAUUSD_HighWinRate.pine` — fort taux de réussite, repli RSI (GOLD-HWR)
- `Gold_Strategy_XAUUSD.pine` — base robuste 4H (GOLD-MASTER-v3)
- `Gold_Strategy_XAUUSD_4H_Sniper.pine` — peu de trades, ultra fiable (GOLD-4H-SNIPER)
- `Gold_Strategy_XAUUSD_30m_Balanced.pine` — **agressif + beaucoup de trades (GOLD-30m-BAL)** ⭐
- `Gold_Strategy_XAUUSD_PriceAction_SR.pine` — supports/résistances (GOLD-SR-PA)
- `Gold_Strategy_XAUUSD_15m_Aggressive.pine` — ⛔ à éviter (GOLD-15m-AGGR)
