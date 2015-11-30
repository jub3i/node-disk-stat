disk-stat
=========

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

**Note:** This repo can be found on github here: [node-disk-stat](https://github.com/jub3i/node-disk-stat)

**Caveat:** Works by parsing `/proc/diskstats`, so will only work on nix OS.

**Note:** If you are interested in the amount of disk space used/free/etc check out [diskusage](https://www.npmjs.com/package/diskusage)

Install
-------

```
npm install disk-stat
```

Examples
--------

Require the module:
```
var diskStat = require('disk-stat');
```

By default `usageRead()` returns usage per second in bytes on device `sda`:
```
diskStat.usageRead(function(bytesPerSecond) {
  console.log(bytesPerSecond);
});
```

Specify some `opts` for `usageRead()` to customize return value, see docs below for more options:
```
diskStat.usageRead({
    device: 'sda1',
    units: 'MiB',
  },
  function(mbPerSecond) {
    console.log(mbPerSecond);
});
```

Get all fields available from `/proc/diskstats` formatted as an object:
```
var raw = diskStat.raw();
console.log(raw);
```

usageRead([opts,] cb)
---------------------

Async function which returns `data`, the disk usage for reads per second in `opts.units` on `opts.device` over `opts.sampleMs` assuming a sector size in bytes of `sectorSizeBytes`

Option               | Type         | Default       | Explanation
-------------------- | ------------ | ------------- | ------------
opts                 | `Object`     | see below     | Options object, specify what you need, the defaults will be filled in
opts.units           | `String`     | `'bytes'`     | The units of the returned value, can be one of `bytes`, `KiB`, `MiB` or `GiB`
opts.sectorSizeBytes | `Number`     | `512`         | The number of bytes in each sector on the `device`
opts.device          | `String`     | `'sda'`       | `device` is the device name to measure, see `raw()` for a list of device names
opts.sampleMs        | `String`     | `1000`        | `sampleMs` is the amount of time to take the measurement over
cb                   | `Function`   | none          | Callback which has signature `cb(usagePerSecond)`

**Note:** get system sector size in bytes: `sudo hdparm -I /dev/sda | grep Physical`

**Note:** get system sector size in bytes: `cat /sys/block/sda/queue/physical_block_size`

usageWrite([opts,] cb)
----------------------

Async function which returns `data`, the disk usage for writes per second in `opts.units` on `opts.device` over `opts.sampleMs` assuming a sector size in bytes of `sectorSizeBytes`

Option               | Type         | Default       | Explanation
-------------------- | ------------ | ------------- | ------------
opts                 | `Object`     | see below     | Options object, specify what you need, the defaults will be filled in
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
{
  ram0:
   { deviceNumber: '1',
     deviceNumberMinor: '0',
     readsCompleted: '0',
     readsMerged: '0',
     sectorsRead: '0',
     msReading: '0',
     writesCompleted: '0',
     writesMerged: '0',
     sectorsWritten: '0',
     msWriting: '0',
     iosPending: '0',
     msIo: '0',
     msWeightedIo: '0' },

  ...

  loop0:
   { deviceNumber: '7',
     deviceNumberMinor: '0',
     readsCompleted: '0',
     readsMerged: '0',
     sectorsRead: '0',
     msReading: '0',
     writesCompleted: '0',
     writesMerged: '0',
     sectorsWritten: '0',
     msWriting: '0',
     iosPending: '0',
     msIo: '0',
     msWeightedIo: '0' },

  ...

  sda:
   { deviceNumber: '8',
     deviceNumberMinor: '0',
     readsCompleted: '163027',
     readsMerged: '15815',
     sectorsRead: '12564320',
     msReading: '1662800',
     writesCompleted: '421527',
     writesMerged: '525281',
     sectorsWritten: '27001720',
     msWriting: '8517992',
     iosPending: '0',
     msIo: '1962780',
     msWeightedIo: '10180908' },
  sda1:
   { deviceNumber: '8',
     deviceNumberMinor: '1',
     readsCompleted: '114879',
     readsMerged: '15186',
     sectorsRead: '4628658',
     msReading: '1397120',
     writesCompleted: '390778',
     writesMerged: '483151',
     sectorsWritten: '25718648',
     msWriting: '8096212',
     iosPending: '0',
     msIo: '1755812',
     msWeightedIo: '9493500' },
  sda2:
   { deviceNumber: '8',
     deviceNumberMinor: '2',
     readsCompleted: '2',
     readsMerged: '0',
     sectorsRead: '4',
     msReading: '36',
     writesCompleted: '0',
     writesMerged: '0',
     sectorsWritten: '0',
     msWriting: '0',
     iosPending: '0',
     msIo: '36',
     msWeightedIo: '36' },
  sda5:
   { deviceNumber: '8',
     deviceNumberMinor: '5',
     readsCompleted: '237',
     readsMerged: '193',
     sectorsRead: '3440',
     msReading: '736',
     writesCompleted: '1329',
     writesMerged: '4130',
     sectorsWritten: '43680',
     msWriting: '9672',
     iosPending: '0',
     msIo: '2704',
     msWeightedIo: '10408' },
  sda6:
   { deviceNumber: '8',
     deviceNumberMinor: '6',
     readsCompleted: '47728',
     readsMerged: '436',
     sectorsRead: '7930770',
     msReading: '264080',
     writesCompleted: '22937',
     writesMerged: '38000',
     sectorsWritten: '1239392',
     msWriting: '392228',
     iosPending: '0',
     msIo: '266864',
     msWeightedIo: '656260' }
}
```

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

Other Stat Modules
------------------

- cpu-stat [npm](https://www.npmjs.com/package/cpu-stat) [git](https://github.com/jub3i/node-cpu-stat)
- net-stat [npm](https://www.npmjs.com/package/net-stat) [git](https://github.com/jub3i/node-net-stat)
- disk-stat [npm](https://www.npmjs.com/package/disk-stat) [git](https://github.com/jub3i/node-disk-stat)
- mem-stat [npm](https://www.npmjs.com/package/mem-stat) [git](https://github.com/jub3i/node-mem-stat)

**Note:** `net-stat`, `disk-stat`, `mem-stat` only work on nix platforms.

License
-------

MIT
