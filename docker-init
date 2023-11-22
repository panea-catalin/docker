from flask import Flask, render_template
import requests
import plotly.express as px
import plotly
import json
from datetime import datetime, timedelta

app = Flask(__name__)

def fetch_btc_prices():
    current_time = datetime.now()
    one_hour_ago = current_time - timedelta(hours=1)
    url = "https://api.coingecko.com/api/v3/coins/bitcoin/market_chart/range"
    params = {
        "vs_currency": "usd",
        "from": one_hour_ago.timestamp(),
        "to": current_time.timestamp()
    }
    response = requests.get(url, params=params)
    data = response.json()
    prices = data["prices"]
    return prices

def plot_prices(prices):
    times = [datetime.fromtimestamp(time/1000) for time, price in prices]
    values = [price for time, price in prices]
    fig = px.line(x=times, y=values, labels={'x': 'Time', 'y': 'Price (USD)'}, title='BTC Price in the Last Hour')
    fig.update_traces(mode='lines+markers')
    graphJSON = json.dumps(fig, cls=plotly.utils.PlotlyJSONEncoder)
    return graphJSON

@app.route('/')
def btc_chart():
    prices = fetch_btc_prices()
    graphJSON = plot_prices(prices)
    return render_template("chart.html", graphJSON=graphJSON)

if __name__ == '__main__':
    app.run(debug=True)
