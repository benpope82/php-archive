PHPArchive - Pure PHP ZIP and TAR handling
==========================================

This library allows to handle new ZIP and TAR archives without the need for any special PHP extensions (gz and bzip are
needed for compression). It can create new files or extract existing ones.

To keep things simple, the modification (adding or removing files) of existing archives is not supported.

Install
-------

Use composer:

```php composer.phar require splitbrain/PHPArchive```

Usage
-----

The usage for the Zip and Tar classes are basically the same. Here are some examples for working with TARs to get
you started. Check the source code comments for more info

```php
use splitbrain\PHPArchive\Tar;

// To list the contents of an existing TAR archive, open() it and use contents() on it:
$tar = new Tar();
$tar->open('myfile.tgz');
$toc = $tar->contents();
print_r($toc); // array of FileInfo objects

// To extract the contents of an existing TAR archive, open() it and use extract() on it:
$tar = new Tar();
$tar->open('myfile.tgz');
$tar->extract('/tmp');

// To create a new TAR archive directly on the filesystem (low memory requirements), create() it,
$tar = new Tar();
$tar->create('myfile.tgz');
$tar->addFile(...);
$tar->addData(...);
...
$tar->close();

// To create a TAR archive directly in memory, create() it, add*() files and then either save()
// or getData() it:
$tar = new Tar();
$tar->create();
$tar->addFile(...);
$tar->addData(...);
...
$tar->save('myfile.tgz'); // compresses and saves it
echo $tar->getArchive(Archive::COMPRESS_GZIP); // compresses and returns it
```

Differences between Tar and Zip: Tars are compressed as a whole while Zips compress each file individually. Therefore
```Zip``` accepts an additional compression level parameter in the ```addFile()``` and ```addData()``` functions.

The FileInfo class can be used to specify additional info like ownership or permissions when adding a file to
an archive. 