s3ql-backup
===========
Automatic cloud backup over s3ql.

Usage
-----
(Approximate.)

```bash
# generate a random passphrase
openssl rand -hex 32 # copy that into clipboard

# setup backend auth (see http://www.rath.org/s3ql-docs/authinfo.html )
mkdir -p ~/.s3ql
nano ~/.s3ql/authinfo2 # put the passphrase in there too
chmod 600 ~/.s3ql/authinfo2

# config backup url
mkdir -p ~/.config/s3ql-backup
echo STORAGE_URL=s3c://whatever.example.com/bucket/prefix > ~/.config/s3ql-backup/s3ql-backup.conf

# create the fs
. ~/.config/s3ql-backup/s3ql-backup.conf
mkfs.s3ql "$STORAGE_URL" # paste the passphrase, twice

# test
~/.bin/s3ql-backup ~/pr0n
find ~/.s3ql-backup

# setup the backup
incrontab -e
# for each backed up directory, add eg.:
# /home/myself/pr0n IN_CREATE,IN_DELETE,IN_CLOSE_WRITE,IN_MOVE,IN_NO_LOOP /home/myself/.bin/s3ql-backup $@

# test again
touch ~/pr0n/wiener
find ~/.s3ql-backup
```
