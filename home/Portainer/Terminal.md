# Terminal

```yaml
version: "3.9"  # Or your preferred docker-compose version

services:
  terminal:
    image: ubuntu-python
    container_name: terminal
    ports:
      - "2209:22"
    volumes:
      - /servers/terminal:/files_from_host
    # The --rm and -it flags are typically handled by docker-compose
    # --rm:  Docker Compose will automatically remove the container when it's stopped (unless you define a volume).
    # -it:  For interactive use, you would typically use `docker-compose exec terminal bash` (or sh) to get a shell inside the running container.
```