# Examples

## Create an instance

gnt-instance add -n vm1 -o noop \
	--no-start --no-install --no-name-check --no-ip-check -t ext \
	--disk 0:size=10G,provider=rbd-ssd,access=userspace instance_name

## Mix and match ext. storage providers

gnt-instance add -n vm1 -o noop \
	--no-start --no-install --no-name-check --no-ip-check -t ext \
	--disk 0:size=1G,provider=rbd-ssd,access=userspace \
	--disk 1:size=1G,provider=zfs instance_name

Note: failover will work in one direction, strangely enough, as
the ext storage provider doesn't verify the presence of the volume
on the target node. It fails on the way back, however, as it fails
to detach the source disk. Workaround: create a disk/volume manually
with the same name on the source node, failover, delete the disk.

TODO: for EC, we need to explicitly specify the backing pool as SSD:

	rbd create --size 1G --data-pool ec_pool replicated_pool/image_name

Until then, the data pool is hardcoded (see common/rados.py) at image
creation time.
