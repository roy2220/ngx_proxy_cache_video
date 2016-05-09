# ngx_proxy_cache_video
A patch to make the pseudo-streaming and the proxy cache work together.
## Additional Directives
```
Syntax: 	proxy_cache_mp4;
Default: 	—
Context: 	location
```
Turns on MP4 pseudo-streaming for cache in a surrounding location.
```
Syntax: 	proxy_cache_flv;
Default: 	—
Context: 	location
```
Turns on FLV pseudo-streaming for cache in a surrounding location.
```
Syntax: 	proxy_clear_argument argument;
Default: 	—
Context: 	http, server, location
```
Clears certain arguments in the request line passed to the proxied server.
## Example Configuration
```
http {
	proxy_cache_path cache levels=1:2 keys_zone=default:200m inactive=1d max_size=1g;

	server {
		listen 8080;

		location ~ \.flv$ {
			proxy_cache_flv;               # Additional Directive
			proxy_clear_argument start;    # Additional Directive
			proxy_pass http://localhost:8081;
			proxy_cache default;
			proxy_cache_valid 12h;
		}

		location ~ \.mp4$ {
			proxy_cache_mp4;               # Additional Directive
			proxy_clear_argument start;    # Additional Directive
			proxy_pass http://localhost:8081;
			proxy_cache default;
			proxy_cache_valid 12h;
		}
	}
}
```
