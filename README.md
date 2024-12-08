# Quantitative Trading Framework

A comprehensive quantitative trading framework that implements advanced market analysis, machine learning-based signal generation, and robust risk management.

## Features

- **Market Regime Detection**: Uses Gaussian Mixture Models to identify and classify market states
- **ML-Based Signal Generation**: Employs gradient boosting for market prediction
- **Advanced Risk Management**: Implements VaR, Expected Shortfall, and dynamic position sizing
- **Performance Monitoring**: Tracks and analyzes trading performance with detailed metrics
- **Modular Design**: Easily extensible architecture for adding new components

## Installation

1. Clone the repository:
```bash
git clone https://github.com/yourusername/quant-trading.git
cd quant-trading
```

2. Create and activate a virtual environment:
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

3. Install dependencies:
```bash
pip install -r requirements.txt
```

## Usage

### Basic Example

```python
from quant_trading import DataLoader, TradingStrategy

# Load historical data
data_loader = DataLoader('price_data.csv', 'sentiment_data.csv')
data = data_loader.load_data()
data = data_loader.preprocess_data(data)

# Initialize strategy
strategy = TradingStrategy(
    initial_capital=100000,
    symbol='AAPL',
    max_position_size=0.2,
    max_drawdown=0.15
)

# Split data and train models
train_data, test_data = data_loader.get_train_test_split(data)
metrics = strategy.train(train_data)

# Trading loop
for timestamp, row in test_data.iterrows():
    # Update market regime
    strategy.update_regime(data.loc[:timestamp])
    
    # Generate trading signal
    signal = strategy.generate_signal(data.loc[:timestamp])
    
    # Execute trade if necessary
    strategy.execute_trade(
        timestamp=timestamp,
        signal=signal,
        current_price=row['Close'],
        volatility=row['Volatility']
    )
    
    # Update performance metrics
    if timestamp > data.index[0]:
        strategy.update_performance(
            timestamp=timestamp,
            current_price=row['Close'],
            previous_price=data.loc[data.index[data.index < timestamp][-1], 'Close']
        )

# Generate performance report
report = strategy.generate_report('performance_report.txt')
strategy.plot_performance('equity_curve.png')
```

## Project Structure

```
quant_trading/
├── data/
│   └── data_loader.py
├── models/
│   ├── regime_detector.py
│   └── ml_signal_generator.py
├── risk/
│   └── risk_manager.py
├── monitoring/
│   └── performance_monitor.py
├── strategies/
│   └── trading_strategy.py
└── __init__.py
```

## Components

### Data Loader
- Loads and preprocesses market data
- Calculates technical indicators
- Handles data splitting for training/testing

### Regime Detector
- Identifies market regimes using Gaussian Mixture Models
- Classifies regimes based on return and volatility characteristics
- Provides regime-specific trading parameters

### ML Signal Generator
- Generates trading signals using gradient boosting
- Incorporates multiple features including technical indicators and sentiment
- Provides probability-based signal strength

### Risk Manager
- Implements position sizing based on multiple factors
- Calculates and monitors risk metrics (VaR, ES)
- Manages capital allocation and drawdown limits

### Performance Monitor
- Tracks trading performance metrics
- Generates detailed performance reports
- Creates visualizations of equity curve and drawdowns

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Disclaimer

This software is for educational purposes only. Do not use it to trade real money without thoroughly understanding the risks involved in financial markets. Past performance is not indicative of future results.
# srt-model-quantizing
