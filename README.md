Radiostr-SpotifyWebApi
======================

An async Spotify Web API wrapper for .NET

[![Build status](https://ci.appveyor.com/api/projects/status/3o35cu9twh55t7t9)](https://ci.appveyor.com/project/DanielLarsenNZ/radiostr-spotifywebapi)

### Work in Progress
I writing the wrappers as I need them. If you need any endpoints wrapped urgently then 
[create an Issue](https://github.com/DanielLarsenNZ/Radiostr-SpotifyWebApi/issues/new), or Fork + PR.

### Async all the things
This wrapper library takes advantage of the async-await features in .NET 4.5. If you need sync methods then you can call 
`.Wait()` on them.

### Download

> PM> Install-Package Radiostr-SpotifyWebApi 

### Example

```csharp
    // TODO: Dependency Resolver
    var http = new RestHttpClient(new System.Net.Http.HttpClient());
    var playlists = new PlaylistsApi(http,
        new ClientCredentialsAuthorizationApi(http, System.Configuration.ConfigurationManager.AppSettings,
            new RuntimeMemoryCache(System.Runtime.Caching.MemoryCache.Default)));

    var response = await playlists.GetPlaylists("DanielLarsenNZ");
    var items = new List<dynamic>(response.items);
    Trace.TraceInformation(items.ToString());
```

See [SpotifyService.cs](https://github.com/DanielLarsenNZ/Radiostr/blob/master/Radiostr/Services/SpotifyService.cs#L26) in 
DanielLarsenNZ/Radiostr for a working example of using this library.

#### Acknowledgements
This wrapper design borrows from [Api.js](https://github.com/possan/webapi-player-example/blob/master/services/api.js) 
in [possan/webapi-player-example](https://github.com/possan/webapi-player-example) - a beautifully simple JavaScript implementation.

Nominate [JamesNK/Newtonsoft.Json](https://github.com/JamesNK/Newtonsoft.Json) for the Nobel Peace Prize.
