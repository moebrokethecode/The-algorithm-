import tweepy
import pandas as pd
import re
auth = tweepy.OAuthHandler("7t1BWZ6YiSoUt0GH2FUTPCb20", "qHJEUTmN2jNvhixcEPKPJSb7YTNxSKLwV1OVyAdZXoGoEssuIc")
auth.set_access_token("1893171909791227909-Snejb5qC1VGH1Quyf0jWPohnlRfcuA", "bl8UVOi085Cwl1MzI8qxIE3KAy5URZmjCC2xtlr7T0DdK")
api = tweepy.API(auth)

# Hip-hop keywords
enthusiasm_keywords = ["hip-hop", "rap", "Kendrick", "Drake", "vinyl", "mixtape"]
purchase_keywords = ["bought", "copped", "ordered", "vinyl arrived"]

# Function to score a user
def score_user(username):
    enthusiasm_score = 0
    purchase_score = 0
    
    # Get user bio and recent posts
    try:
        user = api.get_user(screen_name=username)
        bio = user.description.lower()
        tweets = api.user_timeline(screen_name=username, count=50, tweet_mode="extended")
    except Exception as e:
        print(f"Error with {username}: {e}")
        return None

    # Check bio for enthusiasm
    if any(keyword in bio for keyword in enthusiasm_keywords):
        enthusiasm_score += 10

    # Analyze tweets
    for tweet in tweets:
        text = tweet.full_text.lower()
        if any(keyword in text for keyword in enthusiasm_keywords):
            enthusiasm_score += 5
        if any(keyword in text for keyword in purchase_keywords):
            purchase_score += 20

    # Check following count for hip-hop artists (simplified)
    follows = api.get_friends(screen_name=username, count=50)
    if len([f for f in follows if "music" in f.description.lower()]) > 5:
        enthusiasm_score += 15

    total_score = min(enthusiasm_score + purchase_score, 100)
    return total_score if total_score >= 60 else None

# Test with a list of users
users = ["HipHopJunkie88", "RapFiend22"]  # Replace with real usernames
results = []

for user in users:
    score = score_user(user)
    if score:
        results.append({"username": user, "score": score})

# Save results
df = pd.DataFrame(results)
df.to_csv("hip_hop_buyers.csv", index=False)
print(df)
​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​