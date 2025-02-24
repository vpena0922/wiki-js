# Media Uploader

```yaml
version: "3.8"

services:
  media-uploader:
    image: media-uploader
    container_name: media-uploader
    ports:
      - "8009:8009"
    volumes:
      - /servers/media-uploader:/app
    environment:
      - PYTHONUNBUFFERED=1
```