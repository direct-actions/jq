# actions-jq
Simple and robust jq action offering updated jq versions, YAML input/output conversions, input from command or text & more!

# Usage
## Simple usage
The default behavior will take input from text or a command, run a filter, and
make the output available to other steps.
```
- id: jq
  uses: direct-actions/jq@v0.1.0
  with:
    filter: { user: .login }
    input: ${{ github.event.pull_request.head.user }}
- run: |
    echo "The user's login is ${{ fromJSON(steps.jq.outputs.output).user }}"
```
## Specifying jq arguments
Additional jq flags can be specified by using jq argument long names as input
names.
```
- uses: direct-actions/jq@v0.1.0
  with:
    filter: '"The user's login is \(.login)"'
    input: ${{ github.event.pull_request.head.user }}
    raw-output: true
```
While I prefer the long name syntax as it is self-documenting, you can also
use the short argument syntax.
```
- uses: direct-actions/jq@v0.1.0
  with:
    arguments -r
    filter: '"The user's login is \(.login)"'
    input: ${{ github.event.pull_request.head.user }}
```
## Using YAML input/output conversion
```
- uses: direct-actions/jq@v0.1.0
  with:
    filter: |
      .example_key |
      length
    input: |
      example_key:
        - a
        - b
        - c
    yaml-input: true
    yaml-output: true
```
## Input commands or files
Commands can be used for input.
```
- uses: direct-actions/jq@v0.1.0
  with:
    input_command: /tmp/dump_json.sh
    filter: |
      map(
        select(.included != 'false')
      ) |
      length
```
As can files.
```
- uses: direct-actions/jq@v0.1.0
  with:
    input_files: /tmp/one.json /tmp/two.json
    filter: |
      map(
        select(.included != 'false')
      ) |
      length
```
