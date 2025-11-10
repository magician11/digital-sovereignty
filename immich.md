# Immich

Immich is a replacement for Google Photos or iCloud photos.

## Why would you use it?

1. Store your photos and videos on your own drives.
2. No more space restrictions. Just choose a big drive to store it all on. Terabyte drives are fairly cost effective now.

## Installing on a Raspberry Pi

I like to structure my services on my Pi by creating a folder in my home directory called `services`, with the name of the service underneath that.

So I install Immich to `/home/eminem/services/immich` (if your username was `eminem`)

Then follow [these instructions](https://docs.immich.app/install/docker-compose/).

## Media storage

You'll most likely be storing your data (photos, videos) on an external drive. So make sure the drive is mounted before you start Immich.

If Immich was auto-started after say a power failure, and the drive was not mounted yet, then shutdown Immich, mount the drive, then start Immich again.

## Managing Immich

Assuming you have stored your docker file to `/home/eminem/services/immich`

### Navigating around your Pi

To goto your Immich setup, type `cd /home/eminem/services/immich`

### To start Immich

Navigate to the Immich directory as above.

Type `docker compose up -d`

### To shutdown Immich

Navigate to the Immich directory as above.

Type `docker compose down`

## Troubleshooting

Here are some things to check if Immich is not working.

If there is a power failure, the Pi will turn off, then reboot, and Immich will create a temp fake mount point.

In your `.env` file for your Immich setup (which in this demo is located at `/home/eminem/services/immich`) you'll see two lines something like

```
UPLOAD_LOCATION=/media/snoop/dc66e7a9-bc0f-41e7-8b59-6ab91576455d/immich
DB_DATA_LOCATION=/media/snoop/dc66e7a9-bc0f-41e7-8b59-6ab91576455d/immich/immich_db
```

To view this file you can type

```
cd /home/snoop/services/immich

less .env
```

Then goto `/media/snoop` (replace *snoop* with whatever your username is). Then view the directory contents by typing `ls -al`

You'll see something like

```
drwxr-x---+ 12 root   root   4096 Oct 29 12:45 .
drwxr-xr-x   3 root   root   4096 Feb 14  2025 ..
drwxr-xr-x   8 merlin merlin 4096 Sep 16 15:11 dc66e7a9-bc0f-41e7-8b59-6ab91576455d
```

If you have more than one entry in there that is almost the same, shutdown Immich, unmount the drive, then view this directory again. You should now only see one. This you can archive, by typing `sudo mv dc66e7a9-bc0f-41e7-8b59-6ab91576455d backup` (replacing that UUID with your drive)

Then re-mount the drive, and you should see just the one mounted drive entry again.

Now you can start up Immich again.
