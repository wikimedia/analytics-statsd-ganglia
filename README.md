Ganglia backend for StatsD
==========================

ganglia.js is a plug-in for Etsy's StatsD that provides support for
writing statistics to Ganglia, a distributed monitoring system for
clusters and grids.

Prerequisites
-------------
  * StatsD: https://github.com/etsy/statsd/
  * Ganglia: http://ganglia.sourceforge.net/

Configuration
-------------
Place ganglia.js in StatsD's backends directory (most likely
/usr/share/statsd/backends). Then edit /etc/statsd/localConfig.js and
add a reference to the "backends" configuration key. For example:

    "backends": [ "./graphite", "./ganglia" ], 

The following configuration options are available:

    {
      "gangliaHost": "localhost",  // Hostname of Ganglia server (required)
      "gangliaPort": 8649,         // UDP port of Ganglia server (default: 8649)
      "gangliaMulticast": false,   // Use multicast? (default: no)
      "gangliaSpoofHost": "slave", // Associate metrics w/this hostname (default: unset)
      "gangliaGroup": "statsd",    // Default metric group name (default: 'statsd')
      "gangliaFilters": [],        // Array of module paths (see below)
    }

Metric filters
--------------

If you want to choose which metrics get sent to Ganglia, you may set
the "gangliaFilters" configuration to an array of module paths.
Each module should export a "filter" function which takes a metric
object. The function may modify the metric object or return false to
exclude it from Ganglia reporting. For example:

    exports.filter = function ( metric ) {
        // Exclude counters from Ganglia reporting.
        return /count/.test( metric.name ) ? false : metric;
    };

License & copyright
-------------------
Copyright (c) 2013 Wikimedia Foundation <info@wikimedia.org>
Copyright (c) 2013 Ori Livneh <ori@wikimedia.org>

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
"Software"), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND
NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE
LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION
OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION
WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
