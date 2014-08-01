Radiostr-SpotifyWebApi
======================

An async Spotify Web API wrapper for .NET

[![Build status](https://ci.appveyor.com/api/projects/status/3o35cu9twh55t7t9)](https://ci.appveyor.com/project/DanielLarsenNZ/radiostr-spotifywebapi)

### Work in Progress
If you need any endpoints wrapped urgently then 
[create an Issue](https://github.com/DanielLarsenNZ/Radiostr-SpotifyWebApi/issues/new), or Fork + PR.

### Async all the things
This wrapper library takes advantage of the async-await features in .NET 4.5. If you need sync methods you can call 
`.Wait()` on them.

### Download

> PM> Install-Package Radiostr-SpotifyWebApi 

### Classes

#### `PlaylistsApi`

Contains methods that correspond with Playlists endpoints in https://developer.spotify.com/web-api/endpoint-reference/. 
Depends on an `IHttpClient` and an `IAuthorizationApi`.

#### `RestHttpClient`

Implements `IHttpClient`. A simple Wrapper of `System.Net.Http.HttpClient` suitable for use with the Spotify Web API.

#### `ClientCredentialsAuthorizationApi`

Implements `IAuthorizationApi`. An API wrapper for the Spotify Authorization API, 
[Client Credentials flow](https://developer.spotify.com/web-api/authorization-guide/#client-credentials-flow). Depends an 
`IHttpClient` and (optionally) an `ICache`. 

`ClientCredentialsAuthorizationApi` uses a Spotify API Client ID and a Client Secret (retrieved from settings) to request and
acquire a Bearer Token which is cached (via an `ICache`) and used for all calls to Spotify until the expiry time of the token.
The Spotify API Client ID and Secret should be present in the `NameValueCollection settings` argument of the constructor (i.e. 
as in `System.Configuration.ConfigurationManager.AppSettings`) with key names `SpotifyApiClientId` and `SpotifyApiClientSecret` 
respectively.

Go to https://developer.spotify.com/ to register your application and obtain a Spotify API Client ID.

#### `RuntimeMemoryCache`

Implements `ICache`. A simple wrapper of `System.Runtime.Caching.ObjectCache`.

#### Example

```csharp
    // TODO: Dependency Resolver
    var http = new RestHttpClient(new System.Net.Http.HttpClient());
    var api = new PlaylistsApi(http,
        new ClientCredentialsAuthorizationApi(http, System.Configuration.ConfigurationManager.AppSettings,
            new RuntimeMemoryCache(System.Runtime.Caching.MemoryCache.Default)));

    var playlists = await api.GetPlaylists("DanielLarsenNZ");
    Trace.TraceInformation(playlists.ToString());
```

See [SpotifyService.cs](https://github.com/DanielLarsenNZ/Radiostr/blob/master/Radiostr/Services/SpotifyService.cs#L26) in 
[DanielLarsenNZ/Radiostr](https://github.com/DanielLarsenNZ/Radiostr) for a working example of using this library.

### Acknowledgements

This wrapper design borrows from [Api.js](https://github.com/possan/webapi-player-example/blob/master/services/api.js) 
in [possan/webapi-player-example](https://github.com/possan/webapi-player-example) - a beautifully simple JavaScript implementation.

Nominate [JamesNK/Newtonsoft.Json](https://github.com/JamesNK/Newtonsoft.Json) for the Nobel Peace Prize.
