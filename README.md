# 🥇 Gold Master Strategy v3 — XAUUSD (Pine Script v6)

Stratégie de trading sur l'**OR (XAUUSD)** pour TradingView, **reconstruite à partir d'une
recherche sur les méthodes qui fonctionnent réellement sur l'or** : filtre de régime de
tendance + entrée sur pullback + Supertrend en trailing pour laisser courir les gains.

> ⚠️ **Avertissement** : aucune stratégie n'est « garantie rentable ». Le trading comporte
> un risque de perte. Ce script est un outil de recherche/backtest. Aucun backtest ne
> garantit les performances futures. Testez et adaptez à votre courtier avant tout usage réel.

---

## 📉 Pourquoi les versions précédentes perdaient (et ce qu'on a corrigé)

| Version | Problème | Résultat observé |
|---------|----------|------------------|
| v1 | 6 filtres simultanés + croisement → quasi aucun trade | 1 trade, inutilisable |
| v2 | Entrée sur **croisement EMA** → on achète en **haut** du mouvement, juste avant le retournement | PF 0.45–0.64, **16–23 % de réussite** |

**Le diagnostic chiffré** : à un ratio R:R de 1.8, il faut **> 36 % de trades gagnants** pour
être rentable. Les versions précédentes étaient à 16–23 % → entrées de mauvaise qualité
(croisement = retard + achat en extension), et **aucun filtre de régime** → on tradait dans
les ranges où le bruit massacre les stratégies de tendance.

---

## 🧠 Ce que dit la recherche (et ce que fait la v3)

D'après les sources analysées (voir plus bas), les stratégies or robustes partagent 4 principes :

| Principe (recherche) | Implémentation v3 |
|----------------------|-------------------|
| **Un Profit Factor > 1 vient d'un R:R asymétrique** (laisser courir les gains), pas d'un taux de réussite élevé | **Trailing via Supertrend** : petits stops, gros gains |
| **Entrer sur PULLBACK** dans la tendance (pas sur extension) : attendre un repli vers l'EMA puis la reprise | Entrée = repli sous l'EMA20 **puis** reprise (close repasse au-dessus) |
| **Supertrend multi-timeframe** améliore le taux de réussite | Supertrend (ATR 10 / facteur 3) + filtre HTF optionnel |
| **Filtre de régime indispensable** : ne trader que quand ça tend vraiment | EMA50 vs EMA200 + **pente EMA200** + **ADX ≥ 20** |

### Règle d'entrée v3
- **LONG** : EMA50 > EMA200 **et** pente EMA200 haussière **et** Supertrend haussier **et**
  ADX ≥ 20 **et** repli récent sous l'EMA20 **puis** clôture qui repasse au-dessus.
- **SHORT** : conditions strictement symétriques.

### Sortie v3 (le cœur de la rentabilité)
- **Stop initial** = `2 × ATR` (protection rapprochée).
- **Trailing Supertrend** : le stop ne fait que se resserrer → on capture les grandes
  tendances de l'or et on coupe vite les faux départs.
- Take profit fixe en option (R:R 2.5) si tu préfères des cibles fixes.

---

## ⏱️ Time frame recommandé

| Profil | Graphique | Filtre HTF |
|--------|-----------|------------|
| **Swing court (recommandé)** | **1H** | 4H (optionnel) |
| Intraday | 15 min | 4H |

➡️ **Commence en 1H.** Moins de bruit qu'en 15 min, signaux de meilleure qualité pour une
stratégie de tendance/pullback.

---

## 🚀 Installation

1. TradingView → graphique **XAUUSD**, passe-le en **1H**.
2. **⚠️ Supprime les anciennes versions** de la stratégie du graphique (sur tes captures, elle
   était ajoutée **3 fois** — garde-en une seule, sinon les signaux se superposent).
3. **Pine Editor** → colle le contenu de [`Gold_Strategy_XAUUSD.pine`](./Gold_Strategy_XAUUSD.pine) → **Add to chart**.
4. Onglet **Strategy Tester** pour les performances.

---

## 📐 Méthode de réglage (importante)

1. Lance la v3 **par défaut** → tu dois voir un **nombre raisonnable de trades** sur 1–2 ans,
   avec un **Facteur de profit qui doit viser > 1.3** et un taux de réussite **35–50 %**.
2. Si **trop peu de trades** : baisse l'ADX min (18), réduis la fenêtre de repli, désactive la pente.
3. Si **PF < 1** : durcis le régime (ADX 25), active le filtre **HTF**, ou la **session** (overlap 12:00–16:00).
4. Teste **Supertrend trailing** vs **TP fixe** (R:R 2.0–3.0) — garde le meilleur PF.
5. Valide sur **2–3 ans** (Deep Backtest), pas seulement 6 mois. Vise **30+ trades** pour que
   les stats aient un sens. Ne sur-optimise pas (garde des réglages standards).

---

## ⚠️ Le point honnête

Personne ne peut garantir une stratégie « ultra rentable » qui gagne à coup sûr — et les
sources sérieuses le disent toutes (« past backtest results do not guarantee future
performance »). Ce que cette v3 apporte, c'est une **structure que les traders or rentables
utilisent réellement** : trader la tendance, entrer sur repli, couper vite, laisser courir.
Le reste est une affaire d'**optimisation rigoureuse et de gestion du risque**.

---

## 📚 Sources de la recherche

- [Gold (XAUUSD) Trading Strategy: Guide for Forex Traders — NYC Servers](https://newyorkcityservers.com/blog/gold-xauusd-trading-strategy)
- [XAUUSD Trading Strategies: 3 Backtested Approaches (8,693 Trades) — Quant Signals](https://quant-signals.com/xauusd-trading-strategies/)
- [Gold Trading Strategies 2026 — LiteFinance](https://www.litefinance.org/blog/for-investors/gold-trading/gold-trading-strategies/)
- [The Best EMA Settings for Gold Trading — Dominion Markets](https://www.dominionmarkets.com/ema-settings-for-gold-trading-xau-usd/)
- [Ultimate Guide to Backtesting Gold with Smart Money Concepts — ACY](https://acy.com/en/market-news/education/ultimate-guide-backtesting-trading-gold-xau-usd-j-o-110321/)
- [XAU/USD Trading Strategies: What Traders Often Miss — Vantage](https://www.vantagemarkets.com/academy/trading-xauusd-tips-and-strategies/)
- [When to Trade Gold (XAUUSD): Top Strategies — QuantVPS](https://www.quantvps.com/blog/when-to-trade-gold)
- [Pine Script Supertrend Guide — Pineify](https://pineify.app/resources/blog/pine-script-supertrend-a-comprehensive-guide)

---

## 📁 Fichiers
- `Gold_Strategy_XAUUSD.pine` — la stratégie (Pine Script v6).
- `README.md` — ce guide.
