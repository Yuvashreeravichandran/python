import urllib.request
from bs4 import BeautifulSoup
from sklearn.feature_extraction.text import CountVectorizer 
import nltk
from nltk.collocations import *
link = "https://en.wikipedia.org/wiki/Category:Artificial_intelligence"
response = urllib.request.urlopen(link)
the_page = response.read()
response.close
soup = BeautifulSoup(the_page)
print (soup.prettify())
print (soup.title.string)
for div in soup.findAll('div', {'class': 'mw-category-generated'}):
    for x in div.find_all("x"):
        print (x)
        print (x.attrs['href'])
print(soup.get_text())
text = soup.get_text()
vectorizer = CountVectorizer(ngram_range=(1,5))
analyzer = vectorizer.build_analyzer()
print (analyzer(text))
bigram_measures = nltk.collocations.BigramAssocMeasures()
trigram_measures = nltk.collocations.TrigramAssocMeasures()
tokens = nltk.wordpunct_tokenize(text)
finder = BigramCollocationFinder.from_words(tokens)
finder.apply_freq_filter(2)
scored = finder.score_ngrams(bigram_measures.raw_freq)
print(sorted(bigram for bigram, score in scored))
