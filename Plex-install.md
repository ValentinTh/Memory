apt-get install gnupg curl

echo deb https://downloads.plex.tv/repo/deb public main | sudo tee /etc/apt/sources.list.d/plexmediaserver.list
curl https://downloads.plex.tv/plex-keys/PlexSign.key | sudo apt-key add -

wget https://downloads.plex.tv/plex-media-server/1.14.1.5488-cc260c476/plexmediaserver_1.14.1.5488-cc260c476_amd64.deb

sudo dpkg -i plexmediaserver_1.14.1.5488-cc260c476_amd64.deb


http://127.0.0.1:32400/web


https://support.plex.tv/articles/200288586-installation/

https://support.plex.tv/articles/200931138-troubleshooting-remote-access/

https://support.plex.tv/articles/235974187-enable-repository-updating-for-supported-linux-server-distributions/

https://forums.plex.tv/t/how-can-i-fix-the-following-the-repository-https-downloads-plex-tv-repo-deb-public-release/235993/5

https://forums.plex.tv/t/conflicting-distribution-error-when-updating-plex-repo-via-apt-in-ubuntu-18-04/238152/15


## Forward the Port in the Router
In order to forward a port for Plex Media Server, you’ll need three main pieces of information:
WAN/External Port: Port 32400 (TCP) is default, but you can generally use any available port in the 20,000 to 50,000 range.
LAN/Internal Port: This will always be 32400.
IP Address: The local IP Address of the computer running the Plex Media Server. This is what you did above.
https://www.linode.com/docs/applications/media-servers/install-plex-media-server-on-ubuntu-16-04/#install-plex
