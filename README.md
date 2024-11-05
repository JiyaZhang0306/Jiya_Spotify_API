# **Sharing moments** 

###### ----Sunrise & Sunset & Spotify API

I am always enjoying sunrises and appreciating sunsets. I often listen to old songs in the morning and rock music during the twilight hours. However, the busy life often makes me miss these beautiful moments. There have been countless times when I’ve lost track of time due to work, only realizing that I missed a stunning sunset after seeing friends share pictures of the vibrant sky. 



The 9503km distance often makes it difficult to stay connected closely with my family. The most common messages I get from them are “good morning” and “good night”, but even then, it's hard for me to reply in time. I often miss their sunrise and night, and I always miss them in the morning and sunset when I am in a foreign country. 



With this idea, I want to create an interface that captures these **sunset&sunrise** moments across time zones. By utilizing APIs to track and save the current or daily sunrise and sunset times, along with geographical location data, the app will generate unique playlists tailored to those moments. The music styles for sunrise and sunset will be customized based on time preferences and will evolve with similar genres over time. Each day, every different time zone, a new playlist will be created, and a reminder will pop up to enjoy it as the sun rises or sets. These playlists, categorized by location, date, and time, will be stored in a calendar view for easy reference. 



## Contents:

​	1.Library

​	2.API Setup

​	3.Key Features

​	4.Work Flow

​	5.Interface Display

​	6.Conclusion





------

### Library:

- gmplot==1.4.1
- googlemaps==4.10.0
- spotipy==2.24.0
- matplotlib-inline==0.1.7
- urllib3==2.2.3
- pandas==2.2.3
- jupyterlab==4.2.5







### Used API:

- [Google Map API](https://developers.google.com/maps/documentation/javascript/examples/control-positioning)

  **Use the Google Maps API to get:**

  1. The user's real-time IP-based geolocation to acquire the current latitude and longitude information.
  2. The user's hometown geolocation to acquire the latitude and longitude information of their hometown.

- [Sunset & Sunrise API](https://sunrise-sunset.org/api#google_vignette)

  **Using the latitude and longitude information to get:**

  1. The sunrise and sunset times for the real-time geolocation.
  2. The sunrise and sunset times for the hometown geolocation.

- [Spotify API](https://developer.spotify.com/documentation/web-api)

  **Use the Spotify API to get:**

  1. The top charts of popular songs for the specific geolocation.
  2. The audio information of songs in the charts.
  3. A custom-generated playlist.







### Key Features:

​	1.**Unique Sunrise & Sunset Playlists** 

The app will generate unique playlists for both sunrise and sunset based on analysing users’ music 	preferences in different time. The Spotify API will be used to create playlists with song recommendations that match the mood of these times of day. 

​	2.**Dynamic Playlist Generation Based on Location & Time** 

Using the Sunrise & Sunset API, the app will retrieve the specific sunrise and sunset times for a user’s current geographic location or any saved location. Playlists will automatically be generated at those times, ensuring they align with the local environment. 

​	3.**Push Notifications for Sunrise and Sunset Moments** 

Users will receive notifications reminding them when their customized sunrise and sunset playlists are available. This feature will help users engage with their surroundings at meaningful moments. 

​	4.**Calendar Integration for Daily Playlist Archives** 

Each day’s sunrise and sunset playlists will be stored in a calendar view, allowing users to revisit and replay past playlists. This feature is particularly useful for users who want to enjoy specific past moments or music. 

​	5.**Cross-Time Zone Sharing** 

The app will allow users to save multiple locations, enabling them to track sunrise and sunset in different time zones. This feature could be used to stay connected with family and friends by listening to the same playlist during their sunrise or sunset, even from afar. 









### Work Flow

**1**.**Get from Google Maps API:**

 1. user's real-time IP geolocation information; getting the latitude and longitude information of the current location.

    ```python
    location_json = json.loads(response_map.read())
    lattitude, longtitude = location_json['loc'].split(',')
    print(lattitude, longtitude)
    ```

    

 2. geographic location information of the user's hometown; getting the latitude and longitude information of the user's hometown.

    ```python
    hometown_lattitude = response_hometown_map[0]['geometry']['location']['lat']
    hometown_longtitude = response_hometown_map[0]['geometry']['location']['lng']
    print(hometown_lattitude)
    print(hometown_longtitude)
    ```

    

**2.Get from Sunset & Sunrise API：**

​	1.Sunrise and sunset times for real-time geographic locations.

​	2.Sunrise and sunset time of the hometown geographic location.

```python
hometown_suntime_json = json.loads(response_hometown.read())
print(hometown_suntime_json)
```

```python
sunrise_hometown = hometown_suntime_json['results']['sunrise']
sunset_hometown = hometown_suntime_json['results']['sunset']

# the printed time is in UK time(8hrs difference)
print(sunrise_hometown)
print(sunset_hometown)
```

​	3.the time difference between the sunrise and sunset of the implemented geolocation and the hometown geolocation respectively

```python
# suntime difference

#sunrise
format_str = "%I:%M:%S %p"
sunrise_hometown_dt = datetime.strptime(sunrise_hometown, format_str)
sunrise_dt = datetime.strptime(sunrise, format_str)

if sunrise_dt < sunrise_hometown_dt:
    sunrise_dt += timedelta(days=1)

sunrise_time_difference = sunrise_dt - sunrise_hometown_dt
sunrise_difference = sunrise_time_difference.total_seconds() / 3600  # Convert seconds to hours
print(sunrise_difference)

#sunset
format_str = "%I:%M:%S %p"
sunset_hometown_dt = datetime.strptime(sunset_hometown, format_str)
sunset_dt = datetime.strptime(sunset, format_str)

if sunset_dt < sunset_hometown_dt:
    sunset_dt += timedelta(days=1)

sunset_time_difference = sunset_dt - sunset_hometown_dt
sunset_difference = sunset_time_difference.total_seconds() / 3600  # Convert seconds to hours
print(sunset_difference)
```

​	4.Further processing of the acquired time difference (-8hrs), in preparation for the next step of being called by the Spotify API data.

```python
sunrise_mood = sunrise_difference - 8
print(sunrise_mood)

sunset_mood = sunset_difference - 8
print(sunset_mood)
```

**3.Get from Spotify API:**

​	1.Have the top 50 hottest single songs in real-time location (GB) and hometown location (CN) separately

```python
playlist_id = '37i9dQZF1DWY4lFlS4Pnso'
track_results = sp.playlist(playlist_id)
print(track_results)
```

​	2.Analyse the audio features of the songs, extract the ‘id’, and generate a list based on ‘feature’.

```python
song_data = track_results['tracks']['items']
print(song_data)
```

```python
song_feature = sp.audio_features(list(songs_id))
print(song_feature)
```

​	3.Take the time difference to one decimal place, and process the absolute value.

​	4.In the generated song list, get the ‘validity’ feature, which means the positive or negative degree of the song, such as a measure from 0.0 to 1.0 describing the musical positiveness conveyed by a track. Tracks with high valence sound more positive (e.g. happy, cheerful, euphoric), while tracks with low valence sound more negative (e.g. sad, depressed, angry). g. sad, depressed, angry).

​	5.Find valence values equal to the processed time difference, and find equivalent ‘song mood values’.

```
sunrise_matching_songs = [song for song in song_feature if round(song.get('valence'),1) == round(abs(sunrise_mood),1)]
sunset_matching_songs = [song for song in song_feature if round(song.get('valence'),1) == round(abs(sunset_mood),1)]
print(sunrise_matching_songs)
print(sunset_matching_songs)
```

​	6.Output a list of songs with sunrise and sunset that match ‘GB’ and ‘CN’ respectively.

```python
sunset_song_uris = []
for song in sunset_matching_songs:
    sunset_song_uris.append(song['uri'])
    
for song in hometown_sunset_matching_songs:
    sunset_song_uris.append(song['uri'])
    
print(sunset_song_uris)
```



**4.Output"Sunrise time" and "Sunset time" playlist**

