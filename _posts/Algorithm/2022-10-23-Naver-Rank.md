---
layout: post
title: '[Python] Naver-Rank'
excerpt: 네이버 영화 랭킹 댓글
category: Algorithm
tags:
  - Algorithm
---

```py
import requests
from bs4 import BeautifulSoup

class reple:
    def __init__(self, score, chatting, gong, bigong):
        self.score = score
        self.chatting = chatting
        self.gong = gong
        self.bigong = bigong
    def __str__(self):
        return "별점%s,공감:%s,비공감:%s" % (self.score, self.gong, self.bigong)
class MovieInfo:
    def __init__(self, rank, title, reple):
        self.rank = rank
        self.title = title
        self.reple = reple
    def __str__(self):
        return "%d등 : %s\n%s" % (self.rank, self.title, self.reple)

class Naver:
    def site(self, url):
        response = requests.get('https://movie.naver.com{}'.format(url))
        html = response.text
        soup = BeautifulSoup(html, 'html.parser')
        return soup

    def main(self, tag, ranking):
        rlist = []
        url = tag.get('href')
        soup = self.site(url)
        for chat in soup.select('div[class=score_result] ul li'):
            score = "평점 : {}".format(chat.select('div[class=star_score] em')[0].text)
            chatting = "댓글 : {}".format(chat.select('div[class=score_reple] p')[0].text.strip())
            area = chat.select('div[class=btn_area]')[0]
            gong = area.select('a[class=_sympathyButton] strong')[0].text
            bigong = area.select('a[class=_notSympathyButton] strong')[0].text
            rr = reple(score, chatting, gong, bigong)
            rlist.append(rr)
        return MovieInfo(ranking, tag.text, rlist)
    def movieRank(self):
        main_url = '/movie/sdb/rank/rmovie.nhn'
        soup = self.site(main_url)
        alist = []
        ranking = 1
        for tag in soup.select('div[class=tit3] a'):
            alist.append(self.main(tag, ranking))
            ranking = ranking + 1
        return alist

if __name__ == "__main__":
    a=Naver()
    cc=a.movieRank()
    for b in cc:
        print(b)
```
