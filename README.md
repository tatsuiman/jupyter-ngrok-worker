# jupyter-ngrok-worker


```bash
cp env.sample .env
vim .env
vim docker/nginx/nginx.conf
docker-compose up -d --build
```

## update whitelist
```bash
curl -s https://ip-ranges.amazonaws.com/ip-ranges.json | jq -r '.prefixes[] | select(.region=="ap-northeast-1") | .ip_prefix' | sort -n | sed 's/^/allow /; s/$/;/g' |pbcopy
```
