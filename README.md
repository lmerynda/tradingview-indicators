# TradingView Indicators

A comprehensive collection of Pine Script indicators for TradingView. This repository contains popular technical analysis indicators that can be easily imported into TradingView for charting and trading analysis.

## 📊 Available Indicators

### Oscillators
Oscillators are momentum indicators that fluctuate within a bounded range, helping identify overbought and oversold conditions.

- **[RSI (Relative Strength Index)](indicators/oscillators/RSI.pine)** - Measures the magnitude of recent price changes to evaluate overbought or oversold conditions
  - Default period: 14
  - Overbought level: 70
  - Oversold level: 30

- **[MACD (Moving Average Convergence Divergence)](indicators/oscillators/MACD.pine)** - Shows the relationship between two moving averages of a security's price
  - Fast Length: 12
  - Slow Length: 26
  - Signal Smoothing: 9

- **[Stochastic Oscillator](indicators/oscillators/Stochastic.pine)** - Compares a particular closing price to a range of its prices over time
  - %K Length: 14
  - %K Smoothing: 1
  - %D Smoothing: 3

### Trend Indicators
Trend indicators help identify the direction and strength of market trends.

- **[Moving Averages](indicators/trend/MovingAverages.pine)** - Multiple moving averages (SMA, EMA, WMA, VWMA)
  - Fast MA: 20
  - Medium MA: 50
  - Slow MA: 200

- **[ADX (Average Directional Index)](indicators/trend/ADX.pine)** - Measures trend strength regardless of direction
  - Length: 14
  - Trend Strength Threshold: 25

- **[Parabolic SAR](indicators/trend/ParabolicSAR.pine)** - Provides potential entry and exit points
  - Start: 0.02
  - Increment: 0.02
  - Maximum: 0.2

### Volatility Indicators
Volatility indicators measure the rate of price movements, regardless of direction.

- **[Bollinger Bands](indicators/volatility/BollingerBands.pine)** - Consists of a middle band with two outer bands
  - Length: 20
  - Standard Deviation Multiplier: 2.0

- **[ATR (Average True Range)](indicators/volatility/ATR.pine)** - Measures market volatility
  - Length: 14
  - Smoothing options: RMA, SMA, EMA, WMA

- **[Keltner Channels](indicators/volatility/KeltnerChannels.pine)** - Envelope-based indicator using ATR
  - Length: 20
  - Multiplier: 2.0
  - ATR Length: 10

### Volume Indicators
Volume indicators analyze trading volume to confirm trends and identify potential reversals.

- **[VWAP (Volume Weighted Average Price)](indicators/volume/VWAP.pine)** - Average price weighted by volume
  - Anchor Period: Session/Week/Month
  - Standard Deviation Bands included

- **[OBV (On-Balance Volume)](indicators/volume/OBV.pine)** - Cumulative volume indicator
  - MA Length: 20
  - Supports SMA and EMA

- **[Volume Profile](indicators/volume/VolumeProfile.pine)** - Shows volume distribution with moving average
  - Lookback Length: 20
  - MA Length: 20

## 🚀 How to Import Indicators into TradingView

### Method 1: Copy and Paste (Recommended)

1. **Open TradingView** - Go to [TradingView](https://www.tradingview.com/) and open a chart
2. **Open Pine Editor** - Click on "Pine Editor" at the bottom of the chart
3. **Copy the Code** - Navigate to the indicator file in this repository and copy its contents
4. **Paste in Editor** - Paste the code into the Pine Editor
5. **Save the Script** - Click "Save" and give it a name
6. **Add to Chart** - Click "Add to Chart" to apply the indicator

### Method 2: Manual Creation

1. Open TradingView and go to Pine Editor
2. Click "New" to create a new indicator
3. Copy the code from any indicator file in this repository
4. Paste it into the editor
5. Click "Save" and name your indicator
6. Click "Add to Chart"

## 📝 Customization

All indicators come with customizable parameters that can be adjusted in TradingView:

1. Add the indicator to your chart
2. Click on the indicator name in the chart legend
3. Select "Settings" (gear icon)
4. Adjust the parameters in the "Inputs" tab
5. Customize colors and styles in the "Style" tab

## 💡 Usage Tips

- **Combine Multiple Indicators** - Use different types of indicators together for better analysis (e.g., RSI + MACD for momentum confirmation)
- **Adjust Parameters** - Default parameters work well for most cases, but feel free to optimize them for your specific trading style
- **Set Up Alerts** - Most indicators include alert conditions that you can enable in TradingView
- **Backtest** - Use TradingView's backtesting features to validate indicator signals on historical data

## 📚 Learning Resources

- [TradingView Pine Script Documentation](https://www.tradingview.com/pine-script-docs/en/v5/)
- [Pine Script v5 User Manual](https://www.tradingview.com/pine-script-docs/en/v5/Introduction.html)
- [TradingView Community Scripts](https://www.tradingview.com/scripts/)

## 🤝 Contributing

Contributions are welcome! If you have improvements or additional indicators:

1. Fork the repository
2. Create a feature branch
3. Add your indicator with proper documentation
4. Submit a pull request

## 📄 License

This project is licensed under the GNU General Public License v3.0 - see the [LICENSE](LICENSE) file for details.

Pine Script code files may also be licensed under the Mozilla Public License 2.0 as indicated in individual file headers.

## ⚠️ Disclaimer

These indicators are for educational and informational purposes only. They should not be considered as financial advice. Always do your own research and consider your risk tolerance before making trading decisions.

## 🔗 Related Resources

- [TradingView](https://www.tradingview.com/)
- [Pine Script Reference](https://www.tradingview.com/pine-script-reference/v5/)
- [TradingView Ideas](https://www.tradingview.com/ideas/)

---

**Happy Trading! 📈**