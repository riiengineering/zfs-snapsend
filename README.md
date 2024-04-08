# zfs-snapsend

A simple, POSIX shell script to manage automatic ZFS snapshots and syncing of
these to other zpools or remote systems (send/recv).


## Installation

Download the `zfs-snapsend` script from this repository and install it to a
directory of your choice (e.g. `/usr/local/sbin`) or into `/etc/cron.hourly` for
automatic cron job configuration.

```sh
curl -O /usr/local/sbin/zfs-snapsend https://github.com/riiengineering/zfs-snapsend/raw/main/zfs-snapsend
chown 0:0 /usr/local/sbin/zfs-snapsend
chmod 0750 /usr/local/sbin/zfs-snapsend
```


## Usage

The configuration of zfs-snapsend works by setting ZFS properties on the
datasets you wish to have snapshotted or synced.
All properties are applied recursively to child datasets, too.

**Supported properties:**

<dl>
  <dt><code>ch.riiengineering:auto-snapshot</code> = <code>never|hourly|daily|weekly|monthly|yearly</code></dt>
  <dd>
    <p>How often to create a fresh snapshot.</p>
    <p><b>NOTE:</b> Snapshots have a hysteresis of 30 minutes.</p>
    <p><b>NOTE:</b> This script must be run with a higher frequency than the
                     most frequent auto-snapshot.</p>
  </dd>

  <dt><code>ch.riiengineering:auto-snapshots-keep</code> = <code>all|n>0</code></dt>
  <dd>
    <p>Either keep <tt>all</tt> or the last <tt>n</tt> snapshots.</p>
  </dd>

  <dt><code>ch.riiengineering:auto-sync-to</code> = <code>[[user@]host:]pool[/dataset]</code></dt>
  <dd>
    <p>Sync (all) snapshots of this dataset to another zpool (either local or to
       a remote system via SSH).</p>
    <p>Only snapshots created since the last sync are added on the target,
       i.e. snapshots deleted on the remote system will not be restored unless
       they were the latest one(s).</p>
    <p>For syncing to a remote system, the <tt>ssh_config</tt> files are
       respected and the <tt>zfs</tt> binary in the default <tt>PATH</tt> is
       used.</p>
  </dd>
</dl>


-----
[![riiengineered.](https://static.riiengineering.ch/riiengineered-400.png)](https://www.riiengineering.ch)
