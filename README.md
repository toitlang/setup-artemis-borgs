# Artemis Borgs
Instructions on how to set up a server with Artemis borgs.

## Setup
Create a new directory for each borg. Name each `pi-xx`, like
`pi-01`.

Use `artemis device extract --format=tar ...` to obtain
a tar that can be extracted and run in each directory.

Directories should have a `boot.sh` script in their root.

## Service
Copy the `artemis-borg@.service.template` to `artemis-borg@.service` and
adjust it (replacing the username and potentially the path).

Copy the `artemis-borg@.service` file to `/etc/systemd/system/`

```bash
sudo cp artemis-borg@.service /etc/systemd/system/
```

Reload the systemd units.
```bash
sudo systemctl daemon-reload
```

Enable and start all services.
```
for dir in pi-*; do sudo systemctl enable artemis-borg@$dir.service; done
for dir in pi-*; do sudo systemctl start artemis-borg@$dir.service; done
```

Verify that it is correctly running:
```bash
systemctl status artemis-borg@pi-01
```

Or use journalctl:
```
journalctl -u artemis-borg@pi-01.service -f
```
