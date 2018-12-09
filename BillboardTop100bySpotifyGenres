import os
import json
import spotipy
import spotipy.util as util

import pandas as pd
import billboard as bb

# Personal Spotify Username '#########'
username = ''

# Spoitfy Credentials
scope = 'user-read-recently-played user-top-read user-read-playback-state'
client_id = ''
client_secret = ''
redirect_uri = ''

# Output file path
output_file = 'bb+sptoify.csv'


try:
    token = util.prompt_for_user_token(
            username=username,
            scope=scope,
            client_id=client_id,
            client_secret=client_secret,
            redirect_uri=redirect_uri
            )
except:
    os.remove(f".cache-{username}")
    token = util.prompt_for_user_token(
            username=username,
            scope=scope,
            client_id=client_id,
            client_secret=client_secret,
            redirect_uri=redirect_uri
            )

# Spotify Object
spotifyObject = spotipy.Spotify(auth=token)

# Empty Data Frame for Output file
chart_df = pd.DataFrame(columns=['date','rank', 'song',
                                 'full_artist', 'main_artist', 'artist2', 'artist3', 'artist4', 'artist5',
                                 'genre1', 'genre2', 'genre3', 'genre4', 'genre5',
                                 'peakPos', 'lastPos', 'weeks', 'isNew']
                       )
chart = bb.ChartData('hot-100')

# Search Song
#track = 'I Like It'
# song.title

#search song

for song in chart:

    track_query = spotifyObject.search(song.title,50,0,'track') # search limit 50 to find common track names
    sp_tracks = track_query['tracks']['items']

    main_artist = ''
    artist2 = ''
    artist3 = ''
    artist4 = ''
    artist5 = ''

    genre1 = ''
    genre2 = ''
    genre3 = ''
    genre4 = ''
    genre5 = ''


    for sp_track in sp_tracks:

        artist_count = 0
        artist_check = 0

        sp_artists = sp_track['artists']

        for a in sp_artists:
            artist_count+=1

            if a['name'].lower() in song.artist.lower():
                artist_check+=1

        if artist_count == artist_check:

            index = 0

            for a in sp_artists:

                if index == 0:
                    main_artist = a['name']
                    artist_query = spotifyObject.search(main_artist,1,0,'artist')

                    genres = artist_query['artists']['items'][0]['genres']

                    gindex = 0
                    for genre in genres:

                        if gindex == 0:
                            genre1 = genre
                        if gindex == 1:
                            genre2 = genre
                        if gindex == 2:
                            genre3 = genre
                        if gindex == 3:
                            genre4 = genre
                        if gindex == 4:
                            genre5 = genre
                        gindex+=1

                if index == 1:
                    artist2 = a['name']
                if index == 2:
                    artist3 = a['name']
                if index == 3:
                    artist4 = a['name']
                if index == 4:
                    artist5 = a['name']
                index+=1

    chart_df = chart_df.append(
        {'date': chart.date,
        'rank': song.rank,
        'song': song.title,
        'full_artist': song.artist,
        'main_artist': main_artist,
        'artist2': artist2,
        'artist3': artist3,
        'artist4': artist4,
        'artist5': artist5,
        'genre1': genre1,
        'genre2': genre2,
        'genre3': genre3,
        'genre4': genre4,
        'genre5': genre5,
        'peakPos': song.peakPos,
        'lastPos': song.lastPos,
        'weeks': song.weeks,
        'isNew': song.isNew},
        ignore_index=True)

chart_df.to_csv(output_file, index=False)
