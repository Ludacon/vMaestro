---
title: "Offload Strategy for Data Migration and TrueNAS Rebuild"
date: 2023-04-12T00:00:00Z
draft: false
---

In this project, we will be working on a data offload strategy for migrating data, rebuilding a TrueNAS host machine, and setting up a new pool configuration. The plan consists of the following steps:

1. Backup data locally and on cloud storage.
2. Set up a temporary host with 1x 18TB, 1x 8TB WD Red, and 1x 8TB SMR drive. Test and adjust SMR drive performance as needed. Reserve ~8TB for secondary acceptable loss material.
3. Migrate data from TrueNAS pools to the temporary host.
4. Wipe drives in TrueNAS host machine.
5. Plan and create new pool configuration with RAIDZ2 and 12 drives.
6. Rebuild TrueNAS host machine with the new configuration.
7. Migrate data back to the TrueNAS host machine.
8. Test, verify, and update documentation.

## Pool Information

### Pool 'bigger'

- Configuration: 2x RAIDZ1 vdevs with 3x 8TB drives each
- Total size: 32TB
- Used space: 10.47TB

### Pool 'big'

- Configuration: 1x RAIDZ1 vdev with 3x 18TB drives
- Total size: 36TB
- Used space: 27.91TB

### Pool 'biggestsmalls'

- Configuration: 1x RAIDZ1 vdev with 3x 18TB drives (Updated)
- Total size: 36TB
- Used space: 33.56TB

### Total used space

71.94TB

## Rebuild Options

We considered two options for the rebuild:

### Option 1: Two separate RAIDZ2 vdevs (6x 8TB and 6x 18TB)

Pros:
- Higher storage efficiency
- Easier upgrade process
- Higher overall performance

Cons:
- Complexity
- Less fault tolerance within a vdev

### Option 2: Single RAIDZ2 vdev (6x 8TB and 6x 18TB)

Pros:
- Simplified management
- Uniform fault tolerance

Cons:
- Wasted storage capacity
- Slower upgrade process
- Lower overall performance

## Final Plan with Option 2 (Single RAIDZ2 vdev)

1. Backup data locally and on cloud storage (iCloud and OneDrive) for non-replaceable and unacceptable loss files.
2. Set up a temporary offload host with the following drives:
   - 1x 18TB drive
   - 1x 8TB WD Red drive
   - 1x 8TB SMR drive (evaluate performance and adjust if necessary)
3. Reserve ~8TB for secondary acceptable loss material.
4. Available offload capacity: 26TB
5. Migrate up to 26TB of data from the TrueNAS pools to the temporary offload host.
6. Estimated transfer time: ~5.96 hours (based on a 10Gbps network connection)
7. Wipe all drives in the current TrueNAS host machine.
8. Plan and create a new pool configuration with RAIDZ2 and 12 drives (6x 8TB and 6x 18TB) in a single vdev.
