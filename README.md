# prerender-angularjs
Prerender settings for angularjs projects.
Sample nginx configuration:

```
server {
	listen 80;
	server_name example.com;
	root /var/www/example.com;
	index index.html;
	location / {
		try_files $uri @prerender;
	}
	location @prerender {
		set $prerender 0;
		if ($http_user_agent ~* "googlebot|baiduspider|twitterbot|facebookexternalhit|rogerbot|linkedinbot|embedly|quora link preview|showyoubot|outbrain|pinterest|slackbot|vkShare|W3C_Validator") {
			set $prerender 1;
        	}
		if ($args ~ "_escaped_fragment_") {
			set $prerender 1;
        	}
		if ($http_user_agent ~ "Prerender") {
			set $prerender 0;
        	}
		if ($uri ~ "\.(js|css|xml|less|png|jpg|jpeg|gif|pdf|doc|txt|ico|rss|zip|mp3|rar|exe|wmv|doc|avi|ppt|mpg|mpeg|tif|wav|mov|psd|ai|xls|mp4|m4a|swf|dat|dmg|iso|flv|m4v|torrent|ttf|woff)") {
			set $prerender 0;
        	}
	
		if ($prerender = 1) {
			set $prerender "http://127.0.0.1:9999";
			rewrite .* /$scheme://$host$request_uri? break;
			proxy_pass $prerender;
		}
		if ($prerender = 0) {
			rewrite .* /index.html break;
		}
	}
}
```
