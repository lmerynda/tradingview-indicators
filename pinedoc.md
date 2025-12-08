# Pine Script v6 Documentation

Pine Script v6 is TradingView's domain-specific language for creating custom technical indicators, strategies, and libraries on financial charts. This comprehensive documentation repository provides core language concepts, practical implementation patterns, anti-patterns to avoid, and decision trees for optimal development choices. The documentation covers everything from basic script structure to advanced multi-timeframe analysis, with extensive code examples demonstrating real-world trading indicator and strategy development.

This resource serves as a complete reference for Pine Script developers at all levels, from beginners learning the execution model and type system to advanced users implementing complex data structures like matrices and maps introduced in v6. It includes official TradingView documentation, curated anti-patterns that highlight common mistakes with corrected implementations, and decision trees that guide developers through critical choices like avoiding repainting, selecting appropriate data structures, and optimizing performance.

---

## Core APIs and Functions

### Script Declaration - indicator()

Every Pine Script indicator must start with the `indicator()` declaration statement. This function defines the script's name, display properties, and runtime behavior.

```pine
//@version=6
indicator("My RSI Strategy", overlay=false, precision=2, scale=scale.right)

// Calculate RSI with 14-period default
rsiValue = ta.rsi(close, 14)

// Plot with conditional coloring
rsiColor = rsiValue > 70 ? color.red : rsiValue < 30 ? color.green : color.gray
plot(rsiValue, "RSI", rsiColor, linewidth=2)

// Add horizontal reference lines
hline(70, "Overbought", color.red, linestyle=hline.style_dashed)
hline(30, "Oversold", color.green, linestyle=hline.style_dashed)
hline(50, "Midline", color.gray)
```

### Strategy Declaration and Backtesting

Strategies simulate trades across historical and realtime bars with the `strategy()` declaration. They provide access to order management functions and display performance metrics in the Strategy Tester.

```pine
//@version=6
strategy("MA Crossover Strategy", overlay=true, margin_long=100, margin_short=100,
         default_qty_type=strategy.percent_of_equity, default_qty_value=10)

// Define moving average parameters
fastLength = input.int(14, "Fast MA Length", minval=1)
slowLength = input.int(28, "Slow MA Length", minval=1)

// Calculate moving averages
fastMA = ta.sma(close, fastLength)
slowMA = ta.sma(close, slowLength)

// Entry conditions
longCondition = ta.crossover(fastMA, slowMA)
shortCondition = ta.crossunder(fastMA, slowMA)

// Execute trades
if longCondition
    strategy.entry("Long", strategy.long)

if shortCondition
    strategy.entry("Short", strategy.short)

// Plot MAs
plot(fastMA, "Fast MA", color.aqua, linewidth=2)
plot(slowMA, "Slow MA", color.orange, linewidth=2)
```

### Data Visualization - plot()

The `plot()` function displays calculated data on charts with extensive customization for line styles, colors, and histograms.

```pine
//@version=6
indicator("Multi-Style Plotting", overlay=true)

// Plot different styles on highs and lows
plot(high, "Dashed High", color.blue, linewidth=1, style=plot.style_linebr,
     linestyle=plot.linestyle_dashed)
plot(low, "Circles on Low", color.red, linewidth=2, style=plot.style_circles)

// Plot colored area based on conditions
bodyCenter = math.avg(close, open)
bodyColor = close > open ? color.new(color.lime, 70) : color.new(color.red, 70)
plot(bodyCenter, "Body Center", bodyColor, linewidth=3, style=plot.style_area)

// Plot step line
plot(math.min(open, close), "Step Line", color.navy, linewidth=2,
     style=plot.style_stepline)

// Conditional coloring with lookback
ma = ta.sma(close, 20)
maColor = ma > ma[2] ? color.green : ma < ma[2] ? color.red : color.gray
plot(ma, "Colored MA", maColor, linewidth=3)
```

### Alert System - alert() and alertcondition()

Pine Script supports creating alert events that trigger 24/7 on TradingView servers. The `alert()` function provides dynamic messages and flexible triggering.

```pine
//@version=6
indicator("RSI Alert System", overlay=false)

// Calculate indicator
rsiLength = input.int(14, "RSI Length")
rsi = ta.rsi(close, rsiLength)

// Define alert conditions
oversoldCross = ta.crossover(rsi, 30)
overboughtCross = ta.crossunder(rsi, 70)

// Trigger alerts with dynamic messages
if oversoldCross
    alert("Oversold: RSI crossed above 30 at " + str.tostring(rsi, "#.00") +
          "\nPrice: " + str.tostring(close), alert.freq_once_per_bar_close)

if overboughtCross
    alert("Overbought: RSI crossed below 70 at " + str.tostring(rsi, "#.00") +
          "\nPrice: " + str.tostring(close), alert.freq_once_per_bar_close)

// Plot with markers
plot(rsi, "RSI", color.blue, linewidth=2)
plotshape(oversoldCross, "Oversold Alert", shape.triangleup, location.bottom,
          color.green, size=size.small)
plotshape(overboughtCross, "Overbought Alert", shape.triangledown, location.top,
          color.red, size=size.small)

hline(70, "Overbought", color.red)
hline(30, "Oversold", color.green)
```

### Array Operations - One-Dimensional Collections

Arrays store multiple values of the same type in a single structure, supporting dynamic sizing up to 100,000 elements.

```pine
//@version=6
indicator("Array Price History", overlay=true)

// Create array to store last 50 closes
var closePrices = array.new<float>()
var maxSize = 50

// Add confirmed closes to array
if barstate.isconfirmed
    array.unshift(closePrices, close)
    if array.size(closePrices) > maxSize
        array.pop(closePrices)

// Calculate statistics on array
if array.size(closePrices) >= maxSize
    avgPrice = array.avg(closePrices)
    maxPrice = array.max(closePrices)
    minPrice = array.min(closePrices)
    stdDev = array.stdev(closePrices)

    // Plot statistics
    plot(avgPrice, "50-Bar Average", color.blue, linewidth=2)
    plot(maxPrice, "50-Bar High", color.green, linewidth=1, style=plot.style_circles)
    plot(minPrice, "50-Bar Low", color.red, linewidth=1, style=plot.style_circles)

    // Display info in table
    var table infoTable = table.new(position.top_right, 2, 4,
                                     bgcolor=color.new(color.black, 80))
    if barstate.islast
        table.cell(infoTable, 0, 0, "Metric", text_color=color.white, text_size=size.small)
        table.cell(infoTable, 1, 0, "Value", text_color=color.white, text_size=size.small)
        table.cell(infoTable, 0, 1, "Average", text_color=color.white)
        table.cell(infoTable, 1, 1, str.tostring(avgPrice, "#.##"), text_color=color.blue)
        table.cell(infoTable, 0, 2, "High", text_color=color.white)
        table.cell(infoTable, 1, 2, str.tostring(maxPrice, "#.##"), text_color=color.green)
        table.cell(infoTable, 0, 3, "Low", text_color=color.white)
        table.cell(infoTable, 1, 3, str.tostring(minPrice, "#.##"), text_color=color.red)
```

### Map Operations - Key-Value Storage (v6)

Maps provide efficient key-value storage for string, int, or float keys mapping to any type of values.

```pine
//@version=6
indicator("Symbol Performance Map", overlay=false)

// Create map to store symbol performance
var symbolPerf = map.new<string, float>()

// Define symbols to track
symbols = array.from("AAPL", "GOOGL", "MSFT", "TSCO", "NVDA")

// Calculate and store performance for each symbol
for symbol in symbols
    symClose = request.security(symbol, timeframe.period, close)
    symOpen = request.security(symbol, timeframe.period, open)
    if not na(symClose) and not na(symOpen)
        perfPercent = ((symClose - symOpen) / symOpen) * 100
        map.put(symbolPerf, symbol, perfPercent)

// Display results in table
var table perfTable = table.new(position.middle_center, 2, array.size(symbols) + 1,
                                 bgcolor=color.new(color.white, 90))

if barstate.islast and map.size(symbolPerf) > 0
    // Header
    table.cell(perfTable, 0, 0, "Symbol", bgcolor=color.gray, text_color=color.white)
    table.cell(perfTable, 1, 0, "Performance %", bgcolor=color.gray, text_color=color.white)

    // Data rows
    int row = 1
    for symbol in symbols
        if map.contains(symbolPerf, symbol)
            perf = map.get(symbolPerf, symbol)
            cellColor = perf > 0 ? color.new(color.green, 70) : color.new(color.red, 70)
            table.cell(perfTable, 0, row, symbol, text_color=color.black)
            table.cell(perfTable, 1, row, str.tostring(perf, "#.##") + "%",
                      bgcolor=cellColor, text_color=color.white)
            row += 1
```

### Matrix Operations - Two-Dimensional Data (v6)

Matrices handle two-dimensional data structures for complex calculations like correlation matrices or multi-indicator analysis.

```pine
//@version=6
indicator("Correlation Matrix", overlay=false)

// Define periods to analyze
periods = array.from(10, 20, 50)
numPeriods = array.size(periods)

// Create matrix for correlations
var corrMatrix = matrix.new<float>(numPeriods, numPeriods, 0)

// Calculate correlations between different MA periods
if barstate.islast
    for i = 0 to numPeriods - 1
        period1 = array.get(periods, i)
        ma1 = ta.sma(close, period1)

        for j = 0 to numPeriods - 1
            period2 = array.get(periods, j)
            ma2 = ta.sma(close, period2)

            // Calculate correlation
            corr = ta.correlation(ma1, ma2, 50)
            matrix.set(corrMatrix, i, j, corr)

    // Display correlation matrix in table
    var table corrTable = table.new(position.top_right, numPeriods + 1, numPeriods + 1,
                                     bgcolor=color.white)

    // Header row and column
    table.cell(corrTable, 0, 0, "MA", bgcolor=color.gray, text_color=color.white)
    for i = 0 to numPeriods - 1
        period = array.get(periods, i)
        table.cell(corrTable, i + 1, 0, str.tostring(period),
                  bgcolor=color.gray, text_color=color.white)
        table.cell(corrTable, 0, i + 1, str.tostring(period),
                  bgcolor=color.gray, text_color=color.white)

    // Fill correlation values
    for i = 0 to numPeriods - 1
        for j = 0 to numPeriods - 1
            corr = matrix.get(corrMatrix, i, j)
            cellColor = corr > 0.8 ? color.new(color.green, 50) :
                       corr < 0.5 ? color.new(color.red, 50) :
                       color.new(color.yellow, 50)
            table.cell(corrTable, j + 1, i + 1, str.tostring(corr, "#.##"),
                      bgcolor=cellColor, text_color=color.black)
```

### Multi-Timeframe Analysis - request.security()

Request data from different timeframes or symbols to implement multi-timeframe analysis strategies.

```pine
//@version=6
indicator("MTF Trend Alignment", overlay=true)

// Get higher timeframe data safely (avoid repainting)
htfTimeframe = input.timeframe("D", "Higher Timeframe")

// Request confirmed data from higher timeframe
htfClose = request.security(syminfo.tickerid, htfTimeframe, close[1],
                            lookahead=barmerge.lookahead_on)
htfMA20 = request.security(syminfo.tickerid, htfTimeframe, ta.sma(close, 20)[1],
                          lookahead=barmerge.lookahead_on)
htfMA50 = request.security(syminfo.tickerid, htfTimeframe, ta.sma(close, 50)[1],
                          lookahead=barmerge.lookahead_on)

// Current timeframe indicators
ctfMA20 = ta.sma(close, 20)
ctfMA50 = ta.sma(close, 50)

// Determine trend alignment
htfTrend = htfMA20 > htfMA50 ? 1 : -1
ctfTrend = ctfMA20 > ctfMA50 ? 1 : -1
aligned = htfTrend == ctfTrend

// Visual feedback
bgColor = aligned ? (htfTrend > 0 ? color.new(color.green, 90) :
                                    color.new(color.red, 90)) :
                    color.new(color.gray, 95)
bgcolor(bgColor, title="Trend Alignment")

// Plot MAs
plot(ctfMA20, "Current TF MA20", color.blue, linewidth=2)
plot(ctfMA50, "Current TF MA50", color.orange, linewidth=2)
plot(htfMA20, "HTF MA20", color.new(color.blue, 50), linewidth=1,
     style=plot.style_circles)
plot(htfMA50, "HTF MA50", color.new(color.orange, 50), linewidth=1,
     style=plot.style_circles)

// Display status
var label statusLabel = na
if barstate.islast
    labelText = "HTF: " + htfTimeframe + "\n" +
                "Alignment: " + (aligned ? "✓" : "✗") + "\n" +
                "HTF Trend: " + (htfTrend > 0 ? "Bullish" : "Bearish") + "\n" +
                "CTF Trend: " + (ctfTrend > 0 ? "Bullish" : "Bearish")
    labelColor = aligned ? (htfTrend > 0 ? color.green : color.red) : color.gray
    label.delete(statusLabel)
    statusLabel := label.new(bar_index, high, labelText,
                             color=labelColor, textcolor=color.white,
                             style=label.style_label_down)
```

### User-Defined Types and Objects

Create custom data structures to organize complex data and implement object-oriented patterns.

```pine
//@version=6
indicator("Trade Signal Manager", overlay=true)

// Define custom type for trade signals
type TradeSignal
    int barIndex
    float price
    string direction
    float stopLoss
    float takeProfit
    bool active

// Create signal storage
var signals = array.new<TradeSignal>()

// Function to create new signal
createSignal(string dir, float sl, float tp) =>
    signal = TradeSignal.new(
        barIndex = bar_index,
        price = close,
        direction = dir,
        stopLoss = sl,
        takeProfit = tp,
        active = true
    )
    array.push(signals, signal)
    signal

// Detect signals
longSignal = ta.crossover(ta.rsi(close, 14), 30)
shortSignal = ta.crossunder(ta.rsi(close, 14), 70)

// Create signals with risk management
if longSignal
    sl = close - ta.atr(14) * 2
    tp = close + ta.atr(14) * 3
    sig = createSignal("LONG", sl, tp)
    label.new(sig.barIndex, sig.price, "L\nSL:" + str.tostring(sig.stopLoss, "#.##") +
              "\nTP:" + str.tostring(sig.takeProfit, "#.##"),
              color=color.green, textcolor=color.white, style=label.style_label_up)

if shortSignal
    sl = close + ta.atr(14) * 2
    tp = close - ta.atr(14) * 3
    sig = createSignal("SHORT", sl, tp)
    label.new(sig.barIndex, sig.price, "S\nSL:" + str.tostring(sig.stopLoss, "#.##") +
              "\nTP:" + str.tostring(sig.takeProfit, "#.##"),
              color=color.red, textcolor=color.white, style=label.style_label_down)

// Check active signals for exits
for i = 0 to array.size(signals) - 1
    signal = array.get(signals, i)
    if signal.active
        if signal.direction == "LONG"
            if close <= signal.stopLoss or close >= signal.takeProfit
                signal.active := false
                exitLabel = close >= signal.takeProfit ? "TP✓" : "SL✗"
                exitColor = close >= signal.takeProfit ? color.green : color.red
                label.new(bar_index, close, exitLabel, color=exitColor,
                         textcolor=color.white, size=size.small)
        else if signal.direction == "SHORT"
            if close >= signal.stopLoss or close <= signal.takeProfit
                signal.active := false
                exitLabel = close <= signal.takeProfit ? "TP✓" : "SL✗"
                exitColor = close <= signal.takeProfit ? color.green : color.red
                label.new(bar_index, close, exitLabel, color=exitColor,
                         textcolor=color.white, size=size.small)
```

### Avoiding Repainting - Confirmed Data Pattern

Repainting occurs when historical calculations differ from realtime behavior. Always use confirmed data for reliable signals.

```pine
//@version=6
indicator("Non-Repainting Signals", overlay=true)

// Calculate indicators using confirmed data
rsi = ta.rsi(close, 14)
ma = ta.sma(close, 20)

// Method 1: Use historical offset [1]
confirmedRSI = rsi[1]
confirmedMA = ma[1]
confirmedClose = close[1]

// Method 2: Use barstate.isconfirmed
realtimeSignal = ta.crossover(close, ma)
confirmedSignal = realtimeSignal and barstate.isconfirmed

// Method 3: Safe multi-timeframe data
dailyClose = request.security(syminfo.tickerid, "D", close[1],
                              lookahead=barmerge.lookahead_on)

// Generate non-repainting buy signal
buyCondition = confirmedClose > confirmedMA and confirmedRSI < 30
if buyCondition and barstate.isconfirmed
    label.new(bar_index, low, "BUY", color=color.green,
             textcolor=color.white, style=label.style_label_up, size=size.small)
    alert("Buy Signal Confirmed at " + str.tostring(close), alert.freq_once_per_bar_close)

// Generate non-repainting sell signal
sellCondition = confirmedClose < confirmedMA and confirmedRSI > 70
if sellCondition and barstate.isconfirmed
    label.new(bar_index, high, "SELL", color=color.red,
             textcolor=color.white, style=label.style_label_down, size=size.small)
    alert("Sell Signal Confirmed at " + str.tostring(close), alert.freq_once_per_bar_close)

// Plot with proper coloring
plot(ma, "MA(20)", color.blue, linewidth=2)
plotshape(buyCondition and barstate.isconfirmed, "Buy", shape.triangleup,
         location.belowbar, color.green, size=size.small)
plotshape(sellCondition and barstate.isconfirmed, "Sell", shape.triangledown,
         location.abovebar, color.red, size=size.small)
```

### Performance Optimization - Efficient Calculations

Optimize script performance by caching calculations, limiting array sizes, and using built-in functions.

```pine
//@version=6
indicator("Optimized Multi-Indicator", overlay=false)

// Cache expensive calculations with var
var cachedResults = array.new<float>()
var int calculationInterval = 10

// Only recalculate every N bars
if bar_index % calculationInterval == 0
    array.clear(cachedResults)

    // Batch calculations
    for i = 5 to 50 by 5
        rsi = ta.rsi(close, i)
        array.push(cachedResults, rsi)

// Use built-in functions instead of loops
fastAvg = ta.sma(close, 10)  // Fast built-in
slowAvg = ta.sma(close, 50)

// Efficient array operations
if array.size(cachedResults) > 0
    avgRSI = array.avg(cachedResults)
    maxRSI = array.max(cachedResults)
    minRSI = array.min(cachedResults)

    plot(avgRSI, "Average RSI", color.blue, linewidth=2)
    plot(maxRSI, "Max RSI", color.green, linewidth=1)
    plot(minRSI, "Min RSI", color.red, linewidth=1)

// Limit historical data storage
var priceHistory = array.new<float>()
var maxHistorySize = 100

if barstate.isconfirmed
    array.push(priceHistory, close)
    if array.size(priceHistory) > maxHistorySize
        array.shift(priceHistory)  // Remove oldest element

hline(50, "Midline", color.gray)
```

---

## Summary and Integration

Pine Script v6 provides a complete toolkit for developing sophisticated trading indicators and strategies on TradingView. The language's execution model processes scripts bar-by-bar across historical data and realtime updates, requiring careful attention to repainting prevention through confirmed data patterns and proper use of `barstate` variables. Core visualization functions like `plot()`, combined with drawing objects (lines, labels, boxes) and tables, enable rich chart overlays and separate indicator panes.

The v6 release introduced powerful collection types—maps for key-value storage and matrices for two-dimensional data—complementing existing arrays for one-dimensional collections. These structures, combined with user-defined types and methods, enable object-oriented programming patterns for complex indicator systems. Multi-timeframe analysis through `request.security()` allows strategy validation across different time scales, while the alert system enables 24/7 monitoring of trading conditions. Performance optimization through calculation caching, efficient built-in functions, and proper array sizing ensures scripts run smoothly even with extensive historical data processing. The comprehensive anti-pattern library and decision trees included in this documentation help developers avoid common pitfalls and make optimal architectural choices for reliable, production-ready trading tools.
