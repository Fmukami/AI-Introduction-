BOT_NAME = "CryptoBuddy"
WELCOME_MSG = f"👋 Hey, I’m {BOT_NAME}! I’m here to help you find a green and growing crypto. 🌱"
DISCLAIMER = "⚠️ Crypto is risky—always do your own research!"


crypto_db = {  
    "Bitcoin": {  
        "symbol": "BTC",
        "price_trend": "rising",  
        "market_cap": "high",  
        "energy_use": "high",  
        "sustainability_score": 3/10  
    },  
    "Ethereum": {  
        "symbol": "ETH",
        "price_trend": "stable",  
        "market_cap": "high",  
        "energy_use": "medium",  
        "sustainability_score": 6/10  
    },  
    "Cardano": {  
        "symbol": "ADA",
        "price_trend": "rising",  
        "market_cap": "medium",  
        "energy_use": "low",  
        "sustainability_score": 8/10  
    }  
}

def respond(user_query):
    user_query = user_query.lower()
    # Sustainability
    if "sustainable" in user_query or "eco" in user_query:
        recommend = max(crypto_db, key=lambda x: crypto_db[x]["sustainability_score"])
        c = crypto_db[recommend]
        return f"Invest in {recommend} ({c['symbol']})! 🌱 It’s eco-friendly and has long-term potential!\n{DISCLAIMER}"
    # Trending up or profitable
    elif "trending up" in user_query or "rising" in user_query or "profit" in user_query:
        candidates = [k for k,v in crypto_db.items() if v["price_trend"] == "rising" and v["market_cap"] == "high"]
        if candidates:
            c = crypto_db[candidates[0]]
            return f"{candidates[0]} ({c['symbol']}) is rising and has a high market cap! 🚀\n{DISCLAIMER}"
        else:
            # Pick any coin with price_trend == rising
            candidates = [k for k,v in crypto_db.items() if v["price_trend"] == "rising"]
            if candidates:
                c = crypto_db[candidates[0]]
                return f"{candidates[0]} ({c['symbol']}) is trending up! 🚀\n{DISCLAIMER}"
            else:
                return "No coins are trending up right now. Markets are tough! 😅"
    # Long-term growth
    elif "long-term" in user_query or "hold" in user_query or "growth" in user_query:
        # Prefer high sustainability and rising
        candidates = [k for k,v in crypto_db.items() if v["price_trend"] == "rising" and v["sustainability_score"] > 0.7]
        if candidates:
            c = crypto_db[candidates[0]]
            return f"{candidates[0]} ({c['symbol']}) is trending up and has a top-tier sustainability score! 🚀\n{DISCLAIMER}"
        else:
            return "I'd recommend researching more before making a long-term crypto investment! 🤔"
    # Specific coin info
    for coin in crypto_db:
        if coin.lower() in user_query or crypto_db[coin]['symbol'].lower() in user_query:
            c = crypto_db[coin]
            return (f"{coin} ({c['symbol']}):\n"
                    f"- Price Trend: {c['price_trend']}\n"
                    f"- Market Cap: {c['market_cap']}\n"
                    f"- Energy Use: {c['energy_use']}\n"
                    f"- Sustainability Score: {c['sustainability_score']*10}/10\n"
                    f"{DISCLAIMER}")
    # Help
    if "help" in user_query or "what can you do" in user_query:
        return (f"I can recommend cryptos based on sustainability, trends, and profitability. "
                "Try asking:\n- Which crypto is trending up?\n- What’s the most sustainable coin?\n"
                "- Tell me about Ethereum.\n- Which coin should I hold for long-term growth?")
    # Fallback
    return f"Sorry, I didn’t get that. Try asking me about trending, sustainable, or profitable coins! {DISCLAIMER}"


def main():
    print(WELCOME_MSG)
    print("Type 'exit' to quit.")
    while True:
        user_query = input("You: ")
        if user_query.strip().lower() == "exit":
            print(f"{BOT_NAME}: Goodbye and happy investing! 🚀")
            break
        reply = respond(user_query)
        print(f"{BOT_NAME}: {reply}")

if __name__ == "__main__":
    main()
