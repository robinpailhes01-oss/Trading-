# 🥇 Gold Master Strategy — XAUUSD (Pine Script v6)

Stratégie de trading sur l'**OR (XAUUSD)** pour TradingView, construite avec la logique
des desks de prop trading : **suivre la tendance de fond, entrer sur le momentum, et
gérer le risque avant tout**.

> ⚠️ **Avertissement** : aucune stratégie n'est « garantie rentable ». Le trading comporte
> un risque de perte. Ce script est un outil de recherche/backtest. Testez-le et adaptez-le
> à votre courtier et à votre capital avant tout usage réel.

---

## 🔧 v2 — Pourquoi la première version ne donnait qu'1 trade

La v1 exigeait **6 conditions vraies en même temps qu'un croisement frais** → presque
aucun signal ne passait. Résultat : 1 seul trade sur 6 mois (statistiquement inutile).

**Corrections apportées en v2 :**
- **Entrée sur croisement EMA rapide/lente** (signal fréquent) au lieu de « close croise
  l'EMA rapide » combiné à tous les filtres.
- **Tous les filtres sont optionnels** et désactivables → on optimise pas à pas.
- **Sortie sur signal opposé** en plus du Stop/TP → les trades se clôturent correctement.
- Réglages par défaut **permissifs** pour générer assez de trades, puis on resserre.

### 📐 Méthode de réglage recommandée
1. Charge la stratégie **avec les réglages par défaut** (filtres légers) → tu dois voir
   **plusieurs dizaines de trades** sur 6 mois. C'est la base pour mesurer quoi que ce soit.
2. Regarde **Facteur de profit** et **Baisse maximale** dans le Strategy Tester.
3. **Active un filtre à la fois** (EMA 200, puis ADX, puis session) et observe si le Facteur
   de profit **monte**. Garde le filtre seulement s'il améliore les stats.
4. Ajuste **ATR Stop** et **Ratio R:R** pour réduire le drawdown.
5. Valide sur **2–3 ans** de données (Deep Backtest), pas seulement 6 mois.

> ❗ **Important** : un résultat sur 1 ou 2 trades ne veut rien dire. Il faut **30+ trades
> minimum** pour qu'un backtest soit interprétable. Vise un échantillon large d'abord,
> la rentabilité ensuite.

---

## 🎯 Logique de la stratégie

L'or est un actif fortement **directionnel et tendanciel**. La stratégie combine 5 filtres :

| # | Filtre | Rôle |
|---|--------|------|
| 1 | **Tendance** — EMA 21 / 50 / 200 | On ne trade que dans le sens de la tendance dominante |
| 2 | **Tendance supérieure (HTF)** — EMA 200 en 4H | On reste aligné avec la « big picture » |
| 3 | **Force** — ADX ≥ 20 | On évite les marchés plats/range qui tuent les stratégies de tendance |
| 4 | **Momentum** — RSI > 50 (long) / < 50 (short) | On confirme la dynamique du prix |
| 5 | **Session** — Londres + New York (07:00–20:00) | On trade quand la liquidité et la volatilité sont maximales |

### Règle d'entrée
- **LONG** : tendance haussière + prix qui repasse au-dessus de l'EMA rapide + RSI > 50 + ADX OK + HTF haussier + en session.
- **SHORT** : conditions symétriques.

### Gestion du risque (le cœur de la rentabilité)
- **Stop loss** = `1.5 × ATR` (s'adapte à la volatilité réelle de l'or).
- **Take profit** = Stop × **Ratio R:R 2.0** (on gagne 2× ce qu'on risque).
- **Trailing stop ATR** : verrouille les gains quand le trade évolue en notre faveur.
- **Position sizing basé sur le risque** : chaque trade ne risque que **1 % du capital**.

---

## ⏱️ Time frame recommandé

| Profil | Graphique | Filtre HTF |
|--------|-----------|------------|
| **Intraday (recommandé)** | **15 min** | 4H |
| Swing court | 1H | 4H ou 1D |
| Scalping | 5 min | 1H |

➡️ **Mon conseil : commence en 15 min avec le filtre HTF en 4H.** C'est le meilleur
équilibre entre nombre de signaux et qualité, et c'est la fenêtre la plus exploitée par les
traders intraday sur l'or.

---

## 🚀 Installation sur TradingView

1. Ouvre TradingView → graphique **XAUUSD** (ou OANDA:XAUUSD / GOLD selon ton broker).
2. Passe le graphique en **15 minutes**.
3. En bas, ouvre l'onglet **Pine Editor**.
4. Copie tout le contenu de [`Gold_Strategy_XAUUSD.pine`](./Gold_Strategy_XAUUSD.pine).
5. Colle-le dans l'éditeur → clique **Add to chart** (Ajouter au graphique).
6. Ouvre l'onglet **Strategy Tester** pour voir les performances du backtest.

---

## ⚙️ Réglages à optimiser

Tous les paramètres sont configurables dans les **Settings** de la stratégie :

- **Risque par trade** : ajuste `Risque par trade (%)` selon ta tolérance (0.5 % à 2 %).
- **Ratio R:R** : 2.0 par défaut ; teste 1.5 (plus de gains, plus petits) ou 3.0.
- **Multiplicateur ATR (Stop)** : 1.5 par défaut ; augmente si tu te fais stopper trop tôt.
- **ADX minimum** : monte à 25 pour ne prendre que les tendances très fortes.
- **Session** : adapte la plage horaire au fuseau de ton graphique.

> 💡 Utilise la fonction **Deep Backtest** + la **plage de dates** intégrée pour valider la
> robustesse sur plusieurs années et plusieurs régimes de marché.

---

## 📊 Optimisation recommandée

1. Backteste sur **au moins 2–3 ans** de données.
2. Regarde : **Profit Factor (> 1.5)**, **Max Drawdown**, **% trades gagnants**, **nombre de trades**.
3. Fais une **optimisation walk-forward** : optimise sur une période, valide sur la suivante.
4. Ne sur-optimise pas (overfitting) : préfère des réglages robustes à des réglages « parfaits » sur le passé.

---

## 📁 Fichiers

- `Gold_Strategy_XAUUSD.pine` — le code de la stratégie (Pine Script v6).
- `README.md` — ce guide.
