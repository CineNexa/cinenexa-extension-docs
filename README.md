
# CineNexa Extension Docs
![Logo](https://www.cinenexa.com/wp-content/uploads/2022/11/Uniwatch.png)

This Api Doc acts as a starting point for Extension Developers to create and publish extensions. 

For more info on Extensions, visit here: [Developer](https://www.cinenexa.com/developer/)

**Cinenexa has no way to verify the legality of the extensions, you develop. It is your responsibility to abide by the respective laws. See more at [Terms & Conditions](https://www.cinenexa.com/terms-conditions/)**

# Table of Contents
1. [Intro](#intro)
2. [Response](#response)
3. [Received Data](#received-data)
4. [Example](#example)
5. [Deploy](#deploy)

## Intro
An extension is a REST Api, providing movie/show streams to CineNexa app.

Whenever a user clicks on a movie/show, CineNexa sends a data object (containing data related to the movie/show clicked) to all the extensions (installed by the user). The extensions are given a time frame of **30 seconds** to respond to the request after which the request is timed out. The extensions respond data in Json-encoded format.

## Response
Your extension is a simple REST api, which should return results in JSON format. 

This is what a sample response look like:

```
{
    "cacheTimeout" : 43200,
    "streams" : [
        {
           "url":"http://commondatastorage.googleapis.com/gtv-videos-bucket/sample/BigBuckBunny.mp4",
           "name":"Big Buck Bunny",
           "quality":720,
           "size":125,
           "subbed":false,
           "streamGroup":"SAMPLE_EXT_UNIQUE_STRING"
        }
    ]
}
```

Lets dive into more details on the response object.

>**cacheTimeout**, Integer - (Not yet implemented) In order to reduce your server costs as well as user bandwidth, CineNexa does not send request to your server everytime a user opens a specific movie/show details (say X) and stores the responses in cache. This property represents the time period (in seconds) for which CineNexa will keep the returned streams in cache and after which, if the user opens the details of X, a new request will be sent.

>**streams**, list - Stream object containing stream details

### Stream Object

Atleast one of the following properties **MUST** be returned

>**url**, String - Direct link to the video stream

>**ytId**, String - Youtube ID to be played when user clicks on the specific stream 

>**magnet**, String - Magnet link of the torrent file, if returned response video is a torrent 

>**external**, String - Url to be opened, when user clicks on the specific stream

The other optional properties are the following:
>**name**, String - name of stream. For the best experience, it is recommended to keep length of this string less than 20 characters

>**fileIndex**, Integer - (Only if magnet is returned) Index of the video stream within the torrent, if the torrent contains more than 1 file.

>**quality**, String - quality of the stream (accepted values - 144, 360, 480, 720, 1080, 1440, 2000, 4000, 8000)

>**country** - country to which the stream is restricted to (accepts ISO 3166-1 alpha-2 codes). This can be used to represent the audio language of stream

>**subbed**, bool -  if the stream contains embedded subtitles

>**dubbed**, bool - if the stream contains dubbed version of media

>**torrent**, bool - if the provided stream is a torrent

>**size**,double - size of the stream in mb

>**seeds**, double - no. of seeds (for torrent streams)

>**streamGroup**, String - this is used for autoplay of next episodes. For eg., for a given ep of a show, this can be labelled as "EXAMPLE_ADDON_360p" and if any stream of the next episode contains the same streamGroup, that stream will be chosen for autoplay implicitly


## Received Data
Whenever CineNexa makes request to your extension, the following properties will be url encoded to be accessed by your extension:

>**type**, String - Type of the item requested (movie, show)

>**season**, Integer - Season number of the show (if type is show)

>**episode**, Integer - Episode number of the show (if type is show)

>**imdbId**, String - Imdb id of the requested item

>**tmdbId**, String - Tmdb id of the requested item

>**traktId**, String - Trakt id of the requested item

>**name**, String - Name of the requested item

## Example
This sample extension responds 2 streams of Big Buck Bunny, one with direct https url while the second as a magnet link.

```
import express from "express";

var app = express();

const port = 8000;

app.listen(port, function (err) {
  if (err) {
    console.log("Error while starting server");
  } else {
    console.log("Server has been started at " + port);
  }
});

app.get("/", (req, res, next) => {
  let name = req.query.name;
  let imdbId = req.query.imdbId;

  let respondData;

  if (imdbId === "tt10872600" || name === "Big Bunny") {
    respondData = {
      streams: [
        {
          url:
            "http://commondatastorage.googleapis.com/gtv-videos-bucket/sample/BigBuckBunny.mp4",
          name: "Big Bunny",
          quality: "1080"
        },
        {
          magnet:
            "magnet:?xt=urn:btih:c9e15763f722f23e98a29decdfae341b98d53056&dn=Cosmos+Laundromat&tr=udp%3A%2F%2Fexplodie.org%3A6969&tr=udp%3A%2F%2Ftracker.coppersurfer.tk%3A6969&tr=udp%3A%2F%2Ftracker.empire-js.us%3A1337&tr=udp%3A%2F%2Ftracker.leechers-paradise.org%3A6969&tr=udp%3A%2F%2Ftracker.opentrackr.org%3A1337&tr=wss%3A%2F%2Ftracker.btorrent.xyz&tr=wss%3A%2F%2Ftracker.fastcast.nz&tr=wss%3A%2F%2Ftracker.openwebtorrent.com&ws=https%3A%2F%2Fwebtorrent.io%2Ftorrents%2F&xs=https%3A%2F%2Fwebtorrent.io%2Ftorrents%2Fcosmos-laundromat.torrent",
          name: "Big Bunny",
          torrent: true,
          seeds: 1345
        }
      ]
    };
  }

  res.json(respondData);
});
```

## Deploy
We recommend to deploy your extension to any of the following services, as all of them provides free tiers:
- [Render](https://render.com/)
- [Fly](https://fly.io/)
- [Railway](https://railway.app/)

Serverless functions can also be used for deploying extensions, but it is to be kept in mind that these cold start on every requests (or after a given time period) which can increase your response time
- [Firebase Functions](https://firebase.google.com/docs/functions/get-started)
- [AWS Lambda](https://aws.amazon.com/lambda/)
- [Supabase Functions](https://supabase.com/docs/guides/functions)


After deploying your extension, publish your extension in the app for it to be visible to users. You can publish your extension by providing details here - [Publish Extension](https://www.cinenexa.com/publish-extension/)
