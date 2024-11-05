# **Sharing moments** 

###### ----Sunrise & Sunset & Spotify API

I am always enjoying sunrises and appreciating sunsets. I often listen to old songs in the morning and rock music during the twilight hours. However, the busy life often makes me miss these beautiful moments. There have been countless times when I’ve lost track of time due to work, only realizing that I missed a stunning sunset after seeing friends share pictures of the vibrant sky. 



The 9503km distance often makes it difficult to stay connected closely with my family. The most common messages I get from them are “good morning” and “good night”, but even then, it's hard for me to reply in time. I often miss their sunrise and night, and I always miss them in the morning and sunset when I am in a foreign country. 



With this idea, I want to create an interface that captures these special moments across time zones. By utilizing APIs to track and save the current or daily sunrise and sunset times, along with geographical location data, the app will generate unique playlists tailored to those moments. The music styles for sunrise and sunset will be customized based on time preferences and will evolve with similar genres over time. Each day, every different time zone, a new playlist will be created, and a reminder will pop up to enjoy it as the sun rises or sets. These playlists, categorized by location, date, and time, will be stored in a calendar view for easy reference. 



## Contents:

1.Library

2.API Setup

3.Key Features

4.Work Flow

5.Interface Display

6.Conclusion





### Library:

gmplot==1.4.1
googlemaps==4.10.0

matplotlib

Pandas

Jupyter notebook







### Used API:

- [Google Map API](https://developers.google.com/maps/documentation/javascript/examples/control-positioning)

  通过谷歌地图API来获取：

  1.用户实时的IP地理位置信息，从而得到目前位置的经纬度信息

  2.用户故乡的地理位置信息，从而得到用户故乡的经纬度信息

- [Sunset & Sunrise API](https://sunrise-sunset.org/api#google_vignette)

  通过经纬度信息分别调取：

  1.实时地理位置的日出日落时间情况

  2.故乡地理位置的日出日落时间情况

- [Spotify API](https://developer.spotify.com/documentation/web-api)

  通过spotify api来分别获取：

  1.地理位置中最热门的歌曲榜单

  2.榜单中的音频信息

  3.生成定制的歌曲列表





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
 2. geographic location information of the user's hometown; getting the latitude and longitude information of the user's hometown.

**2.Get from Sunset & Sunrise API：**

​	1.Sunrise and sunset times for real-time geographic locations.

​	2.Sunrise and sunset time of the hometown geographic location.

​	3.the time difference between the sunrise and sunset of the implemented geolocation and the hometown geolocation respectively

​	4.Further processing of the acquired time difference (-8hrs), in preparation for the next step of being called by the Spotify API data.



**3.Get from Spotify API:**

​	1.Have the top 50 hottest single songs in real-time location (GB) and hometown location (CN) separately

​	2.Analyse the audio features of the songs, extract the ‘id’, and generate a list based on ‘feature’.

​	3.Take the time difference to one decimal place, and process the absolute value.

​	4.In the generated song list, get the ‘validity’ feature, which means the positive or negative degree of the song, such as A measure from 0.0 to 1.0 describing the musical positiveness conveyed by a track. Tracks with high valence sound more positive (e.g. happy, cheerful, euphoric), while tracks with low valence sound more negative (e.g. sad, depressed, angry). g. sad, depressed, angry).

​	5.Find valence values equal to the processed time difference, and find equivalent ‘song mood values’.

​	6.Output a list of songs with sunrise and sunset that match ‘GB’ and ‘CN’ respectively.



**4.Output"Sunrise time" and "Sunset time" playlist**

