---
layout: post
title: 'Get Started With Beautiful Soup'
tags: ['beautiful', 'soup', 'bs4', 'python', 'web', 'scraper']
---
Here's a code snippet in Python to get started with web scraping and the library Beautiful Soup.

```bash
pip install requests beautifulsoup4
```

```python
import requests
from bs4 import BeautifulSoup

page = requests.get('https://some-url/')
soup = BeautifulSoup(page.text, 'html.parser')

links = soup.find_all('a', class_='links')

for link in links:
    id = link.get('id')
    href = link.get('href')
    text = link.get_text()
```

{% if jekyll.environment == 'production' %}
  {% include responsive-ad.html %}
{% endif %}
