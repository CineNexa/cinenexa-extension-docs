
# CineNexa Extension Docs
![Logo](https://www.cinenexa.com/wp-content/uploads/2022/11/Uniwatch.png)

This Api Doc acts as a starting point for Extension Developers to create and publish extensions. 

For more info on Extensions, visit here: Developer[https://www.cinenexa.com/developer/]

**Cinenexa has no way to verify the legality of the extensions, you develop. It is your responsibility to abide by the respective laws.**


## Intro
Your extension is a simple REST api, which should return results in JSON format. 

This is what a sample response look like:

```
{
   "url":"http://commondatastorage.googleapis.com/gtv-videos-bucket/sample/BigBuckBunny.mp4",
   "name":"Big Buck Bunny",
   "quality":720,
   "size":125,
   "subbed":false,
   "streamGroup":"SAMPLE_EXT_UNIQUE_STRING"
}
```

Lets dive into more details on the response object.

Atleast one of the following properties **MUST** be returned
```
url, String - Direct link to the video stream ("http://commondatastorage.googleapis.com/gtv-videos-bucket/sample/BigBuckBunny.mp4)

ytId, String - Youtube ID to be played when user clicks on the specific stream (aqz-KE-bpKQ)

magnet, String - Magnet link of the torrent file, if returned response video is a torrent (magnet:?xt=urn:btih:dd8255ecdc7ca55fb0bbf81323d87062db1f6d1c&dn=Big+Buck+Bunny&tr=udp%3A%2F%2Fexplodie.org%3A6969&tr=udp%3A%2F%2Ftracker.coppersurfer.tk%3A6969&tr=udp%3A%2F%2Ftracker.empire-js.us%3A1337&tr=udp%3A%2F%2Ftracker.leechers-paradise.org%3A6969&tr=udp%3A%2F%2Ftracker.opentrackr.org%3A1337&tr=wss%3A%2F%2Ftracker.btorrent.xyz&tr=wss%3A%2F%2Ftracker.fastcast.nz&tr=wss%3A%2F%2Ftracker.openwebtorrent.com&ws=https%3A%2F%2Fwebtorrent.io%2Ftorrents%2F&xs=https%3A%2F%2Fwebtorrent.io%2Ftorrents%2Fbig-buck-bunny.torrent)

external, String - Url to be opened, when user clicks on the specific stream (https://www.google.com)
```

The other optional properties are the following:
```
name, String - name of stream. For the best experience, it is recommended to keep length of this string less than 20 characters

fileIndex, Integer - (Only if magnet is returned) Index of the video stream within the torrent, if the torrent contains more than 1 file.

quality, String - quality of the stream (accepted values - 144, 360, 480, 720, 1080, 1440, 2000, 4000, 8000, sd, hd, fhd, 4k, 8k)

langCountry - language country of the stream (accepts ISO 3166-1 alpha-2	 codes)

subbed - bool, if the stream contains embedded subtitles

dubbed - bool, if the stream contains dubbed version of media

torrent - bool, if the provided stream is a torrent

size - double, size of the stream in mb

seeds - double, no. of seeds (for torrent streams)

streamGroup - string, this is used for autoplay of next episodes. For eg., for a given ep of a show, this can be labelled as "EXAMPLE_ADDON_360p" and if any stream of the next episode contains the same streamGroup, that stream will be chosen for autoplay implicitly

```
