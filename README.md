# TradingView VWAP Indicator

A Pine Script implementation of VWAP (Volume Weighted Average Price) with configurable standard deviation bands for TradingView.

## 📊 Indicator

### VWAP (Volume Weighted Average Price)

The Volume Weighted Average Price (VWAP) is a trading benchmark that gives the average price a security has traded at throughout the day, based on both volume and price. This implementation includes standard deviation bands to help identify potential support and resistance levels.

**Features:**
- Volume-weighted price calculation
- Configurable anchor periods (Session, Week, Month)
- Three customizable standard deviation bands (1.0σ, 2.0σ, and 2.3σ extension)
- Clean visual representation with colored bands
- Alert conditions for price crosses

**Default Parameters:**
- Anchor Period: Session
- Source: hlc3 (average of high, low, and close)
- 1st Band Multiplier: 1.0
- 2nd Band Multiplier: 2.0
- 3rd Band Multiplier (Extension): 2.3

## 🚀 How to Import into TradingView

### Method 1: Copy and Paste (Recommended)

1. **Open TradingView** - Go to [TradingView](https://www.tradingview.com/) and open a chart
2. **Open Pine Editor** - Click on "Pine Editor" at the bottom of the chart
3. **Copy the Code** - Navigate to [VWAP.pine](indicators/volume/VWAP.pine) and copy its contents
4. **Paste in Editor** - Paste the code into the Pine Editor
5. **Save the Script** - Click "Save" and give it a name
6. **Add to Chart** - Click "Add to Chart" to apply the indicator

### Method 2: Manual Creation

1. Open TradingView and go to Pine Editor
2. Click "New" to create a new indicator
3. Copy the code from [VWAP.pine](indicators/volume/VWAP.pine)
4. Paste it into the editor
5. Click "Save" and name your indicator
6. Click "Add to Chart"

## 📝 Customization

The indicator comes with customizable parameters that can be adjusted in TradingView:

1. Add the indicator to your chart
2. Click on the indicator name in the chart legend
3. Select "Settings" (gear icon)
4. Adjust the parameters in the "Inputs" tab:
   - **Anchor Period**: Choose between Session, Week, or Month
   - **Source**: Select the price source (default: hlc3)
   - **Show Standard Deviation Bands**: Toggle bands on/off
   - **1st/2nd/3rd Band Multipliers**: Adjust the standard deviation multipliers
5. Customize colors and styles in the "Style" tab

## 💡 Usage Tips

- **VWAP as Support/Resistance**: Price often respects VWAP as a dynamic support or resistance level
- **Band Trading**: The standard deviation bands can help identify overbought/oversold conditions
- **Anchor Period Selection**: Use Session for intraday trading, Week for swing trading, Month for longer-term analysis
- **2.3σ Extension**: The third band at 2.3 standard deviations provides an additional extreme level for identifying significant price deviations
- **Set Up Alerts**: Enable alerts for when price crosses VWAP or the bands

## 📚 Learning Resources

- [TradingView Pine Script Documentation](https://www.tradingview.com/pine-script-docs/en/v5/)
- [Pine Script v5 User Manual](https://www.tradingview.com/pine-script-docs/en/v5/Introduction.html)
- [VWAP Trading Strategies](https://www.tradingview.com/scripts/)

## 📄 License

This repository is dual-licensed:

- **Repository structure and documentation** (README.md, etc.) are licensed under the GNU General Public License v3.0 - see the [LICENSE](LICENSE) file for details.

- **Pine Script indicator file** (VWAP.pine) is licensed under the Mozilla Public License 2.0 as indicated in the file header, allowing easier integration with TradingView.

## ⚠️ Disclaimer

This indicator is for educational and informational purposes only. It should not be considered as financial advice. Always do your own research and consider your risk tolerance before making trading decisions.

## 🔗 Related Resources

- [TradingView](https://www.tradingview.com/)
- [Pine Script Reference](https://www.tradingview.com/pine-script-reference/v5/)
- [TradingView Ideas](https://www.tradingview.com/ideas/)

---

**Happy Trading! 📈**