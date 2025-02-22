# The-algorithm-
Sales algorithms 
import requests
from bs4 import BeautifulSoup
url = "https://www.reddit.com/r/hiphopheads/"
headers = { "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36" }
response = requests.get(url, headers=headers)
soup = BeautifulSoup(response.text, "html.parser")