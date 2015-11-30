disk-stat
========

```
  ______   ___ _______ ___ ___  _______ _______ _______ _______
 |   _  \ |   |   _   |   Y   )|   _   |       |   _   |       |
 |.  |   \|.  |   1___|.  1  / |   1___|.|   | |.  1   |.|   | |
 |.  |    |.  |____   |.  _  \ |____   `-|.  |-|.  _   `-|.  |-'
 |:  1    |:  |:  1   |:  |   \|:  1   | |:  | |:  |   | |:  |
 |::.. . /|::.|::.. . |::.| .  |::.. . | |::.| |::.|:. | |::.|
 `------' `---`-------`--- ---'`-------' `---' `--- ---' `---'
```

**Note:** This repo can be found on npm here: [disk-stat](https://www.npmjs.com/package/disk-stat)

**Caveat:** Works by parsing `/proc/diskstats`, so will only work on nix OS.

Install
-------

```
npm install disk-stat
```

Example
-------

```
var diskStat = require('disk-stat');

//by default returns usage per second in bytes on device `sda`
diskStat.usageRead(function(bytesPerSecond) {
  console.log(bytesPerSecond);
});

//specify some options to customize return value
//see below for more options
diskStat.usageRead({
    device: 'sda1',
    units: 'MiB',
  },
  function(mbPerSecond) {
    console.log(mbPerSecond);
});

//get all fields available from `/proc/diskstats` formatted as an object.
var raw = diskStat.raw();
console.log(raw);
```

usageRead(opts, cb)
-------------------

Async function which returns `data`, the disk usage for reads per second in `opts.units` on `opts.device` over `opts.sampleMs` assuming a sector size in bytes of `sectorSizeBytes`

Option               | Type         | Default       | Explanation
-------------------- | ------------ | ------------- | ------------
opts                 | `Object`     | see below     | Options object, specify what you need the defaults will be filled in
opts.units           | `String`     | `'bytes'`     | The units of the returned value, can be one of `bytes`, `KiB`, `MiB` or `GiB`
opts.sectorSizeBytes | `Number`     | `512`         | The number of bytes in each sector on the `device`
opts.device          | `String`     | `'sda'`       | `device` is the device name to measure, see `raw()` for a list of device names
opts.sampleMs        | `String`     | `1000`        | `sampleMs` is the amount of time to take the measurement over
cb                   | `Function`   | none          | Callback which has signature `cb(usagePerSecond)`

**Note:** get system sector size in bytes: `sudo hdparm -I /dev/sda | grep Physical`

**Note:** get system sector size in bytes: `cat /sys/block/sda/queue/physical_block_size`

usageWrite(opts, cb)
--------------------

Async function which returns `data`, the disk usage for reads per second in `opts.units` on `opts.device` over `opts.sampleMs` assuming a sector size in bytes of `sectorSizeBytes`

Option               | Type         | Default       | Explanation
-------------------- | ------------ | ------------- | ------------
opts                 | `Object`     | see below     | Options object, specify what you need the defaults will be filled in
opts.units           | `String`     | `'bytes'`     | The units of the returned value, can be one of `bytes`, `KiB`, `MiB` or `GiB`
opts.sectorSizeBytes | `Number`     | `512`         | The number of bytes in each sector on the `device`
opts.device          | `String`     | `'sda'`       | `device` is the device name to measure, see `raw()` for a list of device names
opts.sampleMs        | `String`     | `1000`        | `sampleMs` is the amount of time to take the measurement over
cb                   | `Function`   | none          | Callback which has signature `cb(usagePerSecond)`

**Note:** get system sector size: `sudo hdparm -I /dev/sda | grep Physical`

**Note:** get system sector size: `cat /sys/block/sda/queue/physical_block_size`

raw()
-----

Returns an object representing the data in `/proc/diskstats`.

```
  /proc/diskstats

  https://www.kernel.org/doc/Documentation/iostats.txt

  Each set of stats only applies to the indicated device; if you want
  system-wide stats you'll have to find all the devices and sum them all up.

  Field  1 -- # of reads completed
      This is the total number of reads completed successfully.
  Field  2 -- # of reads merged, field 6 -- # of writes merged
      Reads and writes which are adjacent to each other may be merged for
      efficiency.  Thus two 4K reads may become one 8K read before it is
      ultimately handed to the disk, and so it will be counted (and queued)
      as only one I/O.  This field lets you know how often this was done.
  Field  3 -- # of sectors read
      This is the total number of sectors read successfully.
  Field  4 -- # of milliseconds spent reading
      This is the total number of milliseconds spent by all reads (as
      measured from __make_request() to end_that_request_last()).
  Field  5 -- # of writes completed
      This is the total number of writes completed successfully.
  Field  6 -- # of writes merged
      See the description of field 2.
  Field  7 -- # of sectors written
      This is the total number of sectors written successfully.
  Field  8 -- # of milliseconds spent writing
      This is the total number of milliseconds spent by all writes (as
      measured from __make_request() to end_that_request_last()).
  Field  9 -- # of I/Os currently in progress
      The only field that should go to zero. Incremented as requests are
      given to appropriate struct request_queue and decremented as they finish.
  Field 10 -- # of milliseconds spent doing I/Os
      This field increases so long as field 9 is nonzero.
  Field 11 -- weighted # of milliseconds spent doing I/Os
      This field is incremented at each I/O start, I/O completion, I/O
      merge, or read of these stats by the number of I/Os in progress
      (field 9) times the number of milliseconds spent doing I/O since the
      last update of this field.  This can provide an easy measure of both
      I/O completion time and the backlog that may be accumulating.
```

Contributing
------------

Just send a PR, or create an issue if you are not sure.

Areas ripe for contribution:
- testing
- cross compatability for windoze and darwin/osx
- performance
- bugs

License
-------

MIT

