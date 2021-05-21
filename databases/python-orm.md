---
title: Python ORM
description: 
published: true
date: 2021-05-21T11:21:22.925Z
tags: 
editor: markdown
dateCreated: 2021-05-21T11:21:22.925Z
---

# Scraping YTS using <kbd>sqlalchemy</kbd>
```python
import json
from typing import List, Dict

import sqlalchemy
import tqdm
from sqlalchemy import Column, String, Integer, Float, ForeignKey
from sqlalchemy.engine import Engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import relationship
from sqlalchemy.orm import sessionmaker
from sqlalchemy.orm.session import Session

engine: Engine = sqlalchemy.create_engine("sqlite:///yts.sqlite3", echo=False)

session: Session = sessionmaker(bind=engine)()

Base = declarative_base()


class Movie(Base):
    __tablename__ = "movies"

    id = Column(Integer, primary_key=True)
    url = Column(String)
    imdbCode = Column(String)
    title = Column(String)
    titleEnglish = Column(String)
    titleLong = Column(String)
    slug = Column(String)
    year = Column(Integer)
    rating = Column(Float)
    runtime = Column(Integer)

    summary = Column(String)
    descriptionFull = Column(String)
    synopsis = Column(String)
    ytTrailerCode = Column(String)
    language = Column(String)
    mpaRating = Column(String)
    backgroundImage = Column(String)
    backgroundImageOriginal = Column(String)
    smallCoverImage = Column(String)
    mediumCoverImage = Column(String)
    largeCoverImage = Column(String)
    state = Column(String)
    torrents: List["TorrentLink"] = relationship("TorrentLink", back_populates="movie")

    def __init__(self, json_data: Dict[str, any]):
        self.id = json_data["id"]
        self.url = json_data["url"]
        self.imdbCode = json_data["imdb_code"]
        self.title = json_data["title"]
        self.titleEnglish = json_data["title_english"]
        self.titleLong = json_data["title_long"]
        self.slug = json_data["slug"]
        self.year = json_data["year"]
        self.rating = json_data["rating"]
        self.runtime = json_data["runtime"]
        self.summary = json_data["summary"]
        self.descriptionFull = json_data["description_full"]
        self.synopsis = json_data["synopsis"]
        self.ytTrailerCode = json_data["yt_trailer_code"]
        self.language = json_data["language"]
        self.mpaRating = json_data["mpa_rating"]
        self.backgroundImage = json_data["background_image"]
        self.backgroundImageOriginal = json_data["background_image_original"]
        self.smallCoverImage = json_data["small_cover_image"]
        self.mediumCoverImage = json_data["medium_cover_image"]
        self.largeCoverImage = json_data["large_cover_image"]
        self.state = json_data["state"]

    def __repr__(self):
        return f"<Movie(id={self.id}, name={self.title}, rating={self.rating})>"


class TorrentLink(Base):
    __tablename__ = "torrents"

    id = Column(Integer, primary_key=True)
    url = Column(String)
    hash = Column(String)
    quality = Column(String)
    type = Column(String)
    seeds = Column(Integer)
    peers = Column(Integer)
    size = Column(String)
    sizeBytes = Column(Integer)
    movie_id = Column(Integer, ForeignKey("movies.id"))
    movie = relationship("Movie", back_populates="torrents")

    def __init__(self, json: Dict[str, any]):
        self.url = json["url"]
        self.hash = json["hash"]
        self.quality = json["quality"]
        self.type = json["type"]
        self.seeds = json["seeds"]
        self.peers = json["peers"]
        self.size = json["size"]
        self.sizeBytes = json["size_bytes"]

    def __repr__(self):
        return f"<TorrentLink(quality={self.quality}, seeds={self.seeds})>"


Base.metadata.create_all(engine)

# Load data
for current_page in tqdm.tqdm(range(1, 606)):
    with open(f"scraped_data/page{current_page}.json") as f:
        data = json.load(f)

    torrent_link_list: List[TorrentLink] = []
    movie_list = []
    for movie in data["data"]["movies"]:
        new_movie = Movie(movie)
        try:
            for torrent in movie["torrents"]:
                temp = TorrentLink(torrent)
                temp.movie_id = new_movie.id
                new_movie.torrents.append(temp)
        except KeyError as e:
            print("Failed : ", new_movie.title, new_movie.id)
            print(e)

        session.merge(new_movie)

    session.commit()
```