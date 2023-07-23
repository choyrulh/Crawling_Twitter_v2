# Crawling_Twitter_v2
Crawling data Twitter versi tweepy 4.10 atau API v2

Sebelumnya gunakan Python versi 3.9 atau ke atas
Kemudian install library 
`
pip install tweepy 4.10
`
Mendaftarkan akun ke https://developer.twitter.com/en untuk mendapatkan kode API

Kemudian masukkan juga library yang dibutuhkan seperti `time` untuk menggunakan jeda waktu

Gunakan library MySQL untuk menyimpan ke dalam bentuk database MySQL
``` python
....
connection = pymysql.connect(host='localhost',
                             user='root',
                             password='',
                             db='dataset',
                             charset='utf8mb4',
                             cursorclass=pymysql.cursors.DictCursor)
cursor = connection.cursor()

....

 # Insert the tweet into the database
        cursor.execute("INSERT INTO live (tweet) VALUES (%s)", (tweet.text))
        connection.commit()


```


Kemudian import library "csv" jika ingin mengonversi ke file berekstensi csv

``` python
# convert ke csv
            with open('tweet1.csv', 'a') as f:
                writer = csv.DictWriter(f, fieldnames=["id", "tweet"])
                if f.tell() == 0:
                    writer.writeheader()
                writer.writerow({"id": tweet.id, "tweet": tweet.text})
                f.close()
```

## Validasi kode API
``` python 
API_KEY = 'bX3KXcs1dwxuGCfdijrYT8vu8'
API_SECRET_KEY = 'DIL9aSloftiOPV47NHz3v3Pn7c6ZcF1nSajX33Yh18WBMZqqp4'
BEARER_TOKEN = "AAAAAAAAAAAAAAAAAAAAAJ2HfgEAAAAAiFHFDU4OnsKObvqB1hOXVS%2Fu0yg%3Dxn7DFKh8V3sRWOPux2RESHWLTe2N91SE7NZOv1TnHh9i029Yt7"

ACCESS_TOKEN = '1555520320068390912-szmb9tH8gJoZ97QMNQ9BMaoYpBBWPl'
ACCESS_TOKEN_SECRET = 'ewvsHwR939e6PTjoXTE6SF8YoKTDgyjFjpthNmoctTQ9q'

client = tweepy.Client(BEARER_TOKEN, API_KEY, API_SECRET_KEY, ACCESS_TOKEN, ACCESS_TOKEN_SECRET)

auth = tweepy.OAuth1UserHandler(API_KEY, API_SECRET_KEY, ACCESS_TOKEN, ACCESS_TOKEN_SECRET)
api = tweepy.API(auth,wait_on_rate_limit=True)

```
## Tweet yang ingin dicari berdasarkan keyword
``` python 
search_terms = ["Anies Baswedan"]
```

## Inisiasi Stream crawling Twitter
```python 
# Bot searches for tweets containing certain keywords
class MyStream(tweepy.StreamingClient):

    # This function gets called when the stream is working
    def on_connect(self):
        print("Connected")

    # This function gets called when a tweet passes the stream
    def on_tweet(self, tweet):

        # Displaying tweet in console
        if tweet.referenced_tweets == None:
            print(tweet.text)
            client.like(tweet.id)
            tweets.append(tweet)

            # Delay between tweets
            time.sleep(1)

# Creating Stream object
stream = MyStream(bearer_token=BEARER_TOKEN)

# Adding terms to search rules
# It's important to know that these rules don't get deleted when you stop the
# program, so you'd need to use to change them, or you can use the optional parameter to stream.add_rules()
# called dry_run (set it to True, and the rules will get deleted after the bot
# stopped running).
for term in search_terms:
   stream.add_rules(tweepy.StreamRule(term))

#jika mau tambah atau ubah atau hapus value search termsnya, secara manual harus melakukan delet rules dan uncomment script di bawah   
#for rule in stream.get_rules()[0]:
#   stream.delete_rules(rule.id)


stream.filter(tweet_fields=["referenced_tweets"])

```

## Tambahkan script berikut jika ingin mengubah huruf besar ke kecil
```python
 def preprocess_tweet(text):
        text = text.lower()
        return text
...
        text = preprocess_tweet(tweet.text)
        #text = tweet.text
...
```
