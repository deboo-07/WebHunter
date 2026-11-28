# Local OWASP Juice Shop setup for active lab work

Use a local instance for Weeks 2–4 unless the exact external target and test methods are explicitly authorized. The upstream Juice Shop repository documents the following Docker run command, bound to localhost only.

```sh
docker pull bkimminich/juice-shop
docker run --rm -p 127.0.0.1:3000:3000 bkimminich/juice-shop
```

Open `http://127.0.0.1:3000/` in the lab browser. Keep Burp scope limited to `127.0.0.1:3000`. Stop the foreground container with `Ctrl+C` when the lab is finished. Do not change the port binding to `0.0.0.0` or expose the training app publicly.

If Docker is unavailable, use another officially documented local setup path from the [OWASP Juice Shop running guide](https://pwning.owasp-juice.shop/companion-guide/latest/part1/running.html). Follow the current upstream requirements for the selected method.
