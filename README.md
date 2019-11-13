# influxdata
influxdata stack

## Backup influxDB database
```sh
docker exec -it ruuvi-influxdb influxd backup -portable -database ruuvi /tmp/backup_ex
```

#### Mount usb
```sh
ls -l /dev/disk/by-uuid/ <-- verify sda1 is available
sudo mkdir /media/usb
sudo chown -R pi:pi /media/usb
sudo mount /dev/sda1 /media/usb -o uid=pi,gid=pi

```

#### Copy database to usb
```sh
mkdir /media/usb/influxdb
docker cp ruuvi-influxdb:/tmp/backup_ex /media/usb/influxdb/
```

#### Unmount usb
```sh
sudo umount /media/usb <-- may require reboot to work
```

## Restore InfluxDB Database

#### Copy database to new server
```sh
Using winscp copy folder to new server (/tmp/backup_ex) 
```
#### Copy database into container
```sh
docker cp /tmp/backup_ex/ influxdata-influxdb:/tmp/
```

#### Restore Database
```sh
docker exec -it influxdata-influxdb influxd restore -portable -db ruuvi /tmp/backup_ex
```
