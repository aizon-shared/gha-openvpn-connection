# VPN connection github composite action

GitHub Action for connecting to OpenVPN server.

## Inputs

Action connect:

| Name | Description | Required | Default |
| --- | --- | --- | --- |
| configFileContent | OpenVPN config file content | true | |
| privateKeyPassword | Private key password | true | |
| timeout | Timeout for connection in seconds | false | 30 |

Action disconnect:

| Name | Description | Required | Default |
| --- | --- | --- | --- |
| pid | PID of the openVPN process to kill | false | |

## Usage

```yaml
...

jobs:
  openvpn-connection:
    runs-on: ubuntu-latest
    steps:
      - name: Connect to OpenVPN
        id: connect
        uses: aizon-shared/gha-openvpn-connection/connect@V1
        with:
          configFileContent: ${{ secrets.VPN_CONFIG_FILE_CONTENT }}
          privateKeyPassword: ${{ secrets.VPN_PRIVATE_KEY_PASSWORD }}
      - name: echo outputs
        id: echo
        run: |
          echo "OpenVPN version: ${{ steps.connect.outputs.openvpn-version }}"
          echo "PID: ${{ steps.connect.outputs.pid }}"
          echo "Testing connection..."
          curl icanhazip.com
      - name: Disconnect from OpenVPN
        uses: aizon-shared/gha-openvpn-connection/disconnect@V1
        with:
          pid: ${{ steps.connect.outputs.pid }}
...

```

## License

This module is released under the Apache License Version 2.0:

* [http://www.apache.org/licenses/LICENSE-2.0.html](http://www.apache.org/licenses/LICENSE-2.0.html)
