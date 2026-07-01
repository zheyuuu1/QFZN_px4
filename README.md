# QFZN PX4 Archive

This repository is used to publish the full QFZN PX4 source archive as GitHub Release assets.

The archive was created from the WSL source tree:

```text
/home/zzynb/code/PX4-Autopilot
```

It includes the `.git` directory and submodule metadata, so it is distributed as a tar archive instead of a normal Windows ZIP.

## Download

Open the latest GitHub Release and download all files named like:

```text
PX4-Autopilot-full-20260701-140420.tar.gz.part-000
PX4-Autopilot-full-20260701-140420.tar.gz.part-001
...
PX4-Autopilot-full-20260701-140420.release-manifest.txt
```

## Restore on Linux or WSL

Put all parts in the same directory, then run:

```bash
cat PX4-Autopilot-full-20260701-140420.tar.gz.part-* > PX4-Autopilot-full-20260701-140420.tar.gz
sha256sum -c PX4-Autopilot-full-20260701-140420.tar.gz.sha256
# or compare with the original SHA256 listed in the release manifest

tar -xzf PX4-Autopilot-full-20260701-140420.tar.gz
cd PX4-Autopilot
git status
```

Do not restore this project by copying through Windows Explorer from `\\wsl.localhost`; WSL/Linux tar extraction preserves symlinks, permissions, hidden Git data, and submodule metadata more reliably.
