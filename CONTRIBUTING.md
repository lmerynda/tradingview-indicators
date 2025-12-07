# Contributing to TradingView Indicators

Thank you for your interest in contributing to this project! We welcome contributions from the community to help expand our collection of Pine Script indicators.

## 🤝 How to Contribute

### Adding New Indicators

1. **Fork the Repository**
   - Click the "Fork" button at the top right of the repository page

2. **Clone Your Fork**
   ```bash
   git clone https://github.com/YOUR_USERNAME/tradingview-indicators.git
   cd tradingview-indicators
   ```

3. **Create a New Branch**
   ```bash
   git checkout -b add-indicator-name
   ```

4. **Add Your Indicator**
   - Place your Pine Script file in the appropriate category folder:
     - `indicators/oscillators/` - For momentum oscillators (RSI, MACD, etc.)
     - `indicators/trend/` - For trend-following indicators (Moving Averages, ADX, etc.)
     - `indicators/volatility/` - For volatility indicators (ATR, Bollinger Bands, etc.)
     - `indicators/volume/` - For volume-based indicators (OBV, VWAP, etc.)
   - If your indicator doesn't fit any category, you can create a new folder

5. **Follow the File Naming Convention**
   - Use PascalCase for file names (e.g., `RelativeStrengthIndex.pine`, `MACD.pine`)
   - Use descriptive names that clearly identify the indicator

6. **Update the README**
   - Add your indicator to the appropriate section in `README.md`
   - Include a brief description and default parameters
   - Follow the existing format for consistency

7. **Commit Your Changes**
   ```bash
   git add .
   git commit -m "Add [Indicator Name] to [category]"
   ```

8. **Push to Your Fork**
   ```bash
   git push origin add-indicator-name
   ```

9. **Submit a Pull Request**
   - Go to the original repository on GitHub
   - Click "New Pull Request"
   - Select your fork and branch
   - Provide a clear description of your changes

## 📝 Indicator Guidelines

### Code Standards

1. **Pine Script Version**
   - Use Pine Script version 5 (`//@version=5`)

2. **Header Comments**
   - Include the Mozilla Public License 2.0 header
   - Add a copyright notice
   ```pine
   // This source code is subject to the terms of the Mozilla Public License 2.0 at https://mozilla.org/MPL/2.0/
   // © TradingView Indicators
   ```

3. **Indicator Declaration**
   - Use clear, descriptive names
   - Include both full name and short title
   ```pine
   indicator("Relative Strength Index (RSI)", shorttitle="RSI", overlay=false)
   ```

4. **Code Organization**
   - Group related code sections with comments
   - Use sections: Inputs, Calculations, Plotting, Alerts
   ```pine
   // Inputs
   // ... input declarations

   // Calculations
   // ... indicator calculations

   // Plot
   // ... plotting code

   // Alert conditions
   // ... alert setup
   ```

5. **Customizable Inputs**
   - Provide sensible default values
   - Use appropriate input types (int, float, bool, string, source)
   - Include descriptive titles
   ```pine
   length = input.int(14, minval=1, title="RSI Length")
   ```

6. **Comments**
   - Add comments to explain complex calculations
   - Keep comments concise and relevant

7. **Alerts**
   - Include useful alert conditions where appropriate
   - Use descriptive alert messages

### Testing

Before submitting your indicator:

1. **Test in TradingView**
   - Copy your code into TradingView's Pine Editor
   - Verify it compiles without errors
   - Test on multiple timeframes and symbols
   - Ensure all inputs work correctly

2. **Visual Verification**
   - Check that plots display correctly
   - Verify colors and styles are appropriate
   - Test with different input parameters

3. **Alert Testing**
   - Verify alert conditions trigger correctly
   - Check alert messages are clear and helpful

## 🎨 Style Guidelines

### Code Formatting

- Use consistent indentation (4 spaces)
- Keep lines under 120 characters when possible
- Use meaningful variable names
- Prefer clarity over brevity

### Color Scheme

- Use standard TradingView colors when appropriate
- Ensure colors are distinguishable
- Use `color.new()` for transparency when needed

### Plot Styles

- Choose appropriate plot styles (line, histogram, circles, etc.)
- Use consistent linewidth values
- Label all plots clearly

## 🐛 Reporting Issues

If you find a bug or have a suggestion:

1. **Check Existing Issues**
   - Search to see if the issue already exists

2. **Create a New Issue**
   - Use a clear, descriptive title
   - Provide detailed steps to reproduce (for bugs)
   - Include expected vs actual behavior
   - Add screenshots if helpful

3. **Include Context**
   - Pine Script version
   - TradingView account type (free/pro)
   - Browser and OS (if relevant)

## 📋 Pull Request Checklist

Before submitting your PR, ensure:

- [ ] Code follows Pine Script v5 syntax
- [ ] Indicator has been tested in TradingView
- [ ] README.md has been updated
- [ ] Code includes proper header comments
- [ ] Inputs have sensible defaults
- [ ] Code is well-commented
- [ ] No syntax errors or warnings
- [ ] Alert conditions are included (if applicable)
- [ ] Commit messages are clear and descriptive

## 📖 Resources

- [Pine Script v5 Documentation](https://www.tradingview.com/pine-script-docs/en/v5/)
- [Pine Script Reference](https://www.tradingview.com/pine-script-reference/v5/)
- [TradingView Community](https://www.tradingview.com/community/)

## 💬 Questions?

If you have questions about contributing:

- Open a discussion in the repository
- Check the TradingView Pine Script documentation
- Review existing indicators for examples

## 🌟 Recognition

All contributors will be recognized in the repository. Thank you for helping make this project better!

---

By contributing to this project, you agree that your contributions will be licensed under the GNU General Public License v3.0 and the Mozilla Public License 2.0 where applicable.
