---
schema_version: "1.0.0"
document_id: "7ea3ce6ebe71cb0d3334aeb121d0835caab734e7f5ac52f2010f1f8f4debed03"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/uncovering-disk-io-bottlenecks-github-actions-ci"
published_at: "2024-09-30T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T22:01:06.812214+00:00"
content_hash: "sha256:763fe774a878002b34aa4aacf3a45b65184b92ba736a8d9a11ec9fd5d8dddc2b"
---

# Uncovering Disk I/O Bottlenecks in GitHub Actions

Disk I/O bottlenecks are easy to overlook when analyzing CI pipeline performance, but tools like[iostat](https://linux.die.net/man/1/iostat) and[fio](https://github.com/axboe/fio) can help shed a light on what might be slowing down your pipelines more than you know.


GitHub offers different hosted-runners with a range of specs, but for this test we are using the default` ubuntu-22.04` runner in a private repository, which does give us an additional 2 vCPUs but does not alter the disk performance.


## How to monitor disk performance


Getting a baseline benchmark from a tool like` fio` is useful for comparing the relative disk performance of different runners. However, to investigate if you are hitting disk I/O bottlenecks in your CI pipeline, it is more useful to monitor disk performance during the pipeline execution.


We can use a tool like` iostat` to monitor the disk while installing dependencies from the cache to see how much we are saturating the disk.


```text
-   name  :   Start IOPS Monitoring
run  :   |
echo "Starting IOPS monitoring"
# Start iostat in the background, logging IOPS every second to iostat.log
nohup iostat -dx 1 > iostat.log 2>&1 &
echo $! > iostat_pid.txt  # Save the iostat process ID to stop it later


-   uses  :   actions/cache@v4
timeout-minutes  :   5
id  :   cache-pnpm-store
with  :
path  :   ${{ steps.get-store-path.outputs.STORE_PATH }}
key  :   pnpm-store-${{ hashFiles('pnpm-lock.yaml') }}
restore-keys  :   |
pnpm-store-
pnpm-store-${{ hashFiles('pnpm-lock.yaml') }}


-   name  :   Stop IOPS Monitoring
run  :   |
echo "Stopping IOPS monitoring"
kill $(cat iostat_pid.txt)


-   name  :   Save IOPS Data
uses  :   actions/upload-artifact@v4
with  :
name  :   iops-log
path  :   iostat.log
```


### Monitoring disk during untar of Next.js dependencies


In the above test, we used` iostat` to monitor disk performance while the cache action downloaded and untarred the dependencies for` vercel/next.js` :


```text
Received   96468992   of   343934082   (28.0%), 91.1 MBs/sec
Received   281018368   of   343934082   (81.7%), 133.1 MBs/sec
Cache   Size:   ~328   MB   (343934082   B  )
/usr/bin/tar   -xf   /home/  <  pat  h  >  /cache.tzst   -P   -C   /home/  <  pat  h  >  /gha-disk-benchmark   --use-compress-program   unzstd
Received   343934082   of   343934082   (100.0%), 108.8 MBs/sec
Cache   restored   successfully
```


The full step took 12s to complete, and we can estimate the download took around 3s, leaving 9s for the untar operation.


The compressed tarball is only about 328MB, but after extraction, the total amount of data written to the disk is about 1.6GB. That smaller size got our cache across the network plenty fast, and most CPUs can handle decompression fast enough, meaning higher compression is often favorable. Once download and decompression are no longer the bottleneck, that leaves writing to disk.


Reading from a tarball is a fairly efficient process as it's mostly sequential reads, however, we then need to write each file to disk. This is where we can hit disk I/O bottlenecks, especially with a large number of small files.


It’s important to note that this is just a single run, not an average. Running multiple tests over time will give you a much clearer picture of the overall performance. Variance between runs can be quite high, so an individual bad run doesn’t necessarily indicate a problem.


What this run suggests is a possible throughput bottleneck. We’re seeing spikes in the maximum total throughput, with most hovering around ~220MB/s. This is likely the maximum throughput we are able to achieve to this disk, we'll verify this next. We should continue to monitor this and compare it to other runners to see if we can find an ideal runner for our workflow. We'll use` fio` to double-check if we are hitting the disk's maximum throughput.


An interesting aside before we move on, we can see from this side-by-side how relatively low read operations to writes there are. Since we’re reading from a tarball, most reads are sequential, which tends to be more efficient. That read data is likely going into a buffer before being written to the disk in a more random pattern as it creates a copy of each file. This is why we see a higher write IOPS than read IOPS.


### Maximum disk throughput


One of the first optimizations developers usually make to their CI pipelines is caching dependencies. Even though the cache still gets uploaded and downloaded with each run, it speeds things up by packaging all your dependencies into one compressed file. This skips the hassle of resolving dependencies, avoids multiple potentially slow downloads, and cuts down on network delays.


But as we saw above, network speed isn't usually our bottleneck when downloading the cache.


Test Type Block Size Bandwidth


**Read Throughput** 1024KiB ~209MB/s


**Write Throughput** 1024KiB ~209MB/s


Using` fio` to test our throughput, notice that both "read" and "write" throughput are both capped at the same value. This is a fairly telling sign that the limitation here is not actually the disk physically, but rather a bandwidth limit imposed by GitHub. This is a standard practice to divide up resources among multiple users who may be accessing the same physical disk from their virtual machines. It isn't always documented, but most providers will have higher bandwidth limits on higher tier runners.


What we measured here aligns fairly closely with the 220MB/s we saw in the untar test, giving us another hint that we are likely being slowed down during our dependency installation, not by the network or CPU, but by the disk.


Regardless of how fast our download speed is, we won't be able to write to disk any faster than our max throughput to the disk.


Uncompressed Cache Size


Disk Bandwidth


Estimated time to write to disk:


~9.52 seconds


Realistically, your disk performance will vary greatly depending on your specific cache size, the number of files, and just general build-to-build variance. That's why it's a good idea to monitor your CI runners for a consistent baseline, and we'll talk about testing your workflow on multiple runners for comparison.


### Maximum IOPS (Input/Output Operations Per Second)


After downloading the cache tarball, it needs to be extracted. Depending on the compression level it could be a CPU-intensive operation but this isn't usually a problem. When untar-ing the dependencies, we are performing a lot of small read and write operations, which is where we can hit disk I/O bottlenecks.


Test Type Block Size IOPS


**Read IOPS** 4096B ~51K


**Write IOPS** 4096B ~57K


**Random Read IOPS** 4096B ~9370


**Random Write IOPS** 4096B ~3290


IOPS is a measure of how many read/write operations can be performed in a second. When we have a lot of small files, like especially with a` node_modules` directory, it is possible to saturate the IOPS limit of the disk (or the imposed limit) and become a different kind of IO bottleneck.


Similarly to how we can't write to the disk any faster than the bandwidth limit, there is a limit to how many IOPS we can perform on the disk.


## Running benchmarks on different runners


If you are seeing bottlenecks in your CI pipeline, of any kind, we want to try to optimize for those issues with strategies like caching and parallelizing where possible. But we also need to know if we are hitting the limits of the runner we are using. It's easy enough to add a matrix strategy to your workflow to test on multiple runners for a quick comparison of speed of the same steps on different hardware.


```text
jobs  :
build  :
runs-on  :   ${{ matrix.runner }}
strategy  :
matrix  :
runner  : [  ubuntu-22.04  ,   depot-ubuntu-22.04  ]
```


To get a more detailed look at the specific disk performance of each runner, you can use the` fio` benchmarking tool we mentioned earlier. This will give you a better idea of the disk performance of each runner, and a reference point for checking for bottlenecks in your CI pipeline.


```text
-   name  :   Random Read Throughput Test
run  :   |
fio --ioengine=sync --bs=4k --rw=randread --name=random_read_throughput \
--direct=1 --filename=$HOME/fio_test/file --time_based --runtime=10s \
--size=250m --output=random_read_throughput_result-${{ matrix.runner }}.txt


-   name  :   Clean up Test Directory
run  :   rm -rf $HOME/fio_test/*


-   name  :   Random Write Throughput Test
run  :   |
fio --ioengine=sync --bs=4k --rw=randwrite --name=random_write_throughput \
--direct=1 --filename=$HOME/fio_test/file --time_based --runtime=10s \
--size=250m --output=random_write_throughput_result-${{ matrix.runner }}.txt


-   name  :   Clean up Test Directory
run  :   rm -rf $HOME/fio_test/*


-   name  :   Random Read IOPS Test
run  :   |
fio --name=random_read_iops --directory=$HOME/fio_test --size=5G \
--time_based --runtime=60s --ramp_time=2s --ioengine=libaio --direct=1 \
--verify=0 --bs=4K --iodepth=256 --rw=randread --group_reporting=1 \
--iodepth_batch_submit=256 --iodepth_batch_complete_max=256 \
--output=random_read_iops_result-${{ matrix.runner }}.txt


-   name  :   Clean up Test Directory
run  :   rm -rf $HOME/fio_test/*


-   name  :   Random Write IOPS Test
run  :   |
fio --name=random_write_iops --directory=$HOME/fio_test --size=5G \
--time_based --runtime=60s --ramp_time=2s --ioengine=libaio --direct=1 \
--verify=0 --bs=4K --iodepth=256 --rw=randwrite --group_reporting=1 \
--iodepth_batch_submit=256 --iodepth_batch_complete_max=256 \
--output=random_write_iops_result-${{ matrix.runner }}.txt
```


## Ultra-fast disk I/O with Depot Ultra Runner


Depot is launching a new runner type with ultra-fast disk I/O, the Depot Ultra Runner. The Ultra Runner utilizes a large RAM disk cache and higher-powered CPUs to maximize performance in both high IOPS and high throughput scenarios.


Want to be notified when the Depot Ultra Runner is available? Subscribe to our[changelog](https://depot.dev/changelog) for all major updates.


Try comparing your current workflow on a Depot runner. Sign up for our[7-day free trial](https://depot.dev/sign-up) and compare your CI pipeline performance on[Depot Runners](https://depot.dev/docs/github-actions/runner-types) with a matrix job.


Kyle Tryon
