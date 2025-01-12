
Первый вариант решения - https://huecker.io/

Второй вариант решения (на линуксе):
```shell
sudo nano /etc/docker/daemon.json
```

``` json
{
    "registry-mirrors": ["https://mirror.gcr.io"]
}
```

```shell
sudo service docker restart
```

