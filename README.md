# GitHub Action — Deploy SSL certificate to Tencent Cloud

## Usage

### Example Workflow

```yaml
jobs:
  deploy-to-tencent-cloud:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
        with:
          # If you just commited and pushed your newly issued certificate to this repo in a previous job,
          # use `ref` to make sure checking out the newest commit in this job
          ref: ${{ github.ref }}

      - name: Deploy cert to Tencent Cloud
        uses: linrongda/deploy-certificate-to-tencent-cloud@v2
        with:
          secret-id: ${{ secrets.TENCENTCLOUD_SECRET_ID }}
          secret-key: ${{ secrets.TENCENTCLOUD_SECRET_KEY }}
          fullchain-file: ${{ env.FILE_FULLCHAIN }}
          key-file: ${{ env.FILE_KEY }}
          # each domain represent an old certificate
          domains: |
            cdn1.example.com cdn2.example.com
            zone-XXXX eo1.example.com eo2.example.com
```

### Tips:

Each line contains domains separated by spaces.

If a line begins with a Zone ID (starts with `zone-`) the remainder of that line are EdgeOne targets for that zone.
Otherwise tokens are treated as CDN domains.

## Permissions

Ensure the API credentials have permissions for:

- `QcloudSSLFullAccess`
- `QcloudCDNFullAccess` (If used)
- `QcloudTEOFullAccess` (If used)
