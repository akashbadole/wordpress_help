```sh

#adding custome headers : usefull for shared hostings
Header add X-Frame-Options "DENY"
Header add X-XSS-Protection "1; mode=block"
# use this only if all scripts are in same domain.
#Header add X-Content-Security-Policy "default-src 'self'"
Header add X-Content-Type-Options "nosniff"
Header unset link
Header unset Server
Header unset X-Pingback
ServerSignature Off
Options -Indexes

DirectoryIndex index.php index.html /index.php

#block wp-config.php
<files wp-config.php>
order allow,deny
deny from all
</files>


#cache control
# BEGIN EXPIRES
<IfModule mod_expires.c>
    ExpiresActive On
    ExpiresDefault "access plus 10 days"
    ExpiresByType text/css "access plus 1 week"
    ExpiresByType text/plain "access plus 1 month"
    ExpiresByType image/gif "access plus 1 month"
    ExpiresByType image/png "access plus 1 month"
    ExpiresByType image/jpeg "access plus 1 month"
    ExpiresByType application/x-javascript "access plus 1 month"
    ExpiresByType application/javascript "access plus 1 week"
    ExpiresByType application/x-icon "access plus 1 year"
</IfModule>
# END EXPIRES

#compression
SetOutputFilter DEFLATE
<IfModule mod_setenvif.c>
  SetEnvIfNoCase Request_URI \.(?:rar|zip)$ no-gzip dont-vary
  SetEnvIfNoCase Request_URI \.(?:gif|jpg|png)$ no-gzip dont-vary
  SetEnvIfNoCase Request_URI \.(?:avi|mov|mp4)$ no-gzip dont-vary
  SetEnvIfNoCase Request_URI \.mp3$ no-gzip dont-vary
</IfModule>


# REQUEST METHODS FILTERED
RewriteEngine On
RewriteCond %{REQUEST_METHOD} ^(HEAD|TRACE|DELETE|TRACK|DEBUG) [NC]
RewriteRule ^(.*)$ index.php [L]

#custom HTACCESS entries

<Files .htaccess,.svn,error_log>
order allow,deny
deny from all
</Files>

Options +FollowSymlinks

# bad bot protection
<IfModule mod_rewrite.c>
RewriteCond %{HTTP_USER_AGENT} ^BlackWidow [OR] 
RewriteCond %{HTTP_USER_AGENT} ^Bot\ mailto:craftbot@yahoo.com [OR] 
RewriteCond %{HTTP_USER_AGENT} ^ChinaClaw [OR] 
RewriteCond %{HTTP_USER_AGENT} ^Custo [OR] 
RewriteCond %{HTTP_USER_AGENT} ^DISCo [OR] 
RewriteCond %{HTTP_USER_AGENT} ^Download\ Demon [OR] 
RewriteCond %{HTTP_USER_AGENT} ^eCatch [OR] 
RewriteCond %{HTTP_USER_AGENT} ^EirGrabber [OR] 
RewriteCond %{HTTP_USER_AGENT} ^EmailSiphon [OR] 
RewriteCond %{HTTP_USER_AGENT} ^EmailWolf [OR] 
RewriteCond %{HTTP_USER_AGENT} ^Express\ WebPictures [OR] 
RewriteCond %{HTTP_USER_AGENT} ^ExtractorPro [OR] 
RewriteCond %{HTTP_USER_AGENT} ^EyeNetIE [OR] 
RewriteCond %{HTTP_USER_AGENT} ^FlashGet [OR] 
RewriteCond %{HTTP_USER_AGENT} ^GetRight [OR] 
RewriteCond %{HTTP_USER_AGENT} ^GetWeb! [OR] 
RewriteCond %{HTTP_USER_AGENT} ^Go!Zilla [OR] 
RewriteCond %{HTTP_USER_AGENT} ^Go-Ahead-Got-It [OR] 
RewriteCond %{HTTP_USER_AGENT} ^GrabNet [OR] 
RewriteCond %{HTTP_USER_AGENT} ^Grafula [OR] 
RewriteCond %{HTTP_USER_AGENT} ^HMView [OR] 
RewriteCond %{HTTP_USER_AGENT} HTTrack [NC,OR] 
RewriteCond %{HTTP_USER_AGENT} ^Image\ Stripper [OR] 
RewriteCond %{HTTP_USER_AGENT} ^Image\ Sucker [OR] 
RewriteCond %{HTTP_USER_AGENT} Indy\ Library [NC,OR] 
RewriteCond %{HTTP_USER_AGENT} ^InterGET [OR] 
RewriteCond %{HTTP_USER_AGENT} ^Internet\ Ninja [OR] 
RewriteCond %{HTTP_USER_AGENT} ^JetCar [OR] 
RewriteCond %{HTTP_USER_AGENT} ^JOC\ Web\ Spider [OR] 
RewriteCond %{HTTP_USER_AGENT} ^larbin [OR] 
RewriteCond %{HTTP_USER_AGENT} ^LeechFTP [OR] 
RewriteCond %{HTTP_USER_AGENT} ^Mass\ Downloader [OR] 
RewriteCond %{HTTP_USER_AGENT} ^MIDown\ tool [OR] 
RewriteCond %{HTTP_USER_AGENT} ^Mister\ PiX [OR] 
RewriteCond %{HTTP_USER_AGENT} ^Navroad [OR] 
RewriteCond %{HTTP_USER_AGENT} ^NearSite [OR] 
RewriteCond %{HTTP_USER_AGENT} ^NetAnts [OR] 
RewriteCond %{HTTP_USER_AGENT} ^NetSpider [OR] 
RewriteCond %{HTTP_USER_AGENT} ^Net\ Vampire [OR] 
RewriteCond %{HTTP_USER_AGENT} ^NetZIP [OR] 
RewriteCond %{HTTP_USER_AGENT} ^Octopus [OR] 
RewriteCond %{HTTP_USER_AGENT} ^Offline\ Explorer [OR] 
RewriteCond %{HTTP_USER_AGENT} ^Offline\ Navigator [OR] 
RewriteCond %{HTTP_USER_AGENT} ^PageGrabber [OR] 
RewriteCond %{HTTP_USER_AGENT} ^Papa\ Foto [OR] 
RewriteCond %{HTTP_USER_AGENT} ^pavuk [OR] 
RewriteCond %{HTTP_USER_AGENT} ^pcBrowser [OR] 
RewriteCond %{HTTP_USER_AGENT} ^RealDownload [OR] 
RewriteCond %{HTTP_USER_AGENT} ^ReGet [OR] 
RewriteCond %{HTTP_USER_AGENT} ^SiteSnagger [OR] 
RewriteCond %{HTTP_USER_AGENT} ^SmartDownload [OR] 
RewriteCond %{HTTP_USER_AGENT} ^SuperBot [OR] 
RewriteCond %{HTTP_USER_AGENT} ^SuperHTTP [OR] 
RewriteCond %{HTTP_USER_AGENT} ^Surfbot [OR] 
RewriteCond %{HTTP_USER_AGENT} ^tAkeOut [OR] 
RewriteCond %{HTTP_USER_AGENT} ^WWW-Mechanize [OR] 
RewriteCond %{HTTP_USER_AGENT} ^Teleport\ Pro [OR] 
RewriteCond %{HTTP_USER_AGENT} ^VoidEYE [OR] 
RewriteCond %{HTTP_USER_AGENT} ^Web\ Image\ Collector [OR] 
RewriteCond %{HTTP_USER_AGENT} ^Web\ Sucker [OR] 
RewriteCond %{HTTP_USER_AGENT} ^WebAuto [OR] 
RewriteCond %{HTTP_USER_AGENT} ^WebCopier [OR] 
RewriteCond %{HTTP_USER_AGENT} ^WebFetch [OR] 
RewriteCond %{HTTP_USER_AGENT} ^WebGo\ IS [OR] 
RewriteCond %{HTTP_USER_AGENT} ^WebLeacher [OR] 
RewriteCond %{HTTP_USER_AGENT} ^WebReaper [OR] 
RewriteCond %{HTTP_USER_AGENT} ^WebSauger [OR] 
RewriteCond %{HTTP_USER_AGENT} ^Website\ eXtractor [OR] 
RewriteCond %{HTTP_USER_AGENT} ^Website\ Quester [OR] 
RewriteCond %{HTTP_USER_AGENT} ^WebStripper [OR] 
RewriteCond %{HTTP_USER_AGENT} ^WebWhacker [OR] 
RewriteCond %{HTTP_USER_AGENT} ^WebZIP [OR] 
RewriteCond %{HTTP_USER_AGENT} ^Widow [OR] 
RewriteCond %{HTTP_USER_AGENT} ^WWWOFFLE [OR] 
RewriteCond %{HTTP_USER_AGENT} ^Xaldon\ WebSpider [OR] 
RewriteCond %{HTTP_USER_AGENT} ^Toata\ dragostea\ mea\ pentru\ diavola [OR]
RewriteCond %{HTTP_USER_AGENT} ^Mozilla/5.0\ SF [OR]
RewriteCond %{HTTP_USER_AGENT} ^Zeus 
RewriteRule ^.* - [F,L]
</IfModule>

#scanner bots as well as malacious input blocker
<IfModule mod_rewrite.c>
RewriteCond %{HTTP_USER_AGENT} ^w3af.sourceforge.net [NC,OR]
RewriteCond %{HTTP_USER_AGENT} dirbuster [NC,OR]
RewriteCond %{HTTP_USER_AGENT} nikto [NC,OR]
RewriteCond %{HTTP_USER_AGENT} sqlmap [NC,OR]
RewriteCond %{HTTP_USER_AGENT} fimap [NC,OR]
RewriteCond %{HTTP_USER_AGENT} nessus [NC,OR]
RewriteCond %{HTTP_USER_AGENT} whatweb [NC,OR]
RewriteCond %{HTTP_USER_AGENT} Openvas [NC,OR]
RewriteCond %{HTTP_USER_AGENT} jbrofuzz [NC,OR]
RewriteCond %{HTTP_USER_AGENT} libwhisker [NC,OR]
RewriteCond %{HTTP_USER_AGENT} webshag [NC,OR]
RewriteCond %{HTTP_USER_AGENT} (havij|Netsparker|libwww-perl|python|nikto|curl|scan|java|winhttp|clshttp|loader) [NC,OR]
RewriteCond %{HTTP_USER_AGENT} (%0A|%0D|%27|%3C|%3E|%00) [NC,OR]
RewriteCond %{HTTP_USER_AGENT} (;|<|>|'|"|\)|\(|%0A|%0D|%22|%27|%28|%3C|%3E|%00).*(libwww-perl|python|nikto|curl|scan|java|winhttp|HTTrack|clshttp|archiver|loader|email|harvest|extract|grab|miner) [NC,OR]
RewriteCond %{HTTP:Acunetix-Product} ^WVS
RewriteCond %{REQUEST_URI} (<|%3C)([^s]*s)+cript.*(>|%3E) [NC,OR]
RewriteCond %{REQUEST_URI} (<|%3C)([^e]*e)+mbed.*(>|%3E) [NC,OR]
RewriteCond %{REQUEST_URI} (<|%3C)([^o]*o)+bject.*(>|%3E) [NC,OR]
RewriteCond %{REQUEST_URI} (<|%3C)([^i]*i)+frame.*(>|%3E) [NC,OR]
RewriteCond %{REQUEST_URI} base64_(en|de)code[^(]*\([^)]*\) [NC,OR]
RewriteCond %{REQUEST_URI} (%0A|%0D|\\r|\\n) [NC,OR]
RewriteCond %{REQUEST_URI} union([^a]*a)+ll([^s]*s)+elect [NC]
RewriteRule ^(.*)$ http://127.0.0.1 [R=301,L]
</IfModule>

#blocking access to readme.txt and html
<files readme.txt>
order allow,deny
deny from all
</files>
<files readme.html>
order allow,deny
deny from all
</files>

#blocking access to swfupload.swf
<files swfupload.swf>
order allow,deny
deny from all
</files>

#blocking timthumb.php
<files timthumb.php>
order allow,deny
deny from all
</files>



# fun with numbers :P
ErrorDocument 400 /index.php
ErrorDocument 404 /index.php
ErrorDocument 403 http://127.0.0.1
ErrorDocument 500 http://127.0.0.1
# for serious usecase try this
#ErrorDocument 403 /
#ErrorDocument 400 /
#ErrorDocument 404 /

#username / attchment enumeration protection
<IfModule mod_rewrite.c>
RewriteCond %{QUERY_STRING} author=([0-9]*) [OR]
RewriteCond %{QUERY_STRING} attachment_id=([0-9]*)
RewriteRule ^(.*)$ /index.php [F,L]
</IfModule>

# after this let the wordpress code be present.

```
