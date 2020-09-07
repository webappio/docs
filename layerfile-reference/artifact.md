# ARTIFACT

`ARTIFACT [PRIVATE/PUBLIC] [file/directory...]`

The `ARTIFACT` instruction allows you to download files from the virtual machine running the test.

### Examples

- Use `ARTIFACT PUBLIC built/file.jar` to create a download link to the built jar file
- Use `ARTIFACT PRIVATE /var/log/buildlog` to create a download link to the logs for the run
