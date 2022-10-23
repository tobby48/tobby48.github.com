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

class info():
    def __init__(self,score,chatting,area):
        self.score = score
        self.chatting = chatting
        self.area = area
    def comment(self):
        aa=[self.score,self.chatting,self.area]
        return "\n".join(aa)

def main(tag, ranking):
    rlist=[]
    rlist.append(str(ranking) + '위 : ' + tag.text)
    url = tag.get('href')
    soup = site(url)
    for chat in soup.select('div[class=score_result] ul li'):
        score = "평점 : {}" .format(chat.select('div[class=star_score] em')[0].text)
        chatting = "댓글 : {}" .format(chat.select('div[class=score_reple] p')[0].text.strip())
        area = chat.select('div[class=btn_area]')[0].text.replace("\n","")
        movie = info(score,chatting,area)
        rlist.append(movie.comment())
    return "\n".join(rlist)

def run():
    main_url = '/movie/sdb/rank/rmovie.nhn'
    soup = site(main_url)
    alist=[]
    ranking = 1
    for tag in soup.select('div[class=tit3] a'):
        alist.append(main(tag,ranking))
        ranking = ranking + 1
    return "\n".join(alist)

def site(url):
    response = requests.get('https://movie.naver.com{}'.format(url))
    html = response.text
    soup = BeautifulSoup(html, 'html.parser')
    return soup

if __name__ == "__main__":
    r=run()
    print(r) #약 20초정도 걸려요
```
