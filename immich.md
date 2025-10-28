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
