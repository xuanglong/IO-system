# Лабораторная работа 2

## Описание функциональности драйвера

Use this utility to format disk partitions, create virtual disks, and test data transmission speed

Create a 50MB virtual disk, formatted as 25MB, 15MB, 10MB

## Use

1. sudo insmod  main.ko
2. lsblk
3. mkfs -t ext3 /dev/mydisk 1 5 6
4. mkdir m1 m5 m6
5. mount /dev/mydisk1 m1
6. echo "     " > xxx.txt 
7. dd if=xxx.txt of=/m1 bs=8k count=256k
8. cd m1
9. echo "   " >xxx.txt
10. dd if=xxx.txt of=/m2 bs=8k count=256k

## test 
1. write test program
2. Read and write the 
in the test program.
3. gcc test.c; ./a.out

## test program  code 
1) Using register_ Blkdev() creates a block device
2) blk_ init_ Queue () allocates an application queue using and assigns the application queue processing function
3) Use alloc_ Disk() allocates a gendisk structure
4) Sets the members of the gendisk structure
->4.1) set member parameters (major, first_minor, disk_name, FOPS)
->4.2) set the queue member to be equal to the previously allocated application queue
->4.3) pass set_ Capacity() sets the capacity member equal to the number of sectors
5) Use kzalloc () to get the cache address and use it as the sector
6) Use add_ Disk() registers the gendisk structure
3.2 in the processing function of application queue
1) While recycling ELV_ next_ Request() gets each unprocessed request in the request queue
2) Use RQ_ data_ Dir() to obtain the read-write command flag of each application. 0 (read) indicates read and 1 (write) indicates write
3) Use memcp () to read or write sectors (CACHE)
4) Use end_ Request() to end each request obtained
3.3 in exit function
1) Use put_ Disk () and del_ Gendisk() to log out and release the gendisk structure
2) Use kfree() to free the disk sector cache
3) Using BLK_ cleanup_ Queue() clears the application queue in memory
4) Using unregister_ Blkdev () unload block device