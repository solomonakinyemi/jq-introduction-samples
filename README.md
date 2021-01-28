DEMO PLAN
- Install docker on your PC's if you want to participate
- The following image is available on Docker hub: 
	- docker run imega/jq
	- How to make a JQ query with the image: 
		- cat <file name> | docker run --rm -i imega/jq .

** USE JQ with Python try the following module
- https://github.com/mwilliamson/jq.py

Small Dataset Query
---
Example 1:
Question: 
- Get all firstnames 
JQ Query:
```
cat 1_simple.json | jq ".[].firstname"
```

Container Query: 
```
cat 1_simple.json | docker run --rm -i imega/jq .
```

Example 2:
Question: 
- Get all firstname and age elements and insert output into a list
JQ Query:
```
cat 1_simple.json | jq "[.[].age]"
```

Container Query: 
```
cat 1_simple.json | docker run --rm -i imega/jq .
```

Example 3:
Question: 
- Get all firstname and age elements  inserting the output into a dictionary with (key: value)
JQ Query:
```
cat 1_simple.json | jq ".[]| {(.firstname): (.age)}"
```

Container Query: 
```
cat 1_simple.json | docker run --rm -i imega/jq '.[]| {(.firstname): (.age)}'
```

Example 4:
Question: 
- Get all genres with funky music type
JQ Query:
```
cat 2_select_example.json | jq '.[] | select(.genre | . and contains("funky"))'
```

Example 5:
Question: 
- Can I do arithmetic operations with JQ?
JQ Query:
```
echo "[1,2,3]" | jq '.[] + 2'
```


Larger Dataset Query (Spotify Data)
---
https://developer.spotify.com/console/get-several-albums/
- Get several Albums

Example 7:
Question: 
- How many albums are in the dataset?
JQ Query:
```
cat 3_album.json | jq '.albums | length'
```


Example 8:
Question: 
- What are the names of the albums in the dataset?
JQ Query:
```
cat 3_album.json | jq '.albums[].name'
```

Example 9:
Question: 
1. Get the names of the songs on the first album
2. Get number of artists on the album 
3. Get information (name, release date, popularity, total_tracks and album name) from the first album
	- Do it for all albums in the dataset 
4. Get information from '3' and create new JSON with result
5. Throw in the track names into the dictionary

** Do '5' in the docker container: 
 


JQ Query:
```
1. cat 3_album.json | jq '.albums[0].tracks.items[].name'

2. cat 3_album.json | jq '.albums[0].artists[] | length'

3. cat 3_album.json | jq '[.[][] | .name, .release_date, .popularity, .total_tracks]'

3b. cat 3_album.json | jq '[.albums[].release_date, .albums[].popularity, .albums[].total_tracks, .albums[].name]

4. cat 3_album.json | jq '[.[][] | {"name": .name, "release": .release_date, "popularity": .popularity, "tracks": .total_tracks}]'

5. cat 3_album.json | jq '[.[][] | {"name": .name, "release": .release_date, "popularity": .popularity, "tracks": .total_tracks, "track_names": [.tracks.items[] | (.name)]}]'


** 
cat 3_album.json | docker run --rm -i imega/jq '[.[][] | {"name": .name, "release": .release_date, "popularity": .popularity, "tracks": .total_tracks, "track_names": [.tracks.items[] | (.name)]}]'
```
