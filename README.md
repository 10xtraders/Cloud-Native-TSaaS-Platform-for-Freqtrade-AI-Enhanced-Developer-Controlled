## 🚀 New: Cloud-Native TSaaS Platform for Freqtrade — AI-Enhanced + Developer-Controlled

Hey everyone 👋

We’re excited to introduce something the [**Freqtrade community**](https://www.freqtrade.io/en/stable/) has needed for a long time:

A [**cloud-native, AI-powered TSaaS platform**](https://10xtraders.ai) that lets you build, test, and run your Freqtrade strategies in the cloud — instantly.

No more CLI headaches, YAML tuning, or DevOps fire drills.

---

### 🚀 What It Is

This platform lets you:

- ✅ **Upload or build your own `strategy with AI`**
- ✅ **Auto-deploy live or paper bots with 1-click Kubernetes orchestration**
- ✅ **Use AI tools to:**
  - Generate new strategies from scratch (GPT-powered)
  - Add fragility filters and pre-positioning risk models *(coming soon)*
- ✅ **View real-time candle charts + trade overlays via TradingView charts**
- ✅ **Export your bot + strategy if you prefer to self-host** — you keep full control
- ✅ **Works 100% with native Freqtrade strategies and config**

---

### 🔐 Open Source Compliance & Privacy

- We use Freqtrade (GPLv3) as the core engine
- Our platform is a hosted service — no code redistribution = GPL compliant
- Your strategies and API keys stay private — we never log or share them

---

### 📈 Who It’s For

- Developers and Traders who want to focus on strategy, not infrastructure
- Algo traders looking for AI tooling without black-box lock-in
- Freqtrade users who want an upgrade path to production trading

---

### 🛠️ Built For the Freqtrade Ecosystem

We’re Freqtrade-native by design — this isn’t a wrapper. You can:

- Deploy any Freqtrade-compatible `strategy`. Check some [**here**](https://github.com/freqtrade/freqtrade-strategies/tree/main) from Freqtrade and [**this**](https://github.com/davidzr/freqtrade-strategies/tree/main/strategies) as provided by [**@davidzr**](https://github.com/davidzr) 
- Create your custom `config` for every strategy
- Live view each bot execution logs

---

### 📬 Try It (Free Paper Bot Plan Available)

🔗 [https://10xtraders.ai](https://10xtraders.ai)

You’ll get:

- 1 free paper bot
- AI strategy generation credits
- Real-time trade logging and performance tracking

---

### 🧠 Coming Soon

- AI Pre-positioning & market regime classification
- Strategy marketplace
- Copy-trading subscriptions
- Multi-bot portfolio analytics
- Broker integration for stocks & options

---

### 🙌 Let’s Collaborate

If you:

- Have a cool strategy to showcase
- Want to contribute to integration ideas
- Have feedback from Freqtrade usage pain points

Drop a comment or DM. We’re building this with and for the Freqtrade community.

Thanks,  
**10XTraders.ai**

---

## 📦 How to Deploy Your own Bot.

You can deploy a Freqtrade bot to our [**cloud**](https://10xtarders.ai). Just initiate a chat on our platform  
You can clone our [**repo*](https://github.com/10xtraders/Cloud-Native-TSaaS-Platform-for-Freqtrade-AI-Enhanced-Developer-Controlled) and follow below instructions. 

### **Summary / Instructions**

- **The config**: Drop in any [Freqtrade sample config](https://www.freqtrade.io/en/stable/configuration/) or your own.
- **Your API payload** must have these four keys:  
  - `namespace`: Unique namespace (per bot/strategy instance)
  - `strategy_class`: Python class name of your strategy (e.g. `MyStrategy`)
  - `strategy_code`: Your strategy code as a Python string
  - `config`: The Freqtrade config dictionary (JSON-serializable)
- **The backend** Run the python code and point to the folder where you saved the heml chart [**from**](https://github.com/DerSalvador/freqtrade-k8s-helm-chart/tree/develop).
- **Kubernetes ingress** is auto-created for each bot for secure access.

### **Sample API Call**

```json
POST /deploy-bot
Content-Type: application/json

{
  "namespace": "simplersistrategy1747867348688vgueu8",
  "strategy_class": "Simplersistrategy1747867348688vgueu8",
  "strategy_code": "class Simplersistrategy1747867348688vgueu8(...): ...", 
    "config": {"botName": "simplersistrategy1747867348688vgueu8", "dry_run": true, "bot_name": "MeghanBot", "exchange": {"key": "bmutua3501@gmail.com", "uid": "", "name": "binance", "secret": "cryptT0ps@0101", "sandbox": false, "password": "", "enable_ws": true, "log_responses": false, "only_from_ccxt": false, "pair_blacklist": ["DOGE/USDT", "SHIB/USDT"], "pair_whitelist": ["AVAX/USDT", "DOT/USDT", "LINK/USDT", "UNI/USDT", "MATIC/USDT", "LTC/USDT", "ATOM/USDT", "NEAR/USDT", "FIL/USDT", "AAVE/USDT", "SAND/USDT", "GRT/USDT", "FTM/USDT", "ALGO/USDT", "ICP/USDT"], "unknown_fee_rate": 0, "skip_open_order_update": false, "markets_refresh_interval": 60}, "stoploss": -0.1, "strategy": "simplersistrategy1747867348688vgueu8", "telegram": {"token": "", "chat_id": "", "enabled": false}, "enable_ws": true, "internals": {"heartbeat_interval": 60, "process_throttle_secs": 5}, "pairlists": [{"method": "StaticPairList"}, {"method": "VolumePairList", "sort_key": "quoteVolume", "number_assets": 20, "refresh_period": 1800}, {"method": "AgeFilter", "min_days_listed": 10}, {"method": "PrecisionFilter"}, {"method": "PriceFilter", "min_price": 0.0000001, "low_price_ratio": 0.01}, {"method": "SpreadFilter", "max_spread_ratio": 0.005}, {"method": "RangeStabilityFilter", "lookback_days": 10, "refresh_period": 1440, "min_rate_of_change": 0.01}], "timeframe": "5m", "api_server": {"enabled": true, "password": "SuperSecret1!", "username": "meghan", "ws_token": "SOME_RANDOM_WS_TOKEN", "verbosity": "error", "listen_port": 8080, "CORS_origins": "[]", "enable_openapi": false, "jwt_secret_key": "CHANGE_ME_TO_RANDOM_SECRET", "listen_ip_address": "127.0.0.1"}, "configName": "simplersistrategy1747867348688vgueu8", "margin_mode": "cross", "minimal_roi": {"0": 0.04, "30": 0.02, "60": 0.01}, "order_types": {"exit": "limit", "entry": "limit", "stoploss": "market", "force_exit": "market", "force_entry": "market", "emergency_exit": "market", "stoploss_on_exchange": false, "stoploss_on_exchange_interval": 60}, "protections": [{"method": "StoplossGuard", "trade_limit": 4, "only_per_pair": false, "stop_duration_candles": 60, "lookback_period_candles": 60}, {"method": "CooldownPeriod", "stop_duration_candles": 20}, {"method": "MaxDrawdown", "trade_limit": 20, "max_allowed_drawdown": 0.2, "stop_duration_candles": 10, "lookback_period_candles": 200}, {"method": "LowProfitPairs", "trade_limit": 1, "required_profit": 0.02, "stop_duration_candles": 2, "lookback_period_candles": 360}], "exit_pricing": {"price_side": "ask", "order_book_top": 1, "use_order_book": false, "price_last_balance": 0}, "stake_amount": 1000, "trading_mode": "spot", "entry_pricing": {"price_side": "bid", "order_book_top": 1, "use_order_book": false, "price_last_balance": 0, "check_depth_of_market": {"enabled": false, "bids_to_ask_delta": 1}}, "initial_state": "stopped", "strategy_path": "user_data/strategies/", "trailing_stop": false, "dry_run_wallet": "10000", "stake_currency": "USDT", "max_open_trades": 6, "unfilledtimeout": {"exit": 30, "unit": "minutes", "entry": 10, "exit_timeout_count": 0}, "use_exit_signal": true, "dataformat_ohlcv": "json", "exit_profit_only": false, "available_capital": 0, "dataformat_trades": "jsongz", "exit_profit_offset": 0, "force_entry_enable": true, "order_time_in_force": {"exit": "gtc", "entry": "gtc"}, "fiat_display_currency": "USD", "amount_reserve_percent": 0.05, "tradable_balance_ratio": 0.99, "trailing_stop_positive": 0.005, "amend_last_stake_amount": false, "disable_dataframe_checks": false, "markets_refresh_interval": 60, "process_only_new_candles": true, "cancel_open_orders_on_exit": false, "ignore_roi_if_entry_signal": false, "position_adjustment_enable": false, "last_stake_amount_min_ratio": 0.5, "max_entry_position_adjustment": -1, "trailing_stop_positive_offset": 0.0051, "trailing_only_offset_is_reached": false, "ignore_buying_expired_candle_after": 0}
}
