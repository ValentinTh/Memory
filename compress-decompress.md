=> Compress the archive

You need to use the tar command as follows (syntax of tar command):

tar -zcvf archive-name.tar.gz directory-name

Where,

-z : Compress archive using gzip program

-c: Create archive

-v: Verbose i.e display progress while creating archive

-f: Archive File name

For example, say you have a directory called /home/jerry/prog and you would like to compress this directory then you can type tar command as follows:

$ tar -zcvf prog-1-jan-2005.tar.gz /home/jerry/prog

Above command will create an archive file called prog-1-jan-2005.tar.gz in current directory. If you wish to restore your archive then you need to use the following command (it will extract all files in current directory):

$ tar -zxvf prog-1-jan-2005.tar.gz

Where,

-x: Extract files

If you wish to extract files in particular directory, for example in /tmp then you need to use the following command:

$ tar -zxvf prog-1-jan-2005.tar.gz -C /tmp

$ cd /tmp

$ ls - 
