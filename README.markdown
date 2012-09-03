S3NUKEM
=======

**s3nukem** is a slightly improved version of [s3nuke](http://github.com/SFEley/s3nuke/), a Ruby script by Steve Eley that relatively quickly deletes an Amazon Web Services (AWS) Simple Storage Service (S3) bucket with many objects (millions) by using multiple threads to retrieve and delete the individual objects.

In my use case, [s3cmd](http://s3tools.org/s3cmd), which deletes with a single thread, deleted objects at a rate of about 1,800/minute (2.5 million / day). s3nukem, with 10 delete threads deleted objects at a rate of about 9,000/minute (13 million / day). My task of deleting 99 million objects went from 40 days to 7.6 days. More threads and Ruby 1.9 (I was using 1.8.5) would have probably completed the job even more quickly.


Improvements
------------

* The key retrieval thread will pause when the queue contains `1000 * thread_count` items. The original script's queue would grow unabated, eating up memory unnecessarily.

* All output is automatically flushed, which ensures you can keep an eye on progress in real-time

* Added the number of seconds elapsed since the start of the script to output so you can calculate the rate at which items are being deleted.


Installation
------------
### You'll need:

* **Ruby**
    Ruby 1.9 should work faster because of the native thread implementation (on the other hand, network/S3 latency may be your biggest bottleneck).

* **right\_aws** gem; [dmarkow's version](http://github.com/dmarkow/right_aws) if you're running Ruby >= 1.9

        # Ruby < 1.9
        sudo gem install right_aws

        # Ruby >= 1.9
        sudo gem install dmarkow-right_aws --source http://gems.github.com

### Download and make executable; e.g.,

    # download
    wget https://raw.github.com/lathanh/s3nukem/master/s3nukem
    # or
    curl -O https://raw.github.com/lathanh/s3nukem/master/s3nukem

    # make executable
    chmod 755 s3nukem


Obvious Warning
---------------
This script is _intended_ to delete all of the items in an S3 bucket very quickly. You will not be prompted to ask you if you're sure. There is no undo.

Do not taunt Happy Fun Script.


License
-------
This script is released under the [Apache License, Version 2.0](http://www.apache.org/licenses/LICENSE-2.0). I really don't care what you do with it, so long as "sue me" is not on the agenda.


Credits
-------
Original script by [Steve Eley](http://extraneous.org/).

Improvements by [Robert LaThanh](http://robertlathanh.com/2010/07/s3nukem-delete-large-amazon-s3-buckets/).
